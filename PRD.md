# PRD: TalentSift, first-pass resume screen

**Status:** Draft for workshop build · **Owner:** Hiring manager, Prairie Lantern Analytics

## 1. Problem

Prairie Lantern Analytics is a 20-person company with no recruiter. It posted one Data Analyst role ([job description](data/job_description.md)) and received **120 applications in a week**. The hiring manager reads every resume by hand and is falling behind on her actual job.

She wants a first-pass screen that turns the pile into a short list she can review.

## 2. Users

- **Hiring manager (primary):** reviews the short list and decides who to interview.
- **Applicants (affected):** never see the tool, but every one of them is judged by it.

## 3. Goals

- Turn every application into a consistent, structured record.
- Remove applicants who do not meet the posting's hard requirements, with a stated reason.
- Produce a ranked list and a top 10 short list for human review.

## 4. Non-goals

- The tool does not make hiring decisions. A person reviews the short list.
- The tool does not contact applicants.

## 5. Requirements

### R1: Extract

Turn each resume (PDF or DOCX) into a structured record.

- Fields: name, email, phone, city and state, education (degree, school, year), work history (title, company, start, end), skills.
- Works on every file in `data/resumes/`.
- If a file cannot be read, say so. Do not skip it silently.

### R2: Filter

Reject applicants who fail the posting's hard requirements.

- Hard requirements: authorized to work in the US, and able to work on-site in Chicago three days a week.
- The answers are in `data/applications.csv` (applicants answered these questions on the application form).
- Every rejection has a reason a person can read.
- The same input always gives the same result.

### R3: Rank

Rank the remaining applicants by fit to the job description.

- Give every applicant who passed R2 a score.
- Produce a ranked list and mark the top 10 as the short list.
- For each short-listed applicant, give the main reasons for the score.

## 6. Inputs and outputs

| Input | Location |
|---|---|
| Resumes (PDF, DOCX) | `data/resumes/` |
| Application form answers | `data/applications.csv` |
| Job description | `data/job_description.md` |

**Output:** `outputs/results.csv` with one row per application:

| Column | Meaning |
|---|---|
| `candidate_id` | From `applications.csv` |
| `name` | Extracted in R1 |
| `passed_filter` | True / False from R2 |
| `filter_reason` | Why an applicant was rejected (blank if passed) |
| `score` | Fit score from R3 (blank if rejected) |
| `rank` | 1 = best (blank if rejected) |
| `shortlisted` | True for the top 10 |
| `reasons` | Main reasons for the score |

## 7. Constraints

- Small company, small budget: cost per application matters.
- Results must be explainable to the hiring manager, and to an applicant who asks.
- Treat the data as personal information: do not send it anywhere you would not send a real resume. Lumen, the university-hosted LLM service, is fine. (Every applicant in this sample is fictional.)

## 8. Success

- Every application has a record, or a stated reason it could not be read.
- No applicant who meets the hard requirements is rejected by R2.
- The hiring manager agrees the top 10 is worth her time.

## 9. Open questions

Write down any question you would ask the hiring manager before shipping this.
