---
name: hebrew-bidi
description: "Apply these rules whenever the conversation contains Hebrew text, the user writes in Hebrew or Arabic, the user needs mixed Hebrew/English text for Microsoft Office (Word, Outlook, PowerPoint, Excel) or messaging apps (WhatsApp, Telegram), or the user mentions bidi, RTL, or direction problems. This skill fires automatically on any message containing Hebrew characters or requesting Hebrew/Office output for Umi Group (Universal Motors Israel) employees."
---

# UMI Group — Hebrew/English Bidi Rules

You are assisting employees of Umi Group (Universal Motors Israel). Conversations may be in Hebrew, English, or mixed Hebrew/English.

---

## Rule 1 — Hebrew Chat Rendering (MANDATORY, NO EXCEPTIONS)

Every response containing even one Hebrew word MUST be wrapped in an RTL widget. Never output plain Hebrew text in chat — the Claude UI renders it LTR and it becomes unreadable.

Use this exact template for ALL Hebrew content:

```html
<div style="direction:rtl;text-align:right;font-family:var(--font-sans);font-size:14px;color:var(--color-text-primary);line-height:1.7;padding:4px 0">
Hebrew content here
</div>
```

**This applies without exception to:**
- Full paragraphs and long answers in Hebrew
- Single sentences and one-liners in Hebrew
- Closing remarks and sign-offs in Hebrew
- Questions asked back to the user in Hebrew
- Bullet points and lists containing Hebrew
- Any content mixing Hebrew and English — even one Hebrew word triggers the requirement

There is no minimum length threshold. A two-word Hebrew response requires the RTL widget just as much as a full paragraph.

English product names, technical terms, and acronyms inside the Hebrew div render LTR naturally within the RTL flow — correct bidi behavior in the browser. Do not add extra Unicode markers inside the widget for in-browser rendering.

---

## Rule 2 — Bidi Markers for Office/WhatsApp

When the user needs mixed Hebrew/English text to copy into Word, Outlook, PowerPoint, Excel, WhatsApp, or Telegram, apply Unicode bidi markers before outputting.

### Dominant direction

Count the words: if more than half are Hebrew → RTL base. Otherwise → LTR base.

| Situation | Base direction | Opening marker |
|---|---|---|
| >50% Hebrew words | RTL | `\u200F` (RLM) |
| >50% English words | LTR | `\u200E` (LRM) |
| Ambiguous (50/50) | First meaningful word's language | — |

### Embedding markers

| Content type | Wrapper |
|---|---|
| English phrase inside Hebrew sentence | `\u202A…\u202C` (LRE…PDF) |
| Hebrew phrase inside English sentence | `\u202B…\u202C` (RLE…PDF) |
| Email address or URL (anywhere) | `\u202A…\u202C` (LRE…PDF) |
| Number in Hebrew sentence | `\u202A…\u202C` (LRE…PDF) |

### Trailing punctuation fix

The most common Office bidi bug: a period, comma, or `!` at the end of an RTL sentence drifts to the wrong side.

Fix: place `\u200F` (RLM) immediately before the punctuation mark.

### Common patterns

```
RTL sentence with English word:
\u200Fאני פותח את \u202AExcel\u202C עכשיו

RTL sentence ending with English + period:
\u200Fנפגש ב-\u202Ameeting\u202C\u200F.

English in parentheses in Hebrew sentence:
\u200Fהפגישה \u202A(Zoom)\u202C בשעה 3

Number in Hebrew sentence:
\u200Fיש לנו \u202A15\u202C לקוחות

Email in Hebrew sentence:
\u200Fשלח ל- \u202Ayoav@company.com\u202C

English sentence with Hebrew word:
\u200EWe need the \u202Bפרויקט\u202C done by Friday
```

### Output format

Always output corrected text in a **code block** labeled `"Copy this into Office:"` so the Unicode markers are visible and copyable without being re-interpreted by the chat UI.

---

## What NOT to do

- Do not reorder the user's words unless the input is clearly garbled
- Do not add markers inside a single-language run — only at language boundaries
- Do not double-mark: one `\u200F` at the start of an RTL paragraph is enough
- Do not use HTML `dir=` attributes — this rule is for plain text / Office, not web

---

## Reference: Office-specific notes

**Word / Outlook**: Paragraph RTL/LTR button (Home → Paragraph → RTL) overrides for the overall paragraph. Use `\u200F` at the start AND the RTL button for maximum compatibility.

**PowerPoint**: Each text box has independent direction. `\u200F` prefix is the most reliable programmatic fix.

**Excel**: Set cell alignment to Right-to-Left in Format Cells → Alignment. For cells with mixed content, `\u200F`/`\u200E` prefix is the best fix.

**WhatsApp / Telegram**: Both apps respect Unicode bidi markers. `\u200F` prefix is usually sufficient.
