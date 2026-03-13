![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

# SCORM Builder

SCORM Builder is a browser-based tool that converts a CSV file of quiz questions into a ready-to-upload SCORM package — no server, no installation, no build step required. It runs entirely in the browser and supports both SCORM 1.2 and SCORM 2004 4th Edition output. Open `index.html`, upload your questions, configure your course, and download a `.zip` ready to drop into any compliant LMS.

## Features

- Upload a CSV of questions and instantly preview them in a validation table
- Generate SCORM 1.2 or SCORM 2004 4th Edition packages
- Three question types: multiple choice (MC), true/false (TF), and multiple select (MS)
- Theme presets (Corporate, Modern, Dark, High Contrast) with custom colour overrides
- Logo upload embedded as base64 — fully self-contained generated quiz
- Optional retake tracking via `localStorage` with configurable attempt limits
- Optional printable pass certificate with signatory fields
- All-questions or one-at-a-time display modes with progress indicator
- CSV validation with inline error highlighting and non-blocking warnings
- Drag-to-reorder questions in the preview table
- Works offline — open `index.html` directly in any modern browser

## Tech Stack

| Concern | Choice |
|---|---|
| Language | Vanilla HTML5, CSS3, JavaScript (ES6+) |
| Zip generation | [JSZip 3.10.1](https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js) (CDN) |
| Frameworks | None |
| Build pipeline | None |
| Hosting | GitHub Pages / any static host |

## Prerequisites

- A modern browser (Chrome, Edge, Firefox, or Safari — last 2 versions)
- An internet connection on first load (for the JSZip CDN)
- A CSV file of quiz questions (see [CSV Format](#csv-format) below)

## Installation

No installation needed. Either:

**Option A — GitHub Pages**

1. Fork or clone this repo.
2. Go to **Settings → Pages → Branch: main → Folder: / (root)** and save.
3. GitHub Pages serves `index.html` at your Pages URL.

**Option B — Local**

```bash
git clone https://github.com/rrumfelt-matrix/SCORM-Generator.git
# Then open index.html in your browser
```

## Usage

1. **Upload CSV** — Drag and drop your `.csv` file onto the upload zone, or click **Browse**. Questions load into the preview table immediately. Use **Download CSV Template** to get a pre-formatted starting file.
2. **Review questions** — Fix any errors flagged in the preview table (red rows = blocking, yellow = warnings). Drag rows to reorder if needed.
3. **Configure course settings** — Set the course title, pass mark, SCORM version, display mode, and optional retake/certificate features.
4. **Choose a theme** — Pick a preset and optionally override individual colours. Upload a logo (PNG/JPG/SVG, max 400×200px, max 500KB).
5. **Generate** — Click **Generate SCORM Package**. A `.zip` file downloads automatically.
6. **Upload to your LMS** — Upload the `.zip` directly to Paycom Learning or any SCORM-compliant LMS.

## CSV Format

### Columns

| Column | Required | Notes |
|---|---|---|
| `Question` | Yes | Question text |
| `Type` | No | `mc` (default), `tf`, or `ms` |
| `OptionA` | Yes | Answer choice A |
| `OptionB` | Yes | Answer choice B |
| `OptionC` | MC/MS only | Blank for `tf` questions |
| `OptionD` | MC/MS only | Blank for `tf` questions |
| `CorrectAnswer` | Yes | `A`, `B`, `C`, or `D` for mc/tf. Multiple letters for ms (e.g. `ACD`) |
| `Explanation` | No | Shown after answering |

### Question Types

| Code | Description | Options | Answer format |
|---|---|---|---|
| `mc` | Multiple choice — 4 radio buttons | A, B, C, D | Single letter: `A`–`D` |
| `tf` | True/False — 2 radio buttons | A (True), B (False) | `A` or `B` |
| `ms` | Multiple select — checkboxes, select all that apply | A, B, C, D | 2–4 letters: `ACD`, `AB`, etc. |

### Example

```csv
Question,Type,OptionA,OptionB,OptionC,OptionD,CorrectAnswer,Explanation
What is phishing?,mc,A scam email impersonating a trusted source,A type of network firewall,An encrypted VPN tunnel,A software patch process,A,Phishing tricks users into revealing credentials by impersonating trusted entities.
Firewalls block all incoming traffic by default.,tf,True,False,,,A,Firewalls filter traffic based on configured rules — they do not block everything by default.
"Which of the following are social engineering attacks? (Select all that apply)",ms,Phishing,SQL injection,Pretexting,Tailgating,ACD,"Phishing, pretexting, and tailgating are social engineering. SQL injection is a technical attack."
What does MFA stand for?,mc,Multi-Factor Authentication,Managed Firewall Access,Multi-File Archive,Mandatory Firmware Alert,A,MFA requires two or more verification steps — something you know plus something you have or are.
```

### CSV Compatibility Note

Excel on Windows saves CSVs as UTF-8 with BOM or as Windows-1252 encoding. Excel on Mac saves as UTF-8. Both are handled automatically by SCORM Builder. If questions appear garbled, re-save the CSV using **File → Save As → CSV UTF-8** in Excel.

## Configuration

All configuration is done in the builder UI — no config files to edit. Key options:

| Setting | Description |
|---|---|
| Course Title | Appears in the manifest, quiz header, and zip filename |
| Organisation Name | Shown in quiz footer and certificate |
| Pass Mark | Integer 1–100 (default 80%) |
| SCORM Version | SCORM 1.2 or SCORM 2004 4th Edition |
| Question Order | As uploaded (respects drag reorder) or Randomise |
| Display Mode | All questions on one page, or one at a time with navigation |
| Enable Retakes | Track attempts in `localStorage`; set max attempts and messaging |
| Enable Certificate | Show a printable certificate on pass |

## Uploading to Your LMS

### Generic SCORM (1.2 or 2004)

1. In SCORM Builder, select the SCORM version your LMS requires.
2. Click **Generate SCORM Package** — a `.zip` downloads.
3. In your LMS, navigate to the course or content library upload area.
4. Upload the `.zip` directly. Do not unzip it first.
5. The LMS will detect the manifest and configure the SCO automatically.

### Paycom Learning

1. Log in to Paycom and go to **Learning Management → Content Library**.
2. Click **Add Content → SCORM Package**.
3. Upload the `.zip` generated by SCORM Builder (SCORM 1.2 recommended for Paycom).
4. Note: Paycom may enforce a 4MB upload limit. If the package exceeds 4MB, SCORM Builder will warn you — consider using a smaller or no logo to reduce file size.

## Local Testing

Open `index.html` directly in your browser (no web server needed). The generated quiz runs fully — all question types, scoring, and result screens work. Since no SCORM LMS is present, the SCORM API is not found and scoring is not reported. The console will log an info message: `SCORM API not found — running in local preview mode`. This is expected behaviour, not an error.

## Contributing

1. Fork the repo on [github.com/rrumfelt-matrix/SCORM-Generator](https://github.com/rrumfelt-matrix/SCORM-Generator).
2. Create a feature branch: `git checkout -b feature/my-change`
3. Commit your changes and open a pull request.

Please keep all functionality in the single `index.html` file — no npm, no bundler.

## License

MIT License — Copyright (c) 2025 Riley Rumfelt. See [LICENSE](LICENSE) for details.
