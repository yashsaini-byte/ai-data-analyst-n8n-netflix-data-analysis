# ai-data-analyst-n8n-netflix-data-analysis

# Automated KPI Reporting System

An n8n workflow project that turns CSV files in Gmail into a database, an Excel report, and an email-based KPI assistant.

**Tools:** n8n, PostgreSQL, Google Sheets, Google Drive, Gmail, Claude (Anthropic), Gemini (Google)

> **Note:** All data in this repo is **[sample / anonymized]** data. *(Edit this line to be truthful.)*

---

## What problem does this solve?

Before this project, KPI reporting was manual:

- Download the CSV from email
- Clean it in Excel
- Build the report by hand
- Answer one-off KPI questions by writing SQL or asking an analyst

This project automates all four steps.

---

## How it works

### Workflow 1: CSV to Excel report

1. **Gmail Trigger** watches for new emails with a CSV attachment.
2. **Extract from File** reads the CSV.
3. **Postgres** inserts the rows into the `[table_name]` table.
4. **Google Sheets** appends the same rows to a sheet.
5. **AI Agent (Gemini)** reads the data and writes a summary report. It uses Postgres chat memory.
6. **Google Drive** downloads the sheet as an Excel file.
7. **Gmail** emails the Excel report to **[recipient]**.

### Workflow 2: KPI questions by email

1. **Gmail Trigger** watches for emails with the label/subject **[KPI request label]**.
2. **KPI Analyst (Claude)** reads the question and writes a SQL query.
3. **run_sql_query tool** runs the query on PostgreSQL.
4. **Gmail** replies with the answer.
5. **Gmail** marks the request as read, so it is not answered twice.

---

## Data

- **Source:** [e.g. sample sales data from Kaggle / synthetic data I generated]
- **Rows:** [number]
- **Columns:** [list main columns]
- **Sample file:** `sample_data/sample.csv`

### Database table

```sql
CREATE TABLE [table_name] (
    -- paste your real table definition here
    id SERIAL PRIMARY KEY,
    order_date DATE,
    region TEXT,
    product TEXT,
    revenue NUMERIC
);
```

---

## KPIs calculated

| KPI | Definition | Why it matters |
|-----|------------|----------------|
| [Total revenue] | [SUM of revenue] | [reason] |
| [KPI 2] | [definition] | [reason] |
| [KPI 3] | [definition] | [reason] |

---

## Example questions and what the AI did

| Email question | SQL the AI wrote | Correct? |
|----------------|------------------|----------|
| "[What was total revenue last month?]" | `SELECT ...` | [Yes / checked by hand] |
| "[Which region sold the most?]" | `SELECT ...` | [Yes / checked by hand] |
| "[Question 3]" | `SELECT ...` | [Yes / No] |

*Paste real examples from your runs. Do not make them up.*

---

## Screenshots

- `screenshots/workflow-1.png`: CSV to Excel report flow
- `screenshots/workflow-2.png`: KPI question flow
- `screenshots/email-reply.png`: an example question and the AI reply

---

## Setup

### What you need

- n8n (cloud or self-hosted)
- PostgreSQL database
- Google account (Gmail, Sheets, Drive access)
- Anthropic API key (Claude)
- Google AI API key (Gemini)

### Steps

1. Create the table using the SQL above.
2. **Create a read-only database user** for the Claude SQL tool (see Safety below).
3. In n8n, import the workflow files from `/workflows`.
4. Add your credentials in n8n: Gmail, Google Sheets, Google Drive, Postgres, Anthropic, Gemini.
5. Update the placeholders: table name, sheet ID, email addresses, Gmail label.
6. Turn on both workflows.
7. Send a test email with a sample CSV, then send a test KPI question.

---

## Safety

- The SQL tool uses a **read-only** database user. The AI can run `SELECT` only. It cannot change or delete data.
- No real credentials are stored in the workflow files. Add your own in n8n.
- **[Add anything else you did, e.g. query limits, restricted tables.]**

---

## Limitations

Be honest here. Interviewers will respect it.

- The AI can write a wrong query that still runs. KPI answers should be checked against **[how you validated: manual SQL, spot checks]**.
- If the CSV format changes (new or renamed columns), the insert step will fail.
- **[Does it handle duplicate rows? If not, say so.]**
- **[Does it handle empty or bad values? If not, say so.]**

## Future improvements

- Add data checks before the insert (column names, data types, duplicates)
- Send an alert email when a workflow fails
- Add a dashboard (Looker Studio / Power BI) on top of the database

---

## Results

*Only fill in what you actually measured.*

- Files handled: **[number]**
- Time to produce a report: **[before]** → **[after]**
- Question answer time: **[before]** → **[after]**

---

## Project structure

```
.
├── README.md
├── workflows/
│   ├── csv-to-report.json
│   └── kpi-analyst.json
├── sample_data/
│   └── sample.csv
├── sql/
│   └── create_table.sql
└── screenshots/
```

## Author

Yash Saini| yashsaini1947@gmail.com
