# DeepFlow Translator — Browser Extension

Translate web pages and documents using the DeepL API. Includes character usage tracking, an inline selection toolbar, a floating editor, and document/XLIFF translation with robust MV3 offscreen support on Chromium browsers.

- Chrome/Edge/Brave: Manifest V3 with offscreen document for XLIFF.
- Firefox: Manifest V2 for maximum compatibility.

## Features
- Translate current page or selected text (preserves layout for inline translations).
- Floating editor with language swap, tone/formality options, and shortcuts.
- Document translation (DeepL Document API) with progress and automatic download.
- XLIFF (.xlf/.xliff) translation that preserves inline markup and structure.
- Multiple API keys with usage display and active-key selector.
- Light/Dark themes, customizable language lists, and safety confirmations.

## Folder structure
- `dpl-api-translate.chrome/` — Chrome build (MV3 with offscreen XLIFF).
- `dpl-api-translate.edge/` — Edge build (MV3 with offscreen XLIFF).
- `dpl-api-translate.brave/` — Brave build (MV3 with offscreen XLIFF).
- `dpl-api-translate.firefox/` — Firefox build (MV2).

Each folder is a standalone extension package (manifest, icons, scripts, HTML/CSS).

## Installation (development)

### Chrome / Edge / Brave (MV3)
1. Open the extensions page:
   - Chrome: `chrome://extensions`
   - Edge: `edge://extensions`
   - Brave: `brave://extensions`
2. Enable Developer Mode.
3. Click “Load unpacked” and select the respective folder:
   - `dpl-api-translate.chrome` (Chrome)
   - `dpl-api-translate.edge` (Edge)
   - `dpl-api-translate.brave` (Brave)
4. Optional: enable “Allow access to file URLs” if you want content scripts on local files.

### Firefox (MV2)
1. Go to `about:debugging` → This Firefox → Load Temporary Add-on.
2. Select `dpl-api-translate.firefox/manifest.json`.

## Usage
- Click the toolbar icon to open the popup.
- Choose source/target languages and press “Translate Page”.
- Select text on a page to see the inline toolbar, or open the Editor from the popup.
- Document Center: click “Translate File” and drop a supported file (`txt, htm, html, pdf, docx, pptx, xlf, xliff`).
- XLIFF: the extension extracts `<source>` segments, translates them, and injects `<target>` while preserving markup.

### Keyboard shortcuts (in editor)
- Translate: Cmd/Ctrl + Enter
- Open Editor: Alt + E
- Close Editor: Esc

## Permissions explained
- `activeTab`, `tabs`: open tabs, query the current tab, send messages to content scripts.
- `storage`: save extension settings (theme, API keys, language prefs) using `chrome.storage.sync`.
- `downloads`: save translated documents to your Downloads (or subfolder if configured).
- `offscreen` (Chromium MV3): create an offscreen document for robust XLIFF DOM parsing/serialization.
- `host_permissions`: DeepL API endpoints and page matches for content scripts.

## Privacy
- Your DeepL API keys are stored in your browser’s sync storage (not sent anywhere except to DeepL when translating).
- The extension doesn’t collect analytics or send browsing data to third parties.
- Translation content is sent only to DeepL endpoints you configure (free/pro), and only when you initiate translation.

## Configuration
- Open the options page from the popup “Settings”.
- Set theme, editor font size, confirmation thresholds, language lists/orders, and default save folder (subdir) for downloads.
- Manage multiple DeepL API keys; the active key’s usage bar shows characters used/remaining.

## XLIFF details
- Supported XLIFF versions: 1.2 (trans-unit) and 2.0 (unit/segment).
- The Chromium builds use an MV3 offscreen document to reliably parse and write XML while the service worker sleeps as needed.
- Inline markup inside segments is preserved during translation.

## Building for Store submission
- Chrome Web Store / Edge Add-ons / Brave: package the MV3 folder as a ZIP.
  - Validate manifest, ensure icons are present (16/48/128), remove dev/test files if needed.
  - Test on a clean profile.
- Firefox Add-ons (AMO): package the MV2 folder as a ZIP.
  - `browser_specific_settings` with a Gecko ID is present in the Firefox manifest.

Note: MV3 is required/recommended on Chromium stores. Firefox MV3 support is evolving; MV2 is used here for best compatibility.

## Localization (i18n)
- Strings are handled via `locales.js` and `data-i18n` attributes.
- Add new strings and ensure they are referenced in popup/options/document UIs.

## Troubleshooting
- “Translate Page” disabled: choose a target language and wait for character counting to finish.
- XLIFF fails on Chromium: ensure the extension has the `offscreen` permission and that `offscreen.html/js` are present; reload the extension.
- Document translation errors: messages are humanized; commonly quota or unsupported format.

## Contributing
- Fork, branch, and PR. Keep UI consistent (light/dark), and avoid adding heavy dependencies.
- Test across Chrome/Edge/Brave/Firefox. For Chromium, verify offscreen XLIFF with a sample `.xlf`.
- Please include a short description and screenshots/GIFs for UI changes.

## Donations
If you find this useful, consider supporting development:
- Ko‑fi: https://ko-fi.com/yourname
- PayPal.me: https://paypal.me/yourname

(Replace with your actual links; both buttons can be shown in the popup/options.)

## License
MIT. See `LICENSE` (add one if missing).

## Disclaimer
This project is not affiliated with or endorsed by DeepL GmbH. All trademarks are property of their respective owners.
