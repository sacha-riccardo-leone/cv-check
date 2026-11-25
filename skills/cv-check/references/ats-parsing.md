# What makes an ATS misread a document

Severity: **Critical** = data likely lost or garbled. **Medium** = some parsers struggle. **Low** = polish.

| Problem | Severity | Why | Fix |
|---|---|---|---|
| Scanned / image-only PDF (no selectable text) | Critical | Nothing to parse | Export a text PDF from Word/Docs, or send .docx |
| Contact details only in the page header/footer | Critical | Many parsers skip headers and footers | Put name, email, phone in the body, first lines |
| Contact details or skills inside images, icons or a sidebar graphic | Critical | Images are not read | Plain text |
| Two or three columns | Medium–Critical | Text gets merged across columns, dates attach to the wrong job | Single column for the main content |
| Tables used for layout | Medium | Cells read out of order | Plain paragraphs and bullets |
| Text boxes / shapes | Medium–Critical | Often skipped entirely | Normal text |
| In the extracted text, dates, levels or locations come out as a separate line away from their item (e.g. `2022–2025 2020–2021 2018–2022` after all diplomas, or `C1 B2` after all languages) | Medium | The parser can't tell which date or level goes with which entry | Put each date/level in the same block of text as its item (same row, not a separate column) |
| Two headings merged on one line in the extracted text | Medium | Section boundary lost | One heading per section |
| Creative section titles ("My Journey", "Ce qui me fait vibrer") | Medium | Parser can't map them to Experience / Education / Skills | Standard titles: Experience, Education, Skills, Languages, Certifications / Expérience professionnelle, Formation, Compétences, Langues |
| Inconsistent or unclear dates ("Summer '22", years only on some lines) | Medium | Durations miscomputed; "years of experience" filters misfire | Same format everywhere: `MM/YYYY – MM/YYYY` or `Jan 2022 – Mar 2024` |
| Skill bars, stars, percentages ("Python ●●●○○") | Medium | Graphic is unreadable; the rating means nothing to a recruiter | List the skill, add context ("Python: 3 years, data pipelines") |
| Unusual fonts, symbols as bullets, emoji | Low | Can turn into `?` or junk characters | Standard fonts, simple bullets |
| Acronym only, or full term only ("SEO" vs "search engine optimization") | Low | Recruiter may search the other form | Use both once: "Search engine optimization (SEO)" |
| File named `CV.pdf` / `document(3).pdf` | Low | Looks careless to humans | `Firstname-Lastname-CV-<Role>.pdf` |
| More than 2 pages (resume), more than 1 page (cover letter) | Low | Human attention, not ATS | Cut the oldest / least relevant |
| .docx vs .pdf | — | Both are fine for modern ATS if text-based; follow the posting if it asks for one | — |

Country notes:
- **US / UK / Canada**: no photo, age, date of birth, marital status, nationality (discrimination risk; some recruiters discard such CVs).
- **France / Belgium / Switzerland**: photo and age are optional and common; leave the choice to the user, but mention that large international companies often prefer none.
