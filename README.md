# Discord Arabic Translator

Swaps Discord UI strings for Arabic. Translation only, no RTL, no layout hacks.

Two builds, same dictionary:

| | File | Where it runs |
|---|------|----------------|
| **Browser** | `DiscordArabicUI.Bookmarklet.js` | [discord.com](https://discord.com/app) in your browser |
| **BetterDiscord** | `DiscordArabicUI.plugin.js` | Discord desktop app with [BetterDiscord](https://betterdiscord.app/) |

## Install

### Browser (bookmarklet)

1. Open `DiscordArabicUI.Bookmarklet.js` and copy the entire file (`Ctrl+A`, `Ctrl+C`). It must start with `javascript:`.
2. Create a new bookmark. Paste the code into the **URL** field (not the name). Save it (e.g. `Discord Arabic`).
3. Open [discord.com/app](https://discord.com/app) and sign in.
4. Each time you load Discord in the browser, click the bookmark once. The UI translates and keeps updating as you navigate.

After a full page refresh (`F5`), click the bookmark again.

### BetterDiscord (plugin)

1. Copy `DiscordArabicUI.plugin.js` into your BD plugins folder:
   - **Windows:** `%appdata%\BetterDiscord\plugins`
2. Reload Discord (`Ctrl+R`), then enable it under **BetterDiscord → Plugins**.

Runs automatically whenever Discord starts; no bookmark needed.

## Adding translations

Everything lives in `this.dict` inside `start()`:

```js
this.dict = {
    "Friends": "الأصدقاء",
    "Add Friend": "إضافة صديق",
};
```

Format is always `"English exactly as Discord shows it": "Arabic"`.

**Workflow:**

1. `Ctrl+Shift+I` → inspect the untranslated string.
2. Copy the exact text (`textContent`, or check `placeholder` / `title` / `aria-label` we handle all four)
3. Add a line to `this.dict`, save, toggle the plugin off/on (or reload Discord).
4. Done. If it still shows English, you copied the wrong string or wrong casing.

## Gotchas

- **Exact match only** after `trim()`. `Log Out` ≠ `Log out` add both if Discord uses both.
- **Leaf nodes only** (`children.length === 0`). Inspect the inner `<span>` if the label sits inside a button.
- **Won't touch** usernames, message content, timestamps, or `"User is typing..."` style dynamic strings unless you add the full string as a key.
- Duplicate keys: last one wins. Don't repeat yourself unless you enjoy debugging.

## Quick debug

```js
const key = "Add Friend";
document.querySelectorAll("*").forEach(el => {
  if (!el.children.length && el.textContent.trim() === key) console.log(el);
});
```

## Docs

Want to extend the plugin or understand how it works? Start with the [BetterDiscord plugins docs](https://docs.betterdiscord.app/plugins/) and look up what this repo actually uses:

| Used in `DiscordArabicUI.plugin.js` | Read |
|-------------------------------------|------|
| `@name` / `@version` header, `module.exports`, `start()` / `stop()` | [Plugin structure](https://docs.betterdiscord.app/plugins/introduction/structure), [Quick start](https://docs.betterdiscord.app/plugins/introduction/quick-start) |
| `document.querySelectorAll`, `textContent`, `placeholder`, `title`, `aria-label` | [Using the DOM](https://docs.betterdiscord.app/plugins/tutorials/using-the-dom) |
| `MutationObserver` on `document.body` (re-translate when Discord updates the UI) | [Using the DOM](https://docs.betterdiscord.app/plugins/tutorials/using-the-dom), [Changing Discord](https://docs.betterdiscord.app/plugins/tutorials/changing-discord) |
| `BdApi.UI.showToast(...)` on enable | [Toasts](https://docs.betterdiscord.app/plugins/ui-components/toasts) |

The bookmarklet uses the same DOM + `MutationObserver` logic without BetterDiscord; only the plugin file needs `BdApi`.

## Status

This plugin is still early but it will keep improving over time. If you want to contribute, reach out on Discord: [dis.ibrhub.net](https://dis.ibrhub.net)


## Star History

<a href="https://www.star-history.com/?repos=IBRHUB%2FDiscordArabicUI&type=date&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=IBRHUB/DiscordArabicUI&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=IBRHUB/DiscordArabicUI&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=IBRHUB/DiscordArabicUI&type=date&legend=top-left" />
 </picture>
</a>