# UMI Group — Claude Plugins

Internal Claude plugin marketplace for Umi Group (Universal Motors Israel).

---

## Available plugins

| Plugin | Description |
|--------|-------------|
| `umi-bidi` | Hebrew/English bidirectional text — enforces RTL rendering in chat and correct Unicode bidi markers for Office/WhatsApp |

---

## Install (one-time per employee)

### In Claude Desktop or Claude.ai

1. Open Claude → Settings → **Plugins**
2. Click **Add** → enter `UMI-GROUP/ClaudePlugins` → click **Sync**
3. Find **UMI Hebrew/English Bidi** in the list → click **Install**

### In Claude Code (terminal)

```bash
claude plugin marketplace add UMI-GROUP/ClaudePlugins
claude plugin install umi-bidi@umi-group
```

---

## What the plugin does

**Hebrew chat rendering**: Every Claude response containing Hebrew is automatically wrapped in a right-to-left widget so it displays correctly in the UI.

**Office/WhatsApp text**: When you ask for mixed Hebrew/English text to paste into Word, Outlook, PowerPoint, Excel, or WhatsApp, Claude applies the correct Unicode bidi markers (`\u200F`, `\u202A…\u202C`, etc.) and outputs the result in a copy-ready code block.

---

## Updating

When a new version is published:

```bash
claude plugin update umi-bidi@umi-group
```

Or in the UI: Settings → Plugins → find `umi-bidi` → Update.

---

## Maintained by

Umi Group AI Engineering — ai@umi.co.il
