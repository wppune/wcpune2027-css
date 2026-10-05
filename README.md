> **⚠️ This repository has moved.**
> Development continues at **https://github.com/WC-Pune/wcpune2027-css**.
> This copy is no longer the source of truth and may be out of date.

# WordCamp Pune 2027 — Remote CSS

`wcpune2027.css` is the custom stylesheet for pune.wordcamp.org/2027 (theme: Twenty Twenty-Five).
It is loaded by **Appearance → Remote CSS**, in **Add on to existing CSS** mode, on top of the theme. Appearance → Additional CSS should stay empty.

| File | Purpose |
|---|---|
| `wcpune2027.css` | The live site CSS (what Remote CSS fetches) |
| `dev-inject.user.js` | Dev only — preview local CSS on the live site |
| `legacy-additional-css.css` | Backup of the old Additional CSS, for rollback |

## Palette

| Colour | Hex | Use for | Text on it |
|---|---|---|---|
| Heritage Maroon | `#8E1616` | Logo, footer background, section dividers, **content links**, button hover, header border | White (9.2:1), Saffron (4.6:1) |
| Marigold Saffron | `#F5A623` | Buttons, link underlines, hover accents | **Charcoal only** (8.1:1) — never white (2.0:1) |
| Midnight Charcoal | `#1A202C` | Headings, body text, header menu, site title | — |
| Crisp White | `#FFFFFF` | Page and header background | Charcoal, Maroon |
| Warm Sand | `#F7F5F0` | Inputs, search/subscribe fields, cards, light backgrounds | Charcoal (15:1), Maroon (8.5:1) |

Borders on sand use `#D6D5D3`.

**Rule of thumb:** saffron is never used for text on a light background.

To change a colour, **find & replace its hex** across `wcpune2027.css`.

## ⚠️ Don't use CSS variables
Remote CSS runs the file through WordCamp's sanitiser, which **deletes every CSS custom
property declaration** (`--anything: value;`). It works in local preview but silently
disappears on the live site. Write colours as plain hex values.
Reading WordPress's own variables, like `var(--wp--preset--font-size--small)`, is fine.
Only declaring new ones is stripped. `@import` is stripped too. Use the WordCamp Fonts tool for fonts.

## How it works
Section 1 overrides the theme's colour classes (`.has-custom-brick-red-color`,
`.has-accent-5-background-color`, …) with the new palette, so existing blocks pick up the
new colours without editing content. These classes use `!important` in WordPress itself,
so our overrides need it too.

The homepage hero styles apply only to `.wc-hero.alignfull`. The Contact Us button group also has the class `wc-hero`; remove that class in the editor when you can. Header menu and site title stay charcoal. Filled buttons are saffron; Outline buttons stay a maroon border. `.paper`, `.fixed-width`, and the old `.wc-hero__panel` rules were removed because the current pages do not use them. `.fixed-width` still narrows the photo grid until Additional CSS is emptied.

## Local development (live site + local CSS)
1. `cd remote-css && python3 -m http.server 8027`
2. Install the [Tampermonkey extension for Chrome](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=en),
   then add `dev-inject.user.js` as a new script.
   (In `chrome://extensions` → Tampermonkey → Details, turn on **Allow User Scripts**.)
3. Open https://pune.wordcamp.org/2027/ — edit the CSS, save, reload.
   Toggle the userscript off to compare with the current live site.

No-install alternative: paste the body of `dev-inject.user.js` into the DevTools console after each reload.

## Publishing
1. Push this folder to a public GitHub repo (e.g. `wcpune2027-css`).
2. wp-admin → **Appearance → Remote CSS**: paste the GitHub URL of `wcpune2027.css`,
   choose **Add on to the existing CSS**, click **Update**.
3. Copy the webhook URL shown on that screen into GitHub → repo Settings → Webhooks,
   so every `git push` re-syncs the site.
4. Check that it's live. View the page source and search for `wordcamp_remote_css`.
   If it's missing, the save on the Remote CSS screen failed: look for a red error message there.
5. Once confirmed, empty **Appearance → Additional CSS** (backup is in `legacy-additional-css.css`).
6. In the editor, change the homepage hero group and header group backgrounds from the
   custom `#e1a48e` to a palette colour; the `[style*="#e1a48e"]` overrides can then be removed.
