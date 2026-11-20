# Hayo Gram plugins

**English** · [Русский](plugins.ru.md) · [中文](plugins.zh.md)

A plugin is Python code that runs **inside** Hayo Gram. It is not a separate
app and not a bot.

## Read this first

There is **no sandbox**. A plugin can read your chats, send messages as you and
access files the app can access. Permissions in a manifest are only the
author's declaration — the app does not enforce them. Install only plugins
whose source you have read and trust.

Plugin code must not be obfuscated: anyone must be able to read what it does.

## Format

A `.marp` file is a zip archive with:

```
plugin.marp
├── manifest.json   required
├── main.py         required
├── icon.png        optional
└── ...             anything else the plugin needs
```

```json
{
  "id": "hayo.example",
  "name": "Example",
  "version": "1.0",
  "author": "hayozimi",
  "description": "Says hello in the console.",
  "min_version": "2.0",
  "permissions": ["ui"]
}
```

`name_ru`, `name_zh`, `description_ru`, `description_zh` are optional
translations.

## Minimal plugin

```python
def on_start():
    margelet.log("hello from Hayo Gram", margelet.name)
```

`on_start()` is called when the plugin starts. Without it the file is simply
executed top to bottom.

The global `margelet` object gives you `log`, `error`, `toast`, `ui`,
`background`, `every`, `cancel`, `get`, `set`, `flag`, `fetch`, `window`,
`send`, `activity` and the event hooks `on_chat_opened`, `on_send`,
`on_message`, `on_deleted`, `on_pin`, `button`, `on_settings`, `pick_file`.

Plugins are off by default. A newly installed plugin stays disabled until you
turn it on.
