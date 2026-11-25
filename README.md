# Underwriting Processing Tool

A single-file web tool (`index.html`) for 1CISP Underwriting Analysts (UAs) to prepare underwriting letters, preview them as a formal letter, and download them as Word files.

Developed by Daigo. © 2026 Daigo. All rights reserved.

**Current version:** v3.0.139

## Features

- **Tool bar:** buttons at the top switch between tools. Current tools, in order: **Letter generator**, **Masterlist Generator**, **Premium Calculator**, **Name Splitter** and **Underwriting FAQ**.
- **Name splitter:** upload an Excel masterlist (`.xlsx`, `.xlsm`) and download a copy with Surname, Extension, First name and Middle name columns added beside the original names. It runs in the browser; the file never leaves the computer. Old `.xls` files must be saved as `.xlsx` first.
- **Premium calculator:** the CIS computation workbook (COMPUTATION - CIS V4.1) as a tool. Type or paste members (branch, name, birthdate, sum insured, effective and expiry dates, premium paid) and it works out age, term, rate, premium due (which can also be typed in to replace the computed amount, for a hidden underpayment) and over / (under) payment, with the summary, service fees, net premium and approver. Separate tabs list the **Underpayment**, **Overpayment** and **Substandard rating** tables (with a rate multiplier per member) and a **Pro-rated** calculator. A cooperative rating list fills the rate and service fee. The Underpayment tab can send its rows into an underpayment letter, and the Overpayment tab into the Premium Refund – Overpayment letter. Results download as an Excel file (values only). Member details are not saved anywhere and are cleared on refresh.
- **Masterlist Generator:** builds the list of insured members (masterlist) as an Excel file with the 1CISP letterhead and logo. Choose the plan (SIP, SGYRT, GYRT, GBLISS, YES Plan, Special GYRT residency 0-119 months, or Special GYRT residency 120 months and above), enter the policy holder, address, reference (A.R. No., C.R. No. or S.I. No.), date, total premium paid and the Underwriting Analyst, then paste the members from Excel in the column order shown. Age, expiry date, term, totals and the over / (under) payment are Excel formulas. For Special GYRT the file is named like `CDO-2311 ML 0-119 SEPTEMBER 2026` (reference number and month typed by the user), principals are numbered, dependents are lettered A, B, C, and a 10% service fee is deducted. Nothing is saved; it is cleared on refresh.
- **Underwriting FAQ:** a searchable list of the questions and answers in the Underwriting Department FAQ deck (LPPI plan). Type words to filter (every word must match, matches are highlighted), pick a section, and expand or collapse the answers. Tables from the deck (misstatement of age, premium rating without an application form, discrepancies, underpayment to collect or forego, incontestability examples) are shown as tables. The grey slide number points back to the deck. The text is built into `index.html`; to change an answer, edit the `FAQ` data in the Underwriting FAQ script.
- **Letter types by category:** Confirmation of Coverage (CL), I-CARD, Underpayment, Premium Refund, Deficiency, Request for FME, Payment for Underpayment (Approval), Late Remittance, Certificate of No Previous Coverage, Renewal Notice, Authority to Deduct (ATD), Transmittal, Cancellation, Multiple Discrepancies (combined sections), and Customized Letter (a blank letter with your own title, text, optional reference and tables with any number of rows and columns).
- **Live letter preview** that paginates like the printed letter. The preview stays blank until a letter type is chosen.
- **Downloads:** Word (`.docx`), Excel request form (`.xlsx`), and Print / PDF, Print letter only, and Print PR only (the request for payment form). In the request for payment form, the Reason and Particulars of each member can be typed freely.
- **Save location:** Save as (ask each time, default), save to a chosen folder, or normal browser download. Folder and Save as work in Chrome and Edge on a computer.
- **Drafts:** the last letter is restored automatically after a page refresh, and named drafts can be saved, duplicated and deleted. When logged in, drafts are saved to your own folder in the shared Google Drive folder (one folder per UA; the script keeps each UA out of the others' folders).
- **Cooperative directory:** remembers cooperatives, branches and managers and fills in name, position and address. A shared list is kept in a Google Sheet so every UA sees the same entries. Backup and import are available.
- **Reference types:** A.R. No., S.I. No. and C.R. No. (reference and date shown in bold in the letter).
- **Version changes:** click the version number at the top of the page to see the short change log.

## Login and administration

- UAs log in with their name and password. Passwords are stored as hashed values, not readable text.
- Users can change their own password (minimum 12 characters). Repeated wrong tries lock a name for a short time.
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
3. Choose a letter type, fill in the details, and check the preview.
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

## Adding a new tool

1. Add a section to `index.html` next to `<div id="nsApp" class="tool-off">`, with its own id and the class `tool-off` (hidden until chosen).
2. Add one line to the `TOOLS` list in the tool bar script (`{id, label, el}`). The button appears by itself.
3. Add the new section's id to the `shell` list in the login script so it is locked before login, and hide it in print if needed.
4. Use only the libraries already loaded (`docx`, `exceljs`), or add a new library link with an integrity hash and allow its host in the content security policy.

## Changing the tool

Each change bumps the version number and adds one short line to the change log (the `CHANGELOG` list in `index.html`).

## Notes

This is an internal tool. Keep the repository private, because the page contains the Apps Script web app address and staff contact details.
