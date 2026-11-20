# Static audit of the patch / Проверка патча

Scope: the full text of `patch/hayo.patch` (69k lines, 381 files) and the
repository files. The upstream Telegram source tree is **not** part of this
repository, so this is a review of everything the patch adds or changes, not a
formal security audit of a compiled APK.

## What was checked

- **Added file types.** 193 `.java`, 110 `.xml`, 17 `.py`, plus gradle/properties.
  No `.so`, `.dex`, `.jar`, `.apk` or executable files are added. The 54 binary
  blobs in the patch are images, sounds and one example `.marp` archive.
- **Permissions.** The patch adds no new Android permissions.
- **Network destinations.** Every literal HTTP(S) endpoint in the patch:
  - this repository (`raw.githubusercontent.com/andreyovciev-cpu/HayoGram`) — badges,
    version, plugin docs;
  - Telegram links (`t.me/hayozimi`, `t.me/zarrokk`);
  - public services for user-initiated features: `genius.com` and `lrclib.net`
    (lyrics), `music.youtube.com` / `i.ytimg.com` (music search and covers);
  - `othergram-backend.othergram.workers.dev` — third-party badge lookup.
  No unknown telemetry or exfiltration endpoint was found.
- **Sensitive APIs.** No SMS, contacts, location, account or device-id reads.
  `TelephonyManager.getSimCountryIso()` is used only to decide whether to offer
  a proxy, and that offer is disabled. `Runtime.exit(0)` is used only by an
  explicit "restart the app" action.
- **Dynamic code / hooks.** The upstream plugin engine (Pine + Xposed API +
  Chaquopy Python + dexmaker) is present. It is a feature, not hidden code, but
  see the risk below.

## Remediations made in this fork

1. Removed the previous author's payment details, donation pages, gift user id
   and servers from `MargeletConfig`, the donate screen and the strings.
   The donate screen now shows only a thank-you note and has no payment rows.
2. Replaced all old host URLs with this repository. `MargeletRemote.BASE` reads
   `badges.json` and `version.json` from GitHub raw.
3. The profile helper bot and the tilcayo easter-egg list now use
   `MargeletConfig.SERVER_URL`, which is empty: they stay offline rather than
   contacting somebody else's server.
4. OtherGram badge lookup sends the numeric ids of people whose names are
   drawn to a third-party server. It is now **off by default**; a person has to
   enable it in settings deliberately.

## Remaining risks

- **Plugins are unsandboxed.** A plugin runs with the app's rights: it can read
  chats, send messages and access files. Manifest permissions are not enforced.
  Never install a plugin whose source you have not read.
- **Binary assets** (images/audio) were inherited from the upstream fork. Their
  text contents cannot be inspected in a patch; they are media files, but a
  compiled APK should still be scanned with Play Protect / VirusTotal before
  wide distribution.
- This fork uses its own signing key and its own `api_id`/`api_hash`. Do not
  publish a build made with Telegram's public test credentials.

## Conclusion

No malicious code was found in the patch text. The two things that could hurt a
user — unsandboxed plugins and third-party badge lookups — are documented, and
the latter is disabled by default.
