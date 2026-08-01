# AI Resume Screening Engine

> Automatically processes candidate resumes from Gmail, extracts PDF content, evaluates candidates against job requirements using AI, stores results in Google Sheets, and notifies HR about relevant applicants.

---

## 📌 Business Problem

HR teams spend significant time manually reviewing resumes, comparing candidates with vacancy requirements, identifying missing skills, and deciding who should proceed to the next hiring stage.

This process is repetitive, time-consuming, and difficult to scale when the company receives a large number of applications.

The goal of this project is to automate the initial resume screening process while keeping the final hiring decision under human control.

---

## 🚀 Solution Overview

The workflow automatically receives candidate resumes from Gmail, downloads the attached PDF, extracts its text, and sends the resume content to an AI model.

The AI evaluates the candidate against predefined requirements for an AI Automation Specialist position and returns structured data, including:

- relevant experience;
- technical skills;
- matched required skills;
- missing required skills;
- strengths;
- score;
- candidate status;
- explanation of the decision.

The workflow then checks whether the candidate already exists in Google Sheets.

- Existing candidates are updated.
- New candidates are added as new records.

Finally, candidates are routed by status:

- `approved` — HR receives a Telegram notification;
- `manual_review` — HR receives a manual review notification;
- `rejected` — the result is stored without sending a notification.

---

## 🏗 Workflow Architecture

![Workflow Architecture](assets/workflow-architecture.png)

---

## 🔄 Workflow

Gmail Resume Trigger
    ↓
Get Resume Email
    ↓
Extract Resume PDF
    ↓
Prepare Resume Text
    ↓
AI Candidate Evaluation
    ↓
Prepare Candidate Data
    ↓
Find Existing Candidate
    ↓
Candidate Exists?
    ├── Yes → Update Candidate Record
    └── No  → Create Candidate Record
              ↓
Route Candidate Status
    ├── Approved → Notify HR
    ├── Manual Review → Notify HR
    └── Rejected → End

---

## ✨ Features

- Automatic resume intake from Gmail
- PDF attachment download
- Text extraction from PDF resumes
- AI-powered candidate evaluation
- Structured JSON output using JSON Schema
- Candidate scoring from 0 to 100
- Candidate classification:
  - approved
  - manual_review
  - rejected
- Duplicate candidate detection by email
- Automatic candidate record update
- New candidate creation
- Google Sheets candidate database
- Telegram notifications for relevant candidates

---

## 🛠 Tech Stack

- n8n
- Gmail API
- OpenAI API
- Google Sheets API
- Telegram Bot API
- JSON Schema
- PDF text extraction

---

## 🎯 Key Skills Demonstrated

- Business process automation
- Workflow architecture design
- Email-triggered automation
- Binary file processing
- PDF data extraction
- Prompt engineering
- Structured AI output
- JSON Schema design
- Data normalization
- Candidate scoring logic
- Conditional routing
- Duplicate detection
- Update-or-create logic
- Google Sheets integration
- Telegram notification automation

---

## 📂 Repository Structure

```text
ai-resume-screening-engine/
├── README.md
├── workflow/
│   └── ai-resume-screening-engine.json
├── assets/
│   └── workflow-architecture.png
├── prompts/
│   └── system-prompt.md
└── schemas/
    └── candidate-evaluation-schema.json
