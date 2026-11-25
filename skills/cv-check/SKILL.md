---
name: cv-check
description: Check a resume/CV or cover letter (lettre de motivation) before applying. Finds what makes an ATS misread it (layout, sections, file format), keywords from the job posting that are missing, and clichés or AI-sounding phrases that make recruiters skim past. Then walks through each fix with the user, one choice at a time. Keeps a personal list of words to flag that grows when the user adds to it. Works in English and French. Use when the user says /cv-check, shares a resume, CV, cover letter or lettre de motivation, asks "is my CV ATS-friendly", "check my cover letter", "does this sound like AI", or is about to apply for a job.
---

# CV Check

Most "ATS banned word" lists online are a myth. ATS software (Workday, Greenhouse, Lever, iCIMS, Taleo, SmartRecruiters…) does not reject a word. It **parses** the file into fields, then recruiters **search and filter** by keywords, and some tools **rank** candidates against the job posting. A document fails for three real reasons, and this skill checks all three:

1. **Parsing**: the ATS can't read it properly (columns, tables, text boxes, headers, images).
2. **Keywords**: it lacks the words a recruiter searches for (taken from the job posting).
3. **Humans**: it gets through, but a recruiter skims past clichés and AI-sounding phrasing.

Never claim a phrase "gets you auto-rejected". Say what it costs: misread, not found, or skimmed past.

## Commands

- `/cv-check <file or pasted text>`: full check (default)
- `/cv-check add <word or phrase> [-> better alternative] [: why]`: add to the personal list
- `/cv-check remove <word>`: remove from the personal list
- `/cv-check words`: show the personal list

## Personal word list

Lives at `~/.claude/cv-check/my-words.md` (outside the plugin, so updates never erase it). Create it on first `add` with a header and this line format:

```
- <phrase> | lang: en|fr|any | instead: <alternative or "cut"> | why: <reason> | added: <YYYY-MM-DD>
```

Read it at the start of every check. Its entries are flagged like the built-in ones and marked "(your list)". If the user rewrites the same kind of phrase twice in a check, or says something like "I hate that word", offer to add it to the list.

## Run a check

### 0. Get the inputs
- The document. If the user didn't give a path and you search for one, never truncate the search results; when several CVs match, list the most recent few with their dates and ask which one. Don't guess: old exports and other people's CVs are common.
- `.pdf`: judge it by its **extracted text**, not by how it looks. Read tools often return only a picture of the page. Extract the raw text in reading order (`pdftotext file.pdf -` without `-layout`, or `pypdf`): that is roughly what an ATS gets. If almost no text comes out, it is an image and **no ATS can read it**: that is the first, critical finding. `.docx`: extract text with the docx skill or `pandoc`, and also look at the raw `word/document.xml` for tables (`<w:tbl>`), text boxes (`<w:txbxContent>`), and contact details only in `header*.xml`/`footer*.xml`.
- The job posting. Ask for it once if not given ("Paste the job ad and I'll check keywords too"). If the user has none, skip step 2.
- Detect the language (EN/FR) and the target country if obvious. Conventions differ (photo and age are common in France, a liability in the US/UK).

### 1. Parsing
Use `references/ats-parsing.md`. Report only what applies.

### 2. Keywords (only with a job posting)
- Pull the hard requirements: job title, tools, skills, certifications, languages, years of experience, degree.
- For each one: present in the document (exact or close wording), present but phrased differently (suggest using the posting's exact term), or missing.
- Missing ones: never invent experience. Ask whether the user actually has it; only then suggest where to add it.
- Mirror the posting's job title in the headline/summary if the user's experience matches it.

### 3. Wording
Scan with `references/words-en.md` or `references/words-fr.md`, plus the personal list. Also flag, without a list:
- Bullets with no result: no number, scope or outcome ("Responsible for…").
- The same verb starting 3+ bullets.
- Cover letter: an opening that could be sent to any company; nothing specific about this company or role.
- AI rhythm: every bullet the same length, triplets everywhere, em-dash chains, a summary built only of adjectives.

Don't flag a word when it is the actual skill or job title (e.g. "Leadership" as a required competency, "dynamique" in "analyse dynamique").

### 4. Fix it together

Sort findings: critical parsing problems first, then missing keywords, then wording. Show the summary first:

```
CV check: <file>
Parsing: <n> issues (<n> critical) · Keywords: <matched>/<total> from the posting · Wording: <n> flags
```

Then go through the fixes with the **AskUserQuestion** tool, up to 4 findings per call, each finding one question:
- `question`: quote the original line and say in a few words what's wrong.
- `header`: short tag like "Cliché", "No result", "Keyword", "Layout".
- `options`: 2 or 3 concrete rewrites that keep the user's real facts (first one = your pick, marked "(Recommended)"), plus "Keep as is". The user can type their own version through "Other".
- For keyword gaps, the options are "I have this: add it", "I don't have it: skip".
- Never put a number or claim in a rewrite that the user didn't give. When a result is missing, ask for it ("How many users / how much time saved?") instead of inventing one.

After each batch, apply the chosen edits. If AskUserQuestion is unavailable, show the same choices as a numbered list and wait.

Finally ask, in one AskUserQuestion: "Add any of these to your personal list?", listing up to 4 phrases the user chose to replace (multiSelect), and add the selected ones.

### 5. Save
- `.md`/`.txt`: edit in place.
- `.docx`: edit with the docx skill if available, keeping formatting; else write the corrected text to `<name>-checked.md` next to it.
- `.pdf`: write `<name>-checked.md` (or `.docx` with the docx skill) next to it; say the user should export a new PDF from their source file.
- End with what's left: issues the user chose to keep, missing keywords they didn't have, and anything not checked.
