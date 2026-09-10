# AI Resume Screening Engine

[Русская версия](README_RU.md)

AI-assisted resume screening workflow built with n8n. It receives PDF resumes from Gmail, evaluates candidates, stores them in Google Sheets, validates the official status through a deterministic API, and notifies HR.

---

## Business Problem

Manual resume screening is repetitive, time-consuming, and difficult to scale. It can also produce inconsistent candidate decisions.

This workflow automates the initial evaluation while keeping status rules deterministic and the final hiring decision under human control.

---

## Workflow Architecture

```text
Gmail → PDF Extraction → AI Evaluation → Prepare Candidate Data
                                      ├→ Find Candidate by Email
                                      │    ├→ Update Existing
                                      │    └→ Create New
                                      │           ↓
                                      │    pending_validation
                                      │           ↓
                                      │       Merge Input 1
                                      │
                                      └→ Validation API
                                           ├→ Success → Merge Input 2
                                           └→ Error → Notify HR

Merge → Save Official Status → Route Status
                              ├→ approved → Notify HR
                              ├→ manual_review → Notify HR
                              └→ rejected → Stop
```

---

## Workflow

![AI Resume Screening Engine workflow](workflowResume.png)

---

## How It Works

1. Gmail receives an email with a PDF resume.
2. n8n extracts the text from the PDF.
3. OpenAI evaluates experience, skills, strengths, missing requirements, and produces a score from 0 to 100.
4. A strict JSON Schema guarantees predictable structured fields.
5. Google Sheets finds the candidate by email and either creates or updates the record.
6. The record is temporarily saved with `pending_validation`.
7. The external API converts the AI score into an official status.
8. The same candidate row is updated with the API status.
9. HR receives the appropriate Telegram notification.

---

## Validation Rules

| Score  | Official status |
| ------ | --------------- |
| 0–49   | `rejected`      |
| 50–89  | `manual_review` |
| 90–100 | `approved`      |

The API returns HTTP `400` for invalid score data. Other API errors follow a separate technical-error notification path.

Validation API repository:

[candidate-validation-api](https://github.com/AlexZaytsev-ai/candidate-validation-api)

---

## Key Architecture Decisions

* AI analyzes the resume and proposes a score but does not assign the official status.
* The Validation API is the source of truth for candidate status.
* Candidate email is the business key used to prevent duplicates.
* Candidates are saved independently of the API response.
* Until validation succeeds, the status remains `pending_validation`.
* API failures are sent to HR for manual processing.
* The final hiring decision remains under human control.

---

## Tech Stack

| Technology            | Purpose                         |
| --------------------- | ------------------------------- |
| n8n                   | Workflow automation             |
| Gmail API             | Resume intake                   |
| OpenAI API            | Resume analysis and scoring     |
| JSON Schema           | Strict structured output        |
| Google Sheets API     | Candidate registry              |
| Node.js / Express API | Deterministic status validation |
| Telegram Bot API      | HR notifications                |

---

## Import and Setup

The public export does not contain credentials, Google Sheets IDs, Telegram chat IDs, or a private API URL.

1. Import `ai-resume-screening-workflow.json` into n8n.
2. Configure Gmail, OpenAI, Google Sheets, and Telegram credentials.
3. Select the candidate spreadsheet in all Google Sheets nodes.
4. Replace `YOUR_VALIDATION_API_URL` with the URL accessible from n8n.
5. Replace the Telegram chat ID placeholders.
6. Test new candidate, existing candidate, and API error scenarios.
7. Activate the workflow after successful testing.

---

## Tested Scenarios

* A new candidate is created with a numeric score.
* An existing candidate is updated without creating a duplicate.
* The initial `pending_validation` status is replaced by the official API status.
* A score of `2` produces `rejected`.
* Candidate data remains stored if the API fails.
* HTTP `400` and other API errors follow separate notification paths.

---

## Author

Alexander Zaytsev

AI Automation Engineer

* GitHub: https://github.com/AlexZaytsev-ai
* Email: [polonix315@gmail.com](mailto:polonix315@gmail.com)
