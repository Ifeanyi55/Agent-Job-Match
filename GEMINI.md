# Agent Job Match

An agent that reformats a reference CV to match the requirements of a specific job advertisement, and drafts a tailored cover letter to accompany it.

## Purpose

Given a job advertisement and a reference CV, produce:
1. A tailored version of the CV that emphasizes the skills, experience, and language most relevant to that job.
2. A cover letter that draws only on the candidate's actual CV content to make the case for fit.

Neither output may fabricate or misrepresent the candidate's actual background.

The human supplies the job ad and the reference CV. The agent handles ingestion, matching, CV formatting, and cover letter drafting.

## Folder Structure

```
Job-Ad/    # source job advertisements (immutable — never modify these)
CV/        # reference CV, as PDF page(s), never modified
Job-CV/    # output: CV tailored to a specific job ad, plus its cover letter
```

## Ingest Workflow

When the user adds a new source to `Job-Ad/` and asks you to ingest it:

1. Read the full source document.
2. Extract the core requirements: required skills, preferred skills, years of experience, key responsibilities, and any explicit keywords (e.g. tools, certifications, methodologies) the ad emphasizes.
3. Read the reference CV in full from `CV/`.
4. Identify which parts of the CV are relevant to the extracted requirements, and note any gaps between the CV and the job ad's requirements (do not invent experience to fill gaps — flag them to the user instead).

## Matching Workflow

**CV**
1. Reorder and re-emphasize the CV's existing content so the most relevant experience and skills appear first.
2. Align terminology with the job ad's language where the underlying experience genuinely supports it (e.g. if the CV says "led a small team" and the ad wants "team leadership," it's fair to surface that explicitly — but never claim a skill or tool the CV doesn't support).
3. Preserve all factual content: dates, titles, employers, and achievements must match the reference CV exactly.
4. Always carry over the candidate's contact information (e.g. phone number, email) from the reference CV onto the matched CV.
5. If the matched CV spans more than one page, do not use page headers.
6. Always arrange work or professional history in descending chronological order (from the most recent to the earliest date).
7. Generate the matched CV as a **PDF** and save it to `Job-CV/`.

**Cover Letter**
1. Open by addressing the hiring manager or recruitment officer (use a name if the job ad provides one; otherwise a role-based greeting such as "Dear Hiring Manager"). Do not include the candidate's own contact information (phone number, email, address) anywhere in the letter.
2. Name the specific role and company from the job ad — no generic openings.
3. Select 2–3 concrete achievements from the CV that most directly address the job ad's top requirements (prioritize the same requirements identified during ingest). Use specifics — numbers, outcomes, technologies — over general claims.
4. Address any notable gap between the CV and the job requirements honestly, framing it in terms of adjacent/transferable experience rather than glossing over it.
5. Keep it to one page: 3–4 short paragraphs (opening, 1–2 evidence paragraphs, closing call to action).
6. Every claim in the letter must trace back to something actually present in the reference CV — do not introduce new skills, employers, or achievements.
7. Generate the letter as a **Word document (.docx)** and save it alongside the matched CV in `Job-CV/`.

## Output Naming

Create a subfolder per job inside `Job-CV/`, and save that job's outputs together inside it:

```
Job-CV/<job-title>-<company>/
  cv.pdf
  cover-letter.docx
```

This keeps each job application self-contained and avoids collisions between jobs with similar titles or companies.
