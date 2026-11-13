# MSBA Professional Development Workshop Series: Responsible AI System Design

**When and How to Use ML, Agents, or Neither**
Abhi Yarlagadda, Data Scientist, Data Science Research Services (DSRS), Gies College of Business, University of Illinois Urbana-Champaign

This repo holds everything for the hands-on build challenge: the brief, the data, and a starter notebook.

## The challenge (20 minutes)

A 20-person company posted one Data Analyst role and is buried in applications. Build the first-pass screen described in [PRD.md](PRD.md):

| | Requirement |
|---|---|
| **R1** | Extract a structured record from each resume |
| **R2** | Reject applicants who fail the hard requirements |
| **R3** | Rank the rest by fit to the job description |

For each requirement, decide: **ML, agent, or neither?** Then build it.

### Rules

- Work on your own.
- Any AI tool is allowed: chat assistants, coding assistants, APIs, libraries.
- No solution is provided. There is more than one good answer.
- If you use an API (for example Lumen, below), load your key from an environment variable. Never paste a key into the notebook or commit it.

### Bring to the debrief

1. Your choice for each requirement (ML, agent, or neither) and why
2. Your top 10 candidates (`outputs/results.csv`)
3. One thing that broke

## Setup

**Option A: local (Python 3.10+)**

```bash
git clone https://github.com/whyabhinay/msba-workshop-talentsift.git
cd msba-workshop-talentsift
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab notebooks/01_build_challenge.ipynb
```

**Option B: Google Colab**

Open a new Colab notebook and run:

```python
!git clone https://github.com/whyabhinay/msba-workshop-talentsift.git
%cd msba-workshop-talentsift
!pip install -q -r requirements.txt
```

Then open `notebooks/01_build_challenge.ipynb` from the file browser.

## Using Lumen (optional)

Lumen is the university-hosted LLM service at NCSA. You can use it for any part of the build.

1. Go to [lumen.ncsa.illinois.edu](https://lumen.ncsa.illinois.edu) and log in with your NetID.
2. **Profile** tab: generate an API key.
3. **Models** tab: select `gemma-4-31b-it`, then click **Use this model**.
4. You don't need to copy the code: the notebook's "Optional: use Lumen" section already has it, as an `ask_lumen()` helper.

**Keep your key out of the code.** Store it as `LUMEN_API_KEY`:

- **Colab:** open the Secrets panel (key icon), add `LUMEN_API_KEY`, and turn on notebook access.
- **Local:** create a `.env` file in the repo root containing `LUMEN_API_KEY=...`. `.env` is already in `.gitignore`.

The notebook loads the key for you. Test it with `print(ask_lumen("Hello!"))`. Working in VS Code instead? Lumen's code reads the same `LUMEN_API_KEY` environment variable.

## Troubleshooting

- **Colab asks for access to a secret:** click *Grant access*. The notebook only reads `LUMEN_API_KEY`.
- **"No LUMEN_API_KEY found" locally:** the `.env` file must sit in the repo root (next to this README), not in `notebooks/`.
- **"Could not read" is not zero:** some resumes did not give up their text to the basic reader. Open a few and look at why.
- **`ModuleNotFoundError`:** run `pip install -r requirements.txt` again in the same environment as your notebook.

## What's in the repo

```
msba-workshop-talentsift/
├── README.md
├── PRD.md                          The brief: what to build and how it is judged
├── requirements.txt
├── data/
│   ├── job_description.md          The posting you are screening against
│   ├── applications.csv            One row per applicant, with application form answers
│   └── resumes/                    120 resumes, PDF and DOCX
├── notebooks/
│   ├── 01_build_challenge.ipynb    Start here
│   └── 02_fairness_audit.ipynb     Used later in the session
└── outputs/                        Your results.csv goes here
```

## About the data

Every applicant is **fictional**. Names were generated, emails use `example.com`, and phone numbers use the 555-01xx range reserved for fiction. No real person's resume is in this repo.

The resumes are deliberately realistic, which means they are deliberately messy. Expect surprises.

## After the session

Audit your ranker for fairness: run `git pull`, then open `notebooks/02_fairness_audit.ipynb` and run all cells. It needs one more data file, which is added to this repo after the workshop.
