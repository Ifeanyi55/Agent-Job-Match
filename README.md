# Agent Job Match

An [Antigravity](https://antigravity.google/) agent that takes a reference CV and a job advertisement, and produces a tailored CV and cover letter for that specific role — without inventing or misrepresenting the candidate's actual experience.

## Objective

Job applications usually need the same underlying CV reshaped for every role: different requirements emphasized, different language mirrored, sometimes a cover letter written from scratch. Doing this by hand for every application is repetitive and easy to get inconsistent.

This project automates that reshaping. The human provides two things — a reference CV and a job advertisement — and the agent handles the rest: reading the job ad, identifying what it's actually asking for, matching that against the CV, and producing a tailored CV and cover letter as ready-to-send files.

The agent is not permitted to fabricate experience. Every claim in the output must trace back to something present in the reference CV; where the CV falls short of a requirement, the agent flags the gap rather than inventing a fix.

## How It Works

1. **Ingest** — the agent reads a new job ad and extracts its core requirements (skills, experience level, responsibilities, keywords), then reads the reference CV and maps what's relevant.
2. **Match** — the agent reorders and re-emphasizes the CV around those requirements, and drafts a cover letter built from the CV's strongest, most relevant points.
3. **Output** — a tailored CV (PDF) and cover letter (Word doc) are generated for that job and saved together.

Full step-by-step rules for each stage live in [`GEMINI.md`](./GEMINI.md) — that file is the agent's operating instructions; this README is the project overview.

## Folder Structure

```
Job-Ad/                          # source job advertisements (immutable — never modified)
CV/                               # reference CV, as PDF page(s) (immutable — never modified)
Job-CV/                           # generated outputs, one subfolder per job
  <job-title>-<company>/
    cv.pdf                        # tailored CV for this job
    cover-letter.docx             # tailored cover letter for this job
```

- `Job-Ad/` and `CV/` are source material the human curates; the agent only reads from them.
- `Job-CV/` is entirely agent-generated. Each job gets its own subfolder so multiple applications can coexist without overwriting one another.

## Output Rules

**CV (PDF)**
- Preserves all factual content exactly as in the reference CV — dates, titles, employers, achievements.
- Reordered and re-emphasized toward the job ad's requirements, with terminology aligned where genuinely supported by the underlying experience.
- Always includes the candidate's contact information (phone, email) carried over from the reference CV.
- No page headers if the CV spans more than one page.

**Cover Letter (Word / .docx)**
- Addressed to the hiring manager or recruitment officer (by name if the job ad provides one, otherwise a role-based greeting).
- Contains no candidate contact information (no phone, email, or address).
- One page: opens by naming the role and company, builds the case with 2–3 concrete achievements from the CV, honestly addresses any experience gaps, and closes with a call to action.
- Every claim traces back to the reference CV — no invented skills or experience.

## Usage

1. Drop a job advertisement into `Job-Ad/`.
2. Make sure the reference CV is present in `CV/`.
3. Ask the agent to ingest the new job ad.
4. Ask the agent to generate the matched CV and cover letter.
5. Find both files under `Job-CV/<job-title>-<company>/`.

## Design Principles

- **No fabrication.** The agent reshapes and reframes; it never invents experience, skills, or achievements not present in the source CV.
- **Source immutability.** `Job-Ad/` and `CV/` are read-only from the agent's perspective — all generated content lives in `Job-CV/`.
- **Self-contained outputs.** Every job application's files live together in their own subfolder, so applications never collide or overwrite each other.
- **Honesty over polish.** Gaps between the CV and a job's requirements are surfaced, not hidden.
