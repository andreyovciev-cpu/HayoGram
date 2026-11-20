# Hayo Gram

**A Telegram fork for Android by [Hayozimi](https://t.me/hayozimi).**

Rebranded fork of [MargyGram](https://git.narez.xyz/narezany/MargyGram). Package
`cat.hayozimi.hayogram`, so it installs next to official Telegram rather than
over it. Built from [DrKLO/Telegram](https://github.com/DrKLO/Telegram) with the
single patch in `patch/hayo.patch`.

[Русский](README.ru.md) · [Badges](docs/badges.md) · [Build](BUILD.md) · [Security notes](docs/security.md)

## Owner badges

Two exclusive badges are built in and shipped in [`badges.json`](badges.json):

| Username | Badge | Colour |
|---|---|---|
| `@hayozimi` | **Owner** ("Владелец") | green, crown |
| `@zarrokk` | **Owner** ("Владелец") | lavender, star |

Matching is by **username**, not by numeric id, so the badges keep working
without hard-coding anybody's private account number. The same two entries are
also compiled into the app as an offline fallback. Badges show in the chat list
and profiles; tapping one opens a window with the icon.

The badge is decoration, not proof of identity. It certifies nothing.

## What changed from MargyGram

- Every `Margy`/`MargyGram` name is now `Hayo`/`Hayo Gram`; package and signing
  keys are `cat.hayozimi.hayogram` / `HAYO_*`.
- Author, channel and feedback links point to `@hayozimi`; source links point
  to this repository.
- The original author's payment details, donor links and servers were removed,
  not carried over. The donate screen no longer offers any payment method.
- `badges.json` contains only the two owner badges and supports `username` and
  `icon` (`crown` / `star`) in addition to the old numeric `peer`.
- Third-party OtherGram badge lookup, which uploads peers' numeric ids to a
  third-party server, is **off by default**.
- Remote files (badges, version) are read from this repository on GitHub.

Everything else from the upstream fork (plugins, music tab, streamer mode,
themes, etc.) is kept. See [ATTRIBUTION.md](ATTRIBUTION.md).

## Building

An APK is **not** included in this repository. Telegram needs SDK 36,
build-tools 36.0.0, NDK 27.2.12479018 and JDK 21.

Two ways:

1. **GitHub Actions** — fork this repository, add your own `HAYO_APP_ID` and
   `HAYO_APP_HASH` as repository secrets (get them at my.telegram.org), then run
   the *Build APK* workflow. The APK is attached to the run as an artifact.
2. **Your computer** — follow [BUILD.md](BUILD.md).

Own API credentials are not committed. The default public Telegram credentials
may work for a test build but are not suitable for a release.

## Safety

There is **no sandbox for plugins**. A plugin runs inside the app and can read
chats, send messages and access files. Install only plugins whose source you
have read. See [docs/security.md](docs/security.md) for the full static audit
of the patch.

## License

Telegram and this fork are GPL-2.0-or-later. If you distribute an APK, publish
the source: this repository plus the commit in `patch/UPSTREAM`.
