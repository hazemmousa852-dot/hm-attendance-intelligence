# HM Attendance Hub

A browser-local attendance analysis tool for fingerprint device exports. It reads XLS, XLSX and CSV files, matches punches to employee shifts, handles overnight shifts, leaves, permissions and official holidays, and exports detailed daily and employee-summary reports.

## Privacy

All parsing and calculations run in the browser. Uploaded employee and attendance files are not sent to a server and are not stored by the application.

## Run locally

Open a terminal in this folder and start any static web server, for example:

```powershell
python -m http.server 8080
```

Then open `http://localhost:8080`.

## GitHub Pages

Upload the folder contents to a GitHub repository, then enable GitHub Pages from the repository root on the main branch.

## Workflow

1. Download the five-sheet input template from the application.
2. Complete `Employees`, `Shift_Schedule`, `Leaves`, `Permissions` and `Official_Holidays`. Keep the sheet names unchanged.
3. Upload the fingerprint transactions file and the completed employee template.
4. Enter the report period as day/month/year (`dd/mm/yyyy`) and review the calculation settings.
5. Generate, review, edit and export the report.

Leave types supported by the template are Annual, Unpaid, Sick and Other. Permission rows support permission type, start, end and total hours for the day.

Use `Shift_Schedule` when an employee changes shifts for a specific date range. Each row contains employee code, inclusive from/to dates, shift name, start, end and rest days. The application uses the scheduled shift for matching dates and falls back to the employee's default shift otherwise. Date ranges for the same employee must not overlap.

## Included logic

- First valid punch is treated as actual in.
- Last punch is treated as actual out when more than one punch exists.
- Movement type is used to prefer explicit in and out records.
- One punch is reported as actual in with a missing-out note.
- Overnight shifts are supported when shift end is earlier than or equal to shift start.
- A worked overnight shift is counted once against its shift-start date, including its next-day departure.
- Weekly rest days come from the employee template.
- Official holidays take priority when they overlap a weekly rest day.
- Attendance on leave, weekly rest or an official holiday is retained and labelled.
- Overtime is split into regular after-shift overtime, weekly-rest overtime and official-holiday overtime.
- Movement cells show the in/out label with the recorded time, or clearly state that the in/out movement is missing.
- Late arrival, early leave, permission hours and work minutes are calculated without payroll deductions.
- The exported workbook includes `Attendance_Report`, `Employees_Summary`, `Raw_Movements` and copies of the five input sheets.

## Files

- `index.html`: application shell
- `styles.css`: responsive black, gold and silver visual system
- `app.js`: import, calculations, editing, language and export logic
- `assets/logo.png`: supplied brand logo
- `vendor/xlsx.full.min.js`: local spreadsheet parser/exporter

## License

The application source is ready for a repository license of your choice. The bundled spreadsheet library remains under its own upstream license.
