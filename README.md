# Underwriting Notice Generator

A single-file web tool (`index.html`) for 1CISP Underwriting Analysts (UAs) to prepare underwriting letters, preview them as a formal letter, and download them as Word files.

Developed by Daigo. © 2026 Daigo. All rights reserved.

**Current version:** v3.0.126

## Features

- **Letter types by category:** Confirmation of Coverage (CL), I-CARD, Underpayment, Premium Refund, Deficiency, Request for FME, Payment for Underpayment (Approval), Late Remittance, Certificate of No Previous Coverage, Renewal Notice, Authority to Deduct (ATD), Transmittal, Cancellation, and Multiple Discrepancies (combined sections).
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

## Changing the tool

Each change bumps the version number and adds one short line to the change log (the `CHANGELOG` list in `index.html`).

## Notes

This is an internal tool. Keep the repository private, because the page contains the Apps Script web app address and staff contact details.
