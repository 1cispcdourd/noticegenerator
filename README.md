# Underwriting Processing Tool

A single-file web tool (`index.html`) for 1CISP Underwriting Analysts (UAs) to prepare underwriting letters, preview them as a formal letter, and download them as Word files. It also holds a premium calculator, a masterlist generator, a name splitter, an underwriting FAQ and an age calculator.

Developed by Daigo. © 2026 Daigo. All rights reserved.

**Current version:** v3.0.198

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
   - The **Underpayment**, **Overpayment** and **Late remittance** tab buttons show a small red count of the members in that list (nothing shows when there are none).
   - **Send to letter generator** on the Underpayment tab fills the Underpayment letter table. On the Overpayment tab it fills the Premium Refund – Overpayment letter table. Each member's effectivity and expiry date from the calculator also go to the letter, so the Coverage Period (From / To) in the refund table is filled.
   - **Cooperative (commission matrix):** the Premium calculator works on the LPPI plan only, so the cooperative list comes from the **LPPI tab** of the commission matrix (for example UPDATED MINDANAO TERRITORY - COMMISSION MATRIX). In Rates and settings, type the start of any word in the cooperative name (for example `Pera` lists names with a word starting with Pera; names that begin with your text come first) or press the arrow to see the list, then pick one. It fills **Rate per 1,000** (gross annual rate / thousand), **Service fee, standard** (coop service fee for 18-65), **Service fee, overage 66+** (coop service fee for overage above 65), and the **Overage settings** (coverage minimum, maximum coverage and the 66-70 / 71-75 / 76-80 yearly rates, read from the SPECIAL RATE FOR OVERAGE text). The NEL and the matrix text are shown under the box. Service fee, underpayment is not changed. The overage service fee now drives the Service fee (age 66+) card and the Excel summary (it was fixed at 10%).
     - Overage rates are read from the usual wording (`*66-70 YEARS OLD - 350,000.00 and BELOW (3.00 PER MONTH 36 PER YEAR)` and so on). For a coop whose text is in another wording (about 78 of 645), the standard 350,000 / 2,000,000 / 36 / 48 / 60 are put in and the box says so; check the matrix text shown there. The Overage option (1 or 2) is not changed.
     - A rate in the matrix that is not a plain number (for example `old - 7.80 / new - 12.00`) is not filled; type the rate yourself.
     - 42 built-in cooperatives from the earlier list (not in the Mindanao matrix) stay in the list; a name found in the matrix uses the matrix.
     - **Commission matrix (Google Sheet)** box (under Overage settings) shows the saved matrix and has **Refresh matrix**, which reads it again. The link itself is saved by the Administrator (Administrator panel > Sheet links); all UAs use it, like the Summary Report sheets. The script finds the tab and columns by their header names (NAME OF COOPERATIVE, COOP SERVICE FEE, GROSS ANNUAL RATE / THOUSAND, NEL, SPECIAL RATE FOR OVERAGE in rows 1 to 10), so new rows and moved columns are picked up. The page also carries a copy of the matrix (2026-11-26) so the list works before the Google Sheet is read or when it cannot be read. The Google Sheet must be shared with the Google account that runs the Apps Script; run `authorizeSheets` once.
4. **Matrix of Commission:** pick a cooperative and read everything written for it in the commission matrix (taken from its Memorandum of Agreement), plan by plan. It reads **every visible plan tab** of the saved Google Sheet (LPPI, GYRT, GBLISS, SIP, SII, GPAI or ICARD, KOOPAMILYA, YES) and **every filled column**, using the same saved link as the Premium calculator (Commission matrix box). Nothing is written to the sheet.
   - Search: type the start of any word in the name; the list also shows which plan tabs the cooperative is in. A cooperative in several plans gets one button per plan tab; a cooperative with more than one row in a tab gets a Row button for each.
   - The answer is set out as questions: commission and service fee, rates, premium and coverage, age limits and overage, underwriting requirements (NEL / NML, contestability), claims, special arrangements and remarks, transferred business, MOA / proposal / master policy, and the officers and marketing agent. The Ask buttons show one question only; the search box inside the cooperative finds a word in any field (for example NEL or waived); each question has a Copy button.
   - Values are shown exactly as the Google Sheet displays them (percent signs, dates). Empty cells are left out. Header labels come from the sheet, so new columns appear without changing the page. Columns are matched by the header row (NAME OF COOPERATIVE) of each tab, so a new plan tab is picked up too; hidden tabs are ignored.
   - Needs Google Apps Script **v3.0.176** or newer (paste, then deploy a new version). The first open reads all the tabs and can take several seconds; the list is then kept for a few minutes. **Refresh from Google Sheet** reads the sheet again.
5. **Age Calculator:** results appear as soon as the dates are picked, and each result has a Copy button. Dates are counted as whole days, like in Excel.
   - **Age:** `ROUND((Effectivity date - Birth date) / 365, 0)`
   - **Number of days:** `Date to - Date from`
   - **Number of months:** `ROUND((Date to - Date from) / (365 / 12), 0)`
   - A warning shows when the birth date is later than the effectivity date, or Date to is earlier than Date from.
6. **Name Splitter:** upload an Excel masterlist (`.xlsx`, `.xlsm`) and download a copy with Surname, Extension, First name and Middle name columns added beside the original names. It runs in the browser; the file never leaves the computer. Old `.xls` files must be saved as `.xlsx` first.
7. **Underwriting FAQ:** a searchable list of questions and answers from the Underwriting Department FAQ deck (LPPI plan), with a section filter.

## Small screens

The page is built to work on a phone. On screens up to 700 px wide, buttons wrap their text and stay inside the screen, rows of buttons wrap onto a new line, and tables in the Administrator panel become cards. Password boxes have one eye button to show or hide the password (the browser's own eye icon is hidden). Do not name any element (id or class) starting with `ad`, `ads` or `banner`: some phone browsers with a built-in ad blocker hide such elements, which once hid a button. The Administrator panel uses the prefix `mg` instead.

## Google Sheet links (Administrator only)

The Google Sheet links (commission matrix, FICCO sheets, Other Coops sheets) are saved, changed and removed **only by the Administrator**, in the Administrator panel > **Sheet links**. UAs can open and use the saved sheets but have no boxes to add or remove a link. This is enforced in the Google Apps Script (**v3.0.177**, paste and deploy a new version): the save actions (`mxsrcset`, `ficcosrcset`, `ocsrcset`) need the administrator session, so a UA login cannot save a link even by other means. Saved changes are written to the activity log as ADMIN.

- **Commission matrix:** name and Google Sheet link (open the LPPI tab first so the link carries its tab). It feeds both the Matrix of Commission and the Premium calculator.
- **FICCO sheets** and **Other Coops sheets:** add a sheet (name, plus the region for Other Coops) or remove one. At least one sheet must stay saved. Removing a sheet does not change the Google Sheet itself.
- Each new Google Sheet must be shared with the Google account that runs the script; run `authorizeSheets` once after adding a new file.
- The saved links are kept in the script properties, so they are the same for every UA and are not lost when the page is updated.

## Last loaded Google Sheet data

Because reading the Google Sheets takes a while, these screens keep the **last data loaded** in the browser's local storage and show it right away, then read the sheet again in the background and replace it (the screen says "saved copy from ..." until the update arrives):

- **Matrix of Commission:** the cooperative list and the last 30 cooperatives opened; the last cooperative opened is reopened automatically.
- **Premium calculator:** the cooperative list from the commission matrix.
- **Summary Report > FICCO Report** and **Other Coops Report:** the last loaded data and the selections.

The saved copy belongs to the UA who loaded it (another UA logging in on the same browser does not see it). It stays after the tab is closed and after logging out, so the next visit starts fast. It is never a substitute for the sheet: the update always runs, and **Refresh** reads the sheet again from scratch. If the browser storage is full, the tool still works but cannot keep the copy.

## Page refresh

Every tool keeps its last data when the page is refreshed (session storage of the browser tab). It is cleared on log out, when the session ends, or when the tab is closed.

- **Letter Generator:** the open letter is restored (draft in local storage, cleared on log out). If a pasted picture is too big for the browser storage, the letter is still saved without the picture.
- **Masterlist Generator:** plan, details and pasted members.
- **Premium Calculator:** rates and settings, member rows, the open tab, pro-rated entries and the letter type picks.
- **Age Calculator:** the four dates.
- **Name Splitter:** the opened workbook (kept in the browser's IndexedDB), the ticked sheets, the chosen name columns and the options.
- **Underwriting FAQ:** the search words, section filter and expand state.
- **Summary Report:** see below.

## Summary Report

The **Summary Report** button at the top (next to Underwriting Tools) opens a list of reports, like the Underwriting Tools list. Pick one to open it. The first report is **FICCO**, read from the FICCO monitoring Google Sheet (one tab per UA, named by initials such as JCTA).

- Shows total net premium, total female, total male, total members and total premium adjustments, **per plan name** or **overall (by UA)**, with UA, plan and month filters.
- Net premium is the PREMIUM total column of each month; adjustments are the ADJUSTMENTS column. Totals are added up from the branch and plan rows, not from the sheet's own total row.
- **Branch (FICCO Report):** the filter row has a **Branch** filter and the View has **Per branch**. The cards, the table and the monthly comparison then show only that branch (net premium, male, female, members, adjustments). This needs the Google Apps Script v3.0.188.
- **S.I and C.R monitoring:** one row per branch and plan with premium in the month (month block columns S.I No., S.I Date, C.R No., C.R Date). Pick the **Year** (saved sheet), **Month** and **Branch** in the box to see whether that branch already has its S.I / C.R (they are the same choices as the Saved sheet and Month at the top). Rows with none are marked **No S.I** / **No C.R** as of the day the report was loaded, and can be filtered by status or searched by S.I No., C.R No., branch, plan or UA. The boxes also show total FICCO branches, total plans, S.I / C.R received and not received.
- **Deposits per month:** per UA and month, the deposited amount (Premium amount indicated in D.S) against the sheet's net premium with the difference, the date deposited (D.S date), date forwarded to treasury and date received by UA (Date received S.I from treasury). When a UA typed one date for the whole month it shows once; different dates show as first – last.
- **Saved sheets:** the Administrator saves each sheet (name, for example FICCO 2027, and Google Sheet link) in Administrator panel > Sheet links. All UAs see the same saved list. Next year, the Administrator saves the new link and you compare.
- **Comparison:** month by month with the change from the previous month, and year to year against another saved sheet, for net premium, members, female, male or adjustments.
- **Page refresh:** the last loaded data, the open report and the filters are kept for the browser tab (session storage) and shown at once after a refresh while the numbers update in the background. They are cleared on log out or when the session ends.
- **Refresh** reads the sheet again; auto-refresh runs every 5 minutes while the page is open.
- **Other Coops** (second item in the Summary Report list): production of the other coops, combined from the saved Google Sheet files. Every visible sheet tab (hidden tabs are skipped) that has the production columns is added together, found by its header names. The header row is searched in the first 40 rows of each tab (Region 9 and 10 have a title block, so their headers are on row 8), so new tabs, new rows, inserted columns and a moved header row are picked up on the next refresh without any change to the script. A tab is read when it has **COOP NAME** (or COOPERATIVE'S NAME) and **GROSS PREMIUM** headers; other tabs, such as Voucher, are listed as not read. The A.R. date can be a real date, a number or text such as `1.12.26`, `03.20.2026` or `3/20//26`; if it cannot be read, the date received is used. The UA is read from the UA column or **UA ASSIGN**, and Male / Female from **MALE COUNT** / **FEMALE COUNT** or **PER UA NO. OF APPROVED MALE / FEMALE MEMBERS**. Where the CSA left Gross premium, Service fee or Net premium empty, the AS PER UA amounts are used. TOTAL and PREPARED BY lines under the table are ignored. The saved link must be a Google Sheet (an Excel file uploaded to Drive must first be opened and saved as a Google Sheet). Region 13 (under marketing Rosalina D. Martinez and Erwin M. Martinez) comes first; the Administrator adds Region 10, Region 9, BARMM and others (name, region and Google Sheet link) in Administrator panel > Sheet links. All saved sheets are loaded at the same time, and the line under the sheet list shows which ones have loaded (✓), are still loading (…) or failed (✗). The Saved sheets dropdown only picks a sheet for the Administrator to remove; every saved sheet is always combined in the report. The cards and table show the original gross premium (AR AMOUNT) and the approved total sum insured (TOTAL SUMINSURED) beside the approved gross premium, service fee and net premium. Under the table, "Columns read in each sheet" lists the column letter found for every field of each tab, so the layout can be checked against the Google Sheet. Under the table, "Pending A.R. list" shows each A.R. that was received but has no Date Processed (A.R. no., date, coop, region, sheet tab and row number) for the current filters, so the pending count can be checked against the Google Sheet. A number typed as text with a typing error (for example 8.818.57 or 8/0) is counted as 0 and shown in the note under the table as a sheet error cell, the same way the Google Sheet's own SUM ignores it; fix the cell in the sheet.
  - Shows total A.R. received (A.R.s whose Date Received or Time Received is filled in the green Underwriting area; rows that repeat the A.R. no. of the row above count as one A.R., and coop rows with nothing in the green area yet are left out), A.R. processed (A.R.s whose Date Processed is filled), Refund / other (the Refund Count column) and A.R. pending (received less processed), plus gross premium, coop service fee, net premium, male, female and members, **per plan, per coop, per marketing, per region or per UA**, with region, UA, marketing, plan, year and month filters. The UA is read from the sheet's UA column (initials or name): JLJS, JCTA, KRMO (or KRO), HCOO (or HCO); blanks and anything else count as Others. Sheets made of several tabs are combined; tabs without the coop / gross premium headers are listed under the table.
  - The month is the A.R. date. Members come from the Male count and Female count columns, so rows not yet processed by Underwriting show 0.
  - Comparison: month by month and year against year, for net premium, gross premium, service fee, members, female, male, A.R. received, processed or pending.
- The Google Apps Script (v3.0.167) reads the sheet, so the sheet needs no public sharing. Share it with the Google account that owns the script, paste the new `directory-apps-script.gs`, run **authorizeSheets** once, then deploy a new version.

## Letter Generator

- **Letter types by category:** Confirmation of Coverage (CL), I-CARD, Underpayment, Premium Refund, Deficiency, Request for FME, Payment for Underpayment (Approval), Late Remittance, Certificate of No Previous Coverage, Renewal Notice, Authority to Deduct (ATD), Transmittal, Cancellation, Multiple Discrepancies (combined sections), and Customized Letter (a blank letter with your own title, text, optional reference and tables with any number of rows and columns).
- **Live letter preview** that paginates like the printed letter. The preview stays blank until a letter type is chosen. Names of insured in the letter tables are shown in proper case.
- **Downloads:** Word (`.docx`), Excel request form (`.xlsx`), and Print / PDF, Print letter only, and Print PR only (the request for payment form). In the request for payment form, the Reason and Particulars of each member can be typed freely. Its reference line shows the letter's A.R. / S.I. / C.R. number and date; if the letter was filled in with the 1CISP invoice number and date instead, those are shown. Its reference line shows the letter's A.R. / S.I. / C.R. number and date; if the letter was filled in with the 1CISP invoice number and date instead, those are shown.
- **Underpayment amounts** are rounded to 2 decimals per member, so the total, service fee and net add up.
- **Refund ending sentence:** with *Returned to your office (15 days)* selected, a net refund of ₱1,000 or less says *Please deduct ... refund on your next remittance* instead; the grey text in the manual box shows the wording the letter really uses. Choose *Always returned to your office (15 days)* to keep the 15-day wording for any amount, or type your own sentence in the box.
- **Save location:** Save as (ask each time, default), save to a chosen folder, or normal browser download. Folder and Save as work in Chrome and Edge on a computer.
- **Drafts:** the last letter is restored automatically after a page refresh, and named drafts can be saved, duplicated and deleted. When logged in, drafts are saved to your own folder in the shared Google Drive folder (one folder per UA; the script keeps each UA out of the others' folders).
- **Cooperative directory:** remembers cooperatives, branches and managers and fills in name, position and address. A shared list is kept in a Google Sheet so every UA sees the same entries. Backup and import are available.
- **Reference types:** A.R. No., S.I. No. and C.R. No. (reference and date shown in bold in the letter).

## Header

- **Account menu:** after login, a "Welcome! (your name)" pill appears at the top right. Click it to open **Change password** or **Log out**.
- On small screens the Preview button scrolls to the letter preview. A failed or slow check of the login session (common on phones) no longer logs you out; only a confirmed ended session does.
- On small screens the subtitle is hidden, the header items wrap, and on phones and short screens the header stops sticking, so the letter has more room.

## Login and administration

- While logged out, the login box is the only thing on the page: every tool is hidden and the page cannot be scrolled behind it (also on phones).

- UAs log in with their name and password. Passwords are stored as hashed values, not readable text.
- Users can change their own password (minimum 12 characters) from the account menu. Repeated wrong tries lock a name for a short time.
- Sessions end after 24 hours without activity.
- An Administrator panel (button on the login screen) lets the admin reset or unlock UA passwords (on phones the Reset password section scrolls into view above the keyboard), force log out, add or remove UAs, manage marketing contacts, approvers and bank accounts, and view the activity log.

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

Each change bumps the version number, which is set in one place in `index.html` (`APP_VER`, used for the page title and the login box). There is no change log inside the page.

## Notes

This is an internal tool. Keep the repository private, because the page contains the Apps Script web app address and staff contact details.

Layout (v3.0.178): every tool page uses the full page width. Matrix of Commission and Underwriting FAQ show two columns on wide screens and one on narrow screens.
Letter Generator (v3.0.179): on screens wider than 1280 px the form takes half the page and the preview the other half.
Premium calculator and Masterlist Generator (v3.0.180): on screens wider than 1100 px the boxes sit side by side (Rates and settings next to Summary; masterlist details next to the paste box).
Letter form (v3.0.181): on screens wider than 1280 px the single-field boxes sit in two columns; checkboxes, text areas and sections stay full width, and an odd field out takes the full row.

- **Loading line:** while any tool is waiting for Google Sheets (the Premium calculator rates, commission matrix, Summary Reports, Directory, saving in the Administrator panel), a thin moving line shows at the top of the page with the note "Loading from Google Sheets…". It appears after a quarter of a second, so quick requests do not flash it, and disappears when the last request finishes.
