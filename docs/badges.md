# Badges / Значки

A badge is the small mark next to a name in the chat list and in a profile.
It is decoration, not proof of anything.

## The file

[`badges.json`](../badges.json) in the `main` branch. The app re-reads it on
startup and caches the last good copy. The same two owner entries are compiled
into `MargeletBadge.BUILT_IN`, so a fresh install without network still shows
them.

```json
{
  "id": "owner_hayozimi",
  "username": "hayozimi",
  "title": "Owner",
  "title_ru": "Владелец",
  "about": "One of the owners of Hayo Gram.",
  "color": "8DD1B0",
  "icon": "crown",
  "url": "https://t.me/hayozimi"
}
```

| Field | Meaning |
|---|---|
| `id` | Stable id used for ordering and hidden badges. Do not change it for an existing badge. |
| `username` | Telegram username without `@`. This is how the two owner badges are matched; numeric ids are not compiled in. |
| `peer` | Old style: numeric user id (channels/groups use a negative id). Either `peer` or `username` is required. |
| `title`, `about` | Shown next to the name and in the badge window. Add `_ru` / `_zh` for translations. |
| `color` | Field colour, hex: `8DD1B0`, `#8DD1B0` and `FF8DD1B0` all work. |
| `icon` | Optional: `crown` or `star`. Drawn as a white vector on the coloured field. When absent, the legacy plane icon is used. |
| `url` | Where the window button leads. Omit it to hide the button. |

Order is seniority: the first matching badge is the one shown next to the name;
the profile shows all of them. `list()` de-duplicates by `id`, not by title, so
two owners can share the title **Owner / Владелец** and still both appear in
the gallery.

If a line cannot be parsed it is skipped. If the whole file is broken, the
previous cached list — or the built-in fallback — is used.
