# Syllabus Analyzer

A browser-based tool for the David Eccles School of Business at the University of Utah. Upload course syllabi, extract structured curriculum data using the Anthropic API, and build a queryable database across courses and semesters.

**Live app:** https://n8thanielz.github.io/syllabus-analyzer/

---

## What it does

- Accepts PDF, Word (.docx), Excel (.xlsx), and plain text syllabi
- Extracts structured fields using Claude (learning outcomes, pedagogy, engagement activities, grading, policies, and more)
- Identifies NACE career readiness competencies with evidence from the syllabus
- Handles multi-section syllabi: one row per section, with shared content and section-specific scheduling fields
- Stores results in a filterable, searchable database
- Exports to CSV for Excel, Airtable, or Power BI

## Setup

1. Open the [live app](https://n8thanielz.github.io/syllabus-analyzer/) in Chrome or Edge
2. Go to **Settings** and paste the Anthropic API key
3. Select a model (Sonnet is recommended; Haiku is faster and cheaper)
4. Save

The API key is stored only in your browser. Nothing is sent to any server other than the Anthropic API.

## Basic workflow

1. **Upload** -- drag syllabus files onto the Upload tab, or click to browse. The app guesses course, semester, and year from the filename. Correct anything wrong before running.
2. **Extract** -- click Run Extraction. Each syllabus takes roughly 20 to 60 seconds.
3. **Review** -- go to Results. Yellow cells were flagged as uncertain by the AI. Click any row to open the editor and correct it. Check the box to mark it spot-checked.
4. **Save** -- click Save Database File and store the JSON file in the shared folder next to the original syllabi. This file is the persistent database; load it back anytime on any computer.
5. **Export** -- Export CSV sends the current filtered view to a spreadsheet.

## Features

- **Batch processing** with a queue, progress bar, and stop button
- **Skills Matrix tab** showing which courses have evidence for each of the 8 NACE career readiness competencies
- **Re-extract Selected** to backfill rows when new fields are added later
- **Column visibility toggles** for wide tables
- **Duplicate detection** warns before re-extracting a file already in the database
- **Schema migration** so old database files load cleanly when fields are added
- **Editable extraction fields** on the Fields tab -- add, remove, or redefine what the AI looks for without touching code

## Default extraction fields

| Field | What it captures |
|---|---|
| Learning Outcomes | Stated goals and objectives |
| Topics Covered | Subject-matter topics from the schedule |
| Assessment Methods | Graded components with weights |
| Class Format | Delivery mode (in-person, online, hybrid) |
| Pedagogy | Instructional approach (lecture, case-method, seminar, etc.) |
| Engagement Activities | Hands-on student activities beyond lecture and reading |
| Required Materials | Textbooks, software, subscriptions |
| Attendance Policy | Tracking method, absence limits, grade impact |
| Classroom Policies & Conduct | Device policy, participation, professionalism expectations |

Core fields extracted on every syllabus regardless of field settings: course number, section, semester, year, instructor, meeting pattern, location, final exam date, NACE durable skills, and AI confidence flags.

## Technical notes

Single HTML file with no build step, no framework, and no backend. Two CDN dependencies: [mammoth.js](https://github.com/mwilliamson/mammoth.js) for Word files and [SheetJS](https://sheetjs.com) for Excel files. PDFs are sent directly to the Anthropic API using native document support. Data is auto-saved to browser localStorage; the JSON database file is the permanent record.
