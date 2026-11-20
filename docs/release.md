# Cutting a release / Выпуск

The client checks `version.json` and offers to download the APK. Order matters:

1. Raise `MargeletConfig.APP_VERSION` in the patch and rebuild.
2. Upload the APK to a GitHub release. Use a **versioned asset name**, e.g.
   `HayoGram-2.0.apk` under tag `v2.0`.
3. Only now update [`version.json`](../version.json) — both `version` and the
   `apk` URL. This is the announcement: as soon as the number goes up, every
   client shows the update bar.

Do not point the updater at a permanent `latest` URL. GitHub caches raw files
for five minutes, so a permanent name can serve the previous build with a 200.
A versioned name has never been cached.

`version.json` fields:

| Field | Meaning |
|---|---|
| `version` | Compared as numbers separated by dots (`0.10` > `0.9`). |
| `apk` | Full download URL with the version in it. |
| `about`, `about_ru`, `about_zh` | One line of release notes. |

Nothing is downloaded or installed without a tap. A partial download is never
offered for installation.

The client asks every three minutes by default; each user can change that in
fork settings or switch automatic checks off. **Check now** always works.
