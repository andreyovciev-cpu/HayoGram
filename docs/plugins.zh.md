# Hayo Gram 插件

[English](plugins.md) · [Русский](plugins.ru.md) · **中文**

插件是在 Hayo Gram **内部**运行的 Python 代码，不是独立应用，也不是机器人。

## 先读这一段

插件**没有沙盒**。它可以读取你的聊天、以你的身份发送消息，并访问应用能访问的文件。
清单里的权限只是作者的声明，应用不会检查或限制。只安装你自己读过源码并信任的插件。

插件代码不得混淆：任何人都要能读懂它做了什么。

## 格式

`.marp` 就是普通的 zip：

```
plugin.marp
├── manifest.json   必需
├── main.py         必需
├── icon.png        可选
└── ...             插件需要的其他文件
```

```json
{
  "id": "hayo.example",
  "name": "示例",
  "version": "1.0",
  "author": "hayozimi",
  "description": "在控制台打印问候。",
  "min_version": "2.0",
  "permissions": ["ui"]
}
```

可选翻译字段：`name_en`、`name_ru`、`description_en`、`description_ru`。

## 最小插件

```python
def on_start():
    margelet.log("你好，来自 Hayo Gram", margelet.name)
```

`on_start()` 在插件启动时调用；没有它时，文件会从上到下直接执行。

全局对象 `margelet` 提供 `log`、`error`、`toast`、`ui`、`background`、`every`、
`cancel`、`get`、`set`、`flag`、`fetch`、`window`、`send`、`activity`，以及
`on_chat_opened`、`on_send`、`on_message`、`on_deleted`、`on_pin`、`button`、
`on_settings`、`pick_file` 等钩子。

插件默认关闭。新安装的插件需要你手动启用。
