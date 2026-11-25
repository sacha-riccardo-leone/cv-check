# cv-check

**A Claude Code skill that checks your resume or cover letter before you apply, then fixes it with you one choice at a time.** English and French.

Most "ATS banned words" lists are a myth: applicant tracking systems don't reject words. A resume fails for three real reasons, and `cv-check` looks at all three:

1. **Parsing**: the ATS misreads it. Image-only PDFs, columns, tables, text boxes, contact details in the page header, dates that end up detached from their jobs.
2. **Keywords**: it's missing the terms from the job posting that recruiters search for.
3. **Humans**: it gets through, but a recruiter skims past clichés and AI-sounding phrasing ("results-driven team player", "delve", "fort de", "force de proposition").

It checks a PDF by the text an ATS would actually extract, not by how the page looks.

## Install

```
/plugin marketplace add sacha-riccardo-leone/cv-check
/plugin install cv-check@cv-check
```

Or copy `skills/cv-check/` into `~/.claude/skills/`.

## Use

| Command | What it does |
|---|---|
| `/cv-check resume.pdf` | Full check. Paste the job ad too and it checks keywords. |
| `/cv-check add <phrase> -> <better> : <why>` | Add a phrase to your personal list |
| `/cv-check remove <phrase>` | Remove a phrase from your list |
| `/cv-check words` | Show your list |

After the summary, it goes through each fix in Claude Code's question window: two or three rewrites built from **your** facts, plus "Keep as is" and a free-text option. It never invents experience or numbers. When a bullet has no result, it asks you for one.

## Your list grows with you

Your personal phrases live in `~/.claude/cv-check/my-words.md`, outside the plugin, so updates never erase them. At the end of each check, it offers to add the phrases you just replaced.

## Contributing

Recruiter or HR person? The word lists in `skills/cv-check/references/` are the part that gets better with real-world input. PRs welcome, especially for other languages.

## License

MIT
