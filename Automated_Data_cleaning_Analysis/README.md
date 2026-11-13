# AutoInsight: Automated Data Cleaning, Analysis and Reporting Pipeline (n8n + AI)

An n8n workflow that takes a raw dataset through a web upload form, cleans it, runs statistical analysis, uses an LLM to write an insight report, and emails the finished report to the user. No manual work is needed after the upload.

![n8n](https://img.shields.io/badge/built%20with-n8n-ea4b71)
![Python](https://img.shields.io/badge/analysis-Python-3776ab)
![AI](https://img.shields.io/badge/insights-Google%20Gemini-4285f4)

---

## Problem Statement

Raw data is usually messy, with missing values, duplicates, and inconsistent formats. Cleaning and analysing it by hand takes a lot of time and technical skill. This project automates that work: upload a raw file and receive a data quality summary, statistical profile, and an easy-to-read insight report.

## Features

- **Upload form:** a shareable n8n form where the user uploads a CSV file.
- **Input validation:** an IF node checks that the file contains data before processing.
- **Automated cleaning (pure Python, no libraries):**
  - trims text and converts `NA`, `null`, `-` and blanks to real missing values
  - removes duplicate rows
  - drops columns with more than 50% missing values
  - fills missing numeric values with the median and categorical values with the mode
  - produces a cleaning log of every action taken
- **Statistical analysis (pure Python, no libraries):**
  - mean, median, standard deviation, quartiles, min and max
  - skewness, kurtosis and distribution shape
  - outlier detection with the IQR method
  - Pearson correlation matrix and notable correlations
  - top categories and group averages
  - a data quality score out of 100
- **AI-written report:** Google Gemini turns the statistics into a plain-language report (dataset summary, data quality feedback, insights, outliers, recommendations).
- **Professional HTML report:** KPI cards, quality badge, statistics tables, findings and cleaning log.
- **Email delivery:** the report is sent to the user through Gmail.

## Workflow Architecture

```
On form submission
        |
Extract from File (CSV)
        |
        IF (is the file empty?)
        |-- false --> Gmail (empty file notice)
        |-- true
        |
Code in Python   (cleaning, profiling, quality score)
        |
Code in Python1  (statistical analysis, correlations, key findings)
        |
Basic LLM Chain  <-- Google Gemini Chat Model
        |
Code in JavaScript (Markdown to HTML conversion)
        |
HTML node        (report template)
        |
Gmail            (send report)
```

| Node | Purpose |
|------|---------|
| On form submission | Upload page for the raw file |
| Extract from File | Reads the CSV into rows |
| If | Rejects empty files |
| Code in Python | Cleans the data and builds the column profile |
| Code in Python1 | Computes extended statistics and key findings |
| Basic LLM Chain + Gemini | Writes the insight report from the statistics only (raw rows are not sent to the model) |
| Code in JavaScript | Converts the AI's Markdown into HTML |
| HTML | Builds the final styled report |
| Gmail | Emails the report |

## Tech Stack

- **n8n** (workflow automation)
- **Python** (n8n Code node, no external libraries)
- **JavaScript** (n8n Code node)
- **Google Gemini** (LLM through the Basic LLM Chain node)
- **Gmail** (OAuth2 credential)
- **HTML / CSS** (report template)

## Getting Started

### Prerequisites

- An n8n instance (n8n Cloud or self-hosted)
- A Google account for Gmail (OAuth2 credential)
- A Google Gemini API key (free tier available from Google AI Studio)

### Setup

1. Download `workflow/AutoInsight.json` from this repository.
2. In n8n, choose **Create workflow**, open the menu (**...**), and select **Import from file**.
3. Add your credentials:
   - **Google Gemini Chat Model:** add your Gemini API key.
   - **Gmail:** sign in with Google.
4. In the Gmail node, set the **To** address (or map it to an email field on the form).
5. If you renamed any Code node, update the node names used in the HTML template (it reads from `Code in Python1`).
6. Activate or publish the workflow, then open the form URL from the **On form submission** node.
7. Upload a CSV file and check your inbox for the report.

> Credentials are not included in the exported workflow. Never commit API keys.

## Sample Output

The report email contains:

- summary cards (rows, columns, duplicates removed, cells filled, quality score)
- AI-generated insights and recommendations
- key statistical findings
- numeric column statistics with outlier counts
- notable correlations and categorical overview
- the full cleaning log

Add screenshots here:

```
screenshots/workflow.png
screenshots/report-email.png
```

## Tested With

- Employee dataset (30 rows, mixed numeric and text columns, with missing values and a duplicate)
- Housing dataset (550 rows, numeric columns)

## Known Limitations

- Only CSV files are supported at the moment.
- Missing values are filled with the median or mode only.
- Impossible values (for example a negative age) are not corrected yet. The AI report may flag them, but they are not removed.
- The Python code runs in n8n's Code node, so very large files (hundreds of thousands of rows) may be slow.
- The report is delivered as an HTML email, and some email clients show it with simpler styling.

## Roadmap

- [ ] Send the report as a PDF attachment
- [ ] Attach the cleaned dataset as a CSV
- [ ] Support Excel and JSON uploads (Switch node)
- [ ] Add data validation rules (range checks, impossible values)
- [ ] Add an error-handling workflow that alerts the owner on failures
- [ ] Optional export to Google Sheets or PostgreSQL for Power BI dashboards

## Project Structure

```
AutoInsight/
├── README.md
├── workflow/
│   └── AutoInsight.json
├── code/
│   ├── clean_data.py
│   ├── statistical_analysis.py
│   └── markdown_to_html.js
├── templates/
│   └── report_template.html
├── sample_data/
│   └── employees_raw.csv
└── screenshots/
    ├── workflow.png
    └── report-email.png
```

## Author

**Challa Chaitanya**
B.Tech CSE (Data Science)

- GitHub: [your-username](https://github.com/your-username)
- LinkedIn: [your-profile](https://www.linkedin.com/in/your-profile)
- Email: your-email@example.com

## License

This project is released under the MIT License.
