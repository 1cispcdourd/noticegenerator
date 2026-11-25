# Underwriting Processing Tool

A single-file web tool (`index.html`) for 1CISP Underwriting Analysts (UAs) to prepare underwriting letters, preview them as a formal letter, and download them as Word files. It also holds a premium calculator, a masterlist generator, a name splitter and an underwriting FAQ.

Developed by Daigo. © 2026 Daigo. All rights reserved.

**Current version:** v3.0.143

## Tools

Buttons in the tool bar at the top switch between tools, in this order:

1. **Letter Generator:** prepares underwriting letters (see below).
2. **Masterlist Generator:** builds the list of insured members as an Excel file with the 1CISP letterhead, for SIP, SGYRT, GYRT, GBLISS, YES and Special GYRT (residency 0-119 and 120 and above).
3. **Premium Calculator:** based on the CIS computation workbook. Calculators for premium due, underpayment, overpayment, substandard rating and pro-rated premium, with an Excel download.
   - Premium due can be typed in to replace the computed amount (shown highlighted); it feeds the over / (under) payment, tables and letters.
   - Members with a sub-standard multiplier are compared against the multiplied premium. The total rate (rate x sub-standard rate) is rounded to 2 decimals.
   - **Send to letter generator** on the Underpayment tab fills the Underpayment letter table. On the Overpayment tab it fills the Premium Refund – Overpayment letter table.
4. **Name Splitter:** upload an Excel masterlist (`.xlsx`, `.xlsm`) and download a copy with Surname, Extension, First name and Middle name columns added beside the original names. It runs in the browser; the file never leaves the computer. Old `.xls` files must be saved as `.xlsx` first.
5. **Underwriting FAQ:** a searchable list of questions and answers from the Underwriting Department FAQ deck (LPPI plan), with a section filter.

## Letter Generator

- **Letter types by category:** Confirmation of Coverage (CL), I-CARD, Underpayment, Premium Refund, Deficiency, Request for FME, Payment for Underpayment (Approval), Late Remittance, Certificate of No Previous Coverage, Renewal Notice, Authority to Deduct (ATD), Transmittal, Cancellation, Multiple Discrepancies (combined sections), and Customized Letter (a blank letter with your own title, text, optional reference and tables with any number of rows and columns).
- **Live letter preview** that paginates like the printed letter. The preview stays blank until a letter type is chosen. Names of insured in the letter tables are shown in proper case.
- **Downloads:** Word (`.docx`), Excel request form (`.xlsx`), and Print / PDF, Print letter only, and Print PR only (the request for payment form). In the request for payment form, the Reason and Particulars of each member can be typed freely.
- **Underpayment amounts** are rounded to 2 decimals per member, so the total, service fee and net add up.
- **Save location:** Save as (ask each time, default), save to a chosen folder, or normal browser download. Folder and Save as work in Chrome and Edge on a computer.
- **Drafts:** the last letter is restored automatically after a page refresh, and named drafts can be saved, duplicated and deleted. When logged in, drafts are saved to your own folder in the shared Google Drive folder (one folder per UA; the script keeps each UA out of the others' folders).
- **Cooperative directory:** remembers cooperatives, branches and managers and fills in name, position and address. A shared list is kept in a Google Sheet so every UA sees the same entries. Backup and import are available.
- **Reference types:** A.R. No., S.I. No. and C.R. No. (reference and date shown in bold in the letter).

## Header

- **Version changes:** click the version number at the top of the page to see the short change log.
- **Account menu:** after login, a "Welcome! (your name)" pill appears at the top right. Click it to open **Change password** or **Log out**.
- On small screens the tool buttons wrap onto their own row and the header stops sticking, so the letter has more room.

## Login and administration

- UAs log in with their name and password. Passwords are stored as hashed values, not readable text.
- Users can change their own password (minimum 12 characters) from the account menu. Repeated wrong tries lock a name for a short time.
- Sessions end after 24 hours without activity.
- An Administrator panel (button on the login screen) lets the admin reset or unlock UA passwords, force log out, add or remove UAs, manage marketing contacts, approvers and bank accounts, and view the activity log.

## How it works

- Everything runs in the browser from one HTML file. No build step.
- Shared data (UA list, directory, roster, activity log) is stored in a Google Sheet through a Google Apps Script web app (`directory-apps-script.gs`).
- The Word and Excel libraries (`docx`, `exceljs`) load from cdn.jsdelivr.net, so an internet connection is needed to generate those files.
- A content security policy limits the page to its own code, those two libraries and the Google Apps Script address. Each library link carries an integrity hash, so a changed file will not run.
- Drafts and settings are kept in the browser's local storage. The saved draft is cleared on log out.

## Use

1. Open the deployed page (or `index.html`) in Chrome or Edge.
2. Log in with your UA name and password.
3. Pick a tool in the tool bar. For letters, choose a letter type, fill in the details, and check the preview.
4. Press **Download Word (.docx)** and choose where to save it.

## Deploy

The tool is a static page. It can be hosted on any static host, such as Netlify (Netlify Drop) or Cloudflare Pages. Upload `index.html` (and `directory.json` if you use the offline directory backup).

To change the shared data connection, set up the Google Sheet and Apps Script from `directory-apps-script.gs`, then place the web app address in the `DIR_API` setting in `index.html`.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole tool |
| `directory-apps-script.gs` | Google Apps Script for the shared Google Sheet |
| `directory.json` | Optional directory backup |
| `README.md` | This file |

## Adding a new tool

1. Add a section to `index.html` next to `<div id="nsApp" class="tool-off">`, with its own id and the class `tool-off` (hidden until chosen).
2. Add one line to the `TOOLS` list in the tool bar script (`{id, label, el}`). The button appears by itself. Keep the label in title case (for example, Letter Generator) to match the other tools.
3. Add the new section's id to the `shell` list in the login script so it is locked before login, and hide it in print if needed.
4. Use only the libraries already loaded (`docx`, `exceljs`), or add a new library link with an integrity hash and allow its host in the content security policy.

## Changing the tool

Each change bumps the version number (the version button and the page title follow it) and adds one short line to the change log (the `CHANGELOG` list in `index.html`).

## Notes

This is an internal tool. Keep the repository private, because the page contains the Apps Script web app address and staff contact details.
