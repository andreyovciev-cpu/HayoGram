# Сборка APK

Готового APK в репозитории нет. Для сборки Telegram нужен компьютер: **SDK 36**,
**build-tools 36.0.0**, **NDK 27.2.12479018** и **JDK 21**. На телефоне внутри
MargyCode собрать нельзя — там нет Android SDK, NDK, Gradle и исходников
Telegram.

## Способ 1 — GitHub Actions (SDK не нужен)

1. Сделайте форк `andreyovciev-cpu/HayoGram` на GitHub (или создайте репозиторий и
   загрузите сюда файлы).
2. В форке: **Settings → Secrets and variables → Actions → New repository
   secret**, добавьте:
   - `HAYO_APP_ID` — api_id с https://my.telegram.org
   - `HAYO_APP_HASH` — api_hash оттуда же
3. Откройте **Actions → Build APK → Run workflow**.
4. Когда запуск закончится, скачайте APK из **Artifacts**.

Необязательно: `HAYO_STORE_FILE_BASE64`, `HAYO_STORE_PASSWORD`,
`HAYO_KEY_ALIAS`, `HAYO_KEY_PASSWORD` — ваш ключ подписи. Без них сборка
подпишется публичным ключом из исходников Telegram: для пробы годится, но
поверх установленной Hayo Gram с вашим ключом такой APK не встанет.

## Способ 2 — свой компьютер

```bash
git clone https://github.com/DrKLO/Telegram
cd Telegram
git checkout $(cat ../HayoGram/patch/UPSTREAM)
git submodule update --init --recursive
git apply ../HayoGram/patch/hayo.patch

# Готовая libprisma.a в исходниках старше самого кода — пересобираем
(cd TMessagesProj/jni/prebuild && ABIS=arm64-v8a bash build_prisma.sh)
```

Создайте `local.properties` в корне Telegram:

```properties
sdk.dir=/path/to/Android/Sdk
ndk.dir=/path/to/Android/Sdk/ndk/27.2.12479018
HAYO_APP_ID=123456
HAYO_APP_HASH=your_api_hash
# подпись, если есть:
# HAYO_STORE_FILE=/path/to/release.jks
# HAYO_STORE_PASSWORD=...
# HAYO_KEY_ALIAS=...
# HAYO_KEY_PASSWORD=...
```

Затем:

```bash
gradle :TMessagesProj_AppStandalone:assembleAfatStandalone
```

APK появится в
`TMessagesProj_AppStandalone/build/outputs/apk/afat/standalone/`.

## Выпуск

После сборки залейте APK в релиз GitHub `v2.0` с именем файла
`HayoGram-2.0.apk` и **только потом** опубликуйте `version.json` — это сигнал,
после которого клиенты показывают плашку обновления. Подробности в
[docs/release.md](docs/release.md).
