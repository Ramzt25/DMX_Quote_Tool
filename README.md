# DMX_Quote_Tool
```markdown
# DMX Quote Tool (TLS DMX TOOL)

A single-file static web application for composing DMX system quotes.  
This tool helps sales staff choose a DMX system, add accessories, pick a startup option, and print a professional quote.

This README documents:
- What this project is
- How to use it as an end user
- How to develop and extend it
- Known issues and recommended fixes

Status
------
Single-file static web app: index.html (contains markup, styles, and JavaScript). Intended to run on any static host (GitHub Pages, Netlify, local web server).

Quick demo
----------
1. Open `index.html` in a modern browser (Chrome, Firefox, Edge, Safari).
2. Select a system from "Select a System" and set the Quantity.
3. Add accessories as needed using the "+" button and set accessory quantities.
4. Choose a Startup Option (required to enable the "PRINT QUOTE" button).
5. Click "PRINT QUOTE" to open the print dialog (the page will hide UI controls and show a printable view).

Features
--------
- Select from 4 base systems (A, B, C, D).
- Automatic system parts table that lists each part, unit price, and extended price.
- Accessories builder: add multiple accessories with quantities.
- Startup options with special pricing logic (in-house, salesman self-startup, on-site).
- Live total calculation combining system + accessories + startup.
- Print-ready layout (controls hidden for printing).
- Basic form reset.

File structure
--------------
- index.html — single-file app with embedded CSS and JavaScript.
  (Recommended refactor: split into `assets/css/styles.css` and `assets/js/app.js`.)

Usage — End users
-----------------
- Salesman: fill in Salesman name and optional "Same as Salesman" to autofill Quoted By.
- Project Name and Quote Date: optional metadata for your printed quote.
- System selection: choose a system and quantity — the "Selected System Parts" table updates.
- Accessories:
  - Click "+" to add accessory rows.
  - Choose an accessory and quantity; selected accessories appear in "Selected Accessories" table.
- Startup options: choose one. This selection is required before printing.
- Print Quote: only enabled when a Startup option is selected. This opens the system link and then triggers print.

Developer notes & how it works
------------------------------
- Data objects at top of script:
  - `systems` — parts lists for each system (part, desc, price)
  - `systemCapabilities` — array of capability strings per system
  - `startupOptions` / `startupDetails` — startup choice strings & detail bullets
  - `accessoryPrice` — accessory unit prices
- Key functions:
  - `loadSystem()` — populates system parts table and capabilities
  - `updateAllAccessorySelects()` — populates accessory selects depending on system
  - `addAccessoryDropdown()` / `removeAccessory()` — accessory UI management
  - `updateAccessoriesTable()` — builds accessories table
  - `updateStartupTable()` / `calculateTotalCost()` — startup and total calculations
  - `printToPDF()` — validator that ensures startup selected, opens system link, then prints

Recommended immediate improvements (apply in code)
-------------------------------------------------
1. Move CSS and JS out of index.html to `assets/css/styles.css` and `assets/js/app.js`.
2. Replace all innerHTML string-building for rows/options with DOM APIs (createElement/textContent)
   to eliminate XSS risk and make testing easier.
3. Use `Intl.NumberFormat` for currency formatting:
   ```js
   const currency = new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' });
   currency.format(123.45); // "$123.45"
   ```
4. Add accessibility improvements:
   - Add `<caption>` to each table.
   - Add `scope="col"` to `<th>`.
   - Use `aria-live="polite"` on the totals so screen readers announce changes.
   - Group inputs with `<fieldset>` and `<legend>`.
5. Make print behavior optional: move the external link opening into a separate user-initiated control to avoid popup blockers.
6. Keep totals numeric in code. If a price is "special", show a separate message rather than mixing types.
7. Add validation & inline error messages (instead of alerts) for a better UX.
8. Add unit tests for core calculations (formatting, total calculation) using Jest + jsdom.

Manual test checklist
---------------------
- [ ] Initial page shows "No system selected."
- [ ] Selecting a system populates system parts table and capabilities.
- [ ] Changing quantity updates line totals and grand total.
- [ ] Adding accessories shows accessories table and correct totals.
- [ ] Selecting startup updates startup table and total cost.
- [ ] Print button disabled until startup selected.
- [ ] Reset returns to default state.
- [ ] Keyboard navigation: tab through all inputs and buttons in a logical order.

Troubleshooting
---------------
- If the page behaves oddly when opening as file://, run a local server:
  - Python: `python3 -m http.server 8000`
  - Node: `npm i -g serve` then `serve -s . -l 5000`
- If print fails to open system link (popup blocked): open the system details link manually before printing.

Extending the project
---------------------
- Persist form state in localStorage so users can resume.
- Export quote as CSV or JSON for import into a CRM.
- Add an invoice/quote template to the print stylesheet to make printed quotes appear branded.
- Add user-configurable pricing rules via a small JSON file or admin view.

Contributing
------------
1. Fork the repository.
2. Create a branch: `git checkout -b feat/refactor-js-css`
3. Make changes, commit, and push.
4. Open a pull request describing your changes.

Suggested branch / PR tasks
- `refactor/css-js` — move inline CSS/JS into separate files and update index.html.
- `security/dom-safety` — replace innerHTML with DOM-safe builds.
- `accessibility/a11y` — add captions, aria-live, fieldsets, and run axe audits.
- `tests` — add Jest + jsdom unit tests for calculation functions.

License
-------
Add a LICENSE file to this repository. If you want a permissive license, use MIT.

Contact
-------
Repository owner: @Ramzt25

Acknowledgements & credits
--------------------------
Built from code provided by the repository owner. This README contains recommendations for improving security, accessibility, and maintainability.

```
