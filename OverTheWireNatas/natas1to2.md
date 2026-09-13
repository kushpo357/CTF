# Natas1 → Natas2

Natas1 uses the same core trick as natas0 — the password is left in an HTML comment — but adds a small obstacle: the page disables right-clicking with an `oncontextmenu` handler, which is the usual way people reach "View Source" or "Inspect."

**Sticking point:** Right-click was blocked, so the normal "Inspect Element" context-menu path was unavailable.

**Resolution:** Browser dev tools don't have to be opened via right-click. Using the keyboard shortcut `Ctrl+Shift+C` (or `Ctrl+Shift+I` for the full DevTools panel) opens the Elements/Inspector panel directly, bypassing the disabled context menu entirely.

Once DevTools was open, the page's HTML showed the same kind of hidden comment as natas0:

```html
<!--The password for natas2 is <natas2_password> (will differ per player) -->
```

**Key insight:** Client-side restrictions like a disabled right-click menu are cosmetic — they block one entry point to the browser's dev tools, not the dev tools themselves. Any client-side "protection" like this can be bypassed since the browser still has full access to the page.

**Key technique:** Bypassing a disabled right-click context menu with the `Ctrl+Shift+C` DevTools shortcut to reach page source.
