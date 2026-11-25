# Underwriting Processing Tool

A single-file web tool (`index.html`) for 1CISP Underwriting Analysts (UAs) to prepare underwriting letters, preview them as a formal letter, and download them as Word files. It also holds a premium calculator, a masterlist generator, a name splitter, an underwriting FAQ and an age calculator.

Developed by Daigo. © 2026 Daigo. All rights reserved.

**Current version:** v3.0.157

## Tools

The **Underwriting Tools** button at the top opens a list of tool buttons. Pick one to switch tools (the current tool is highlighted, and the list closes after you pick). Tools, in this order:

1. **Letter Generator:** prepares underwriting letters (see below).
2. **Masterlist Generator:** builds the list of insured members as an Excel file with the 1CISP letterhead, for SIP, SGYRT, GYRT, GBLISS, YES and Special GYRT (residency 0-119 and 120 and above).
3. **Premium Calculator:** based on the CIS computation workbook. Calculators for premium due, underpayment, overpayment, substandard rating and pro-rated premium, with an Excel download.
   - Premium due can be typed in to replace the computed amount (shown highlighted); it feeds the over / (under) payment, tables and letters.
   - Members with a sub-standard multiplier are compared against the multiplied premium. The total rate (rate x sub-standard rate) is rounded to 2 decimals.
   - **Late remittance** tab: lists the members whose effectivity date is more than 60 days (61 or more) before the A.R. date, which is the **To date** in Rates and settings. The Summary status then reads Late remittance. **Send to letter generator** fills the Late Remittance – Other Plans letter table (name, A.R. no. and date, effectivity date, number of days). In the letter, the A.R. No. and Date column is filled from the letter's own A.R. No. and date (left blank on purpose); the optional A.R. no. box in Rates and settings overrides it. The Excel download gets a Late remittance sheet when there are late members.
   - **Amounts** (sum insured, premium paid, premium due, invoice and unused premium) are shown with 2 decimals, including when uploaded or pasted.
   - **Upload CSV / Excel** fills the member rows from a `.csv`, `.xlsx` or `.xlsm` file (for example the LPPI summary). Columns are matched by their header names (Last Name, First Name, MI, Suffix, Birthdate, Loan Amount or Sum Insured, Effectivity, Expiry, Gross Prem or Premium Paid). Lines without a name, such as the TOTAL line, are skipped. A file without a header row is read in the table's column order. If the table already has members, you are asked before they are replaced. Old `.xls` files must be saved as `.xlsx` or `.csv` first. The file is read in the browser and never leaves the computer.
   - **Gender:** the Members table has an optional Gender column. Only M, F, Male and Female are accepted; any other entry is cleared and the tool guesses from the first name instead (shown in grey in the cell). The upload fills it from a Gender or Sex column. The guess uses a list of common Filipino and English first names plus name endings, so it is not always right; type M or F to correct it. The Summary shows Male, Female and Others counts (Others means the name could not be identified), and the Excel download has the same counts and a Gender column.
   - **Send to letter generator** on the Underpayment tab fills the Underpayment letter table. On the Overpayment tab it fills the Premium Refund – Overpayment letter table.
4. **Age Calculator:** results appear as soon as the dates are picked, and each result has a Copy button. Dates are counted as whole days, like in Excel.
   - **Age:** `ROUND((Effectivity date - Birth date) / 365, 0)`
   - **Number of days:** `Date to - Date from`
   - **Number of months:** `ROUND((Date to - Date from) / (365 / 12), 0)`
   - A warning shows when the birth date is later than the effectivity date, or Date to is earlier than Date from.
5. **Name Splitter:** upload an Excel masterlist (`.xlsx`, `.xlsm`) and download a copy with Surname, Extension, First name and Middle name columns added beside the original names. It runs in the browser; the file never leaves the computer. Old `.xls` files must be saved as `.xlsx` first.
6. **Underwriting FAQ:** a searchable list of questions and answers from the Underwriting Department FAQ deck (LPPI plan), with a section filter.

## Summary Report

The **Summary Report** button at the top (next to Underwriting Tools) opens a list of reports, like the Underwriting Tools list. Pick one to open it. The first report is **FICCO**, read from the FICCO monitoring Google Sheet (one tab per UA, named by initials such as JCTA).

- Shows total net premium, total female, total male, total members and total premium adjustments, **per plan name** or **overall (by UA)**, with UA, plan and month filters.
- Net premium is the PREMIUM total column of each month; adjustments are the ADJUSTMENTS column. Totals are added up from the branch and plan rows, not from the sheet's own total row.
- **Saved sheets:** type a name (for example FICCO 2027) and paste the Google Sheet link, then Save sheet. All UAs see the same saved list. Next year, save the new link and compare.
- **Comparison:** month by month with the change from the previous month, and year to year against another saved sheet, for net premium, members, female, male or adjustments.
- **Refresh** reads the sheet again; auto-refresh runs every 5 minutes while the page is open.
- **Other Coops** (second item in the Summary Report list): production of the other coops, combined from the saved Google Sheet files. Every sheet tab (visible or hidden) that has the production columns is added together, found by its header names. Region 13 (under marketing Rosalina D. Martinez and Erwin M. Martinez) comes first; add Region 10, Region 9, BARMM and others by typing a name and region and pasting their Google Sheet link, then Save sheet.
  - Shows gross premium, coop service fee, net premium, male, female and members, **per plan, per coop, per marketing or per region**, with region, marketing, plan, year and month filters.
  - The month is the A.R. date. Members come from the Male count and Female count columns, so rows not yet processed by Underwriting show 0.
  - Comparison: month by month and year against year, for net premium, gross premium, service fee, members, female or male.
- The Google Apps Script reads the sheet, so the sheet needs no public sharing. Share it with the Google account that owns the script, paste the new `directory-apps-script.gs`, run **authorizeSheets** once, then deploy a new version.

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
- On small screens the subtitle is hidden, the header items wrap, and on phones and short screens the header stops sticking, so the letter has more room.

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
2. Add one line to the `TOOLS` list in the tool bar script (`{id, label, el}`). It appears in the Underwriting Tools list by itself. Keep the label in title case (for example, Letter Generator) to match the other tools.
3. Add the new section's id to the `shell` list in the login script so it is locked before login, and hide it in print if needed.
4. Use only the libraries already loaded (`docx`, `exceljs`), or add a new library link with an integrity hash and allow its host in the content security policy.

## Changing the tool

Each change bumps the version number (the version button and the page title follow it) and adds one short line to the change log (the `CHANGELOG` list in `index.html`).

## Notes

This is an internal tool. Keep the repository private, because the page contains the Apps Script web app address and staff contact details.
