# Building the APK / Сборка APK

An APK is not stored in this repository. Building Telegram requires a desktop
machine: **SDK 36**, **build-tools 36.0.0**, **NDK 27.2.12479018** and **JDK 21**.
It cannot be built on an Android phone inside MargyCode: the Android SDK, NDK,
Gradle and the Telegram source tree are not available there.

## Option 1 — GitHub Actions (no local SDK)

1. Fork `andreyovciev-cpu/HayoGram` on GitHub.
2. In the fork: **Settings → Secrets and variables → Actions → New repository
   secret**, add:
   - `HAYO_APP_ID` — your API id from https://my.telegram.org
   - `HAYO_APP_HASH` — your API hash
3. Open **Actions → Build APK → Run workflow**.
4. When the run finishes, download the APK from **Artifacts**.

Optionally add signing secrets `HAYO_STORE_FILE_BASE64`, `HAYO_STORE_PASSWORD`,
`HAYO_KEY_ALIAS`, `HAYO_KEY_PASSWORD`. Without them the build falls back to the
public release key from Telegram's own source tree — fine for a test build, but
it cannot replace an installed Hayo Gram signed with your real key.

## Option 2 — local build

```bash
git clone https://github.com/DrKLO/Telegram
cd Telegram
git checkout $(cat ../HayoGram/patch/UPSTREAM)
git submodule update --init --recursive
git apply ../HayoGram/patch/hayo.patch

# Telegram's prebuilt libprisma.a is older than its own code
(cd TMessagesProj/jni/prebuild && ABIS=arm64-v8a bash build_prisma.sh)
```

Create `local.properties` in the Telegram root:

```properties
sdk.dir=/path/to/Android/Sdk
ndk.dir=/path/to/Android/Sdk/ndk/27.2.12479018
HAYO_APP_ID=123456
HAYO_APP_HASH=your_api_hash
# optional signing:
# HAYO_STORE_FILE=/path/to/release.jks
# HAYO_STORE_PASSWORD=...
# HAYO_KEY_ALIAS=...
# HAYO_KEY_PASSWORD=...
```

Then:

```bash
gradle :TMessagesProj_AppStandalone:assembleAfatStandalone
```

The APK appears in
`TMessagesProj_AppStandalone/build/outputs/apk/afat/standalone/`.

## Release

After building, put the APK under a GitHub release named `v2.0` with the asset
name `HayoGram-2.0.apk`, then publish `version.json` **last** — that is the
signal that makes clients show the update bar. See [docs/release.md](docs/release.md).
