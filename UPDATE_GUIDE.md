# Apply This AI Essentials Hub Update

This package contains **28 Markdown files and the unchanged original LICENSE**. It includes replacements for all 17 Markdown files in the supplied archive and 11 new Markdown files. It is a documentation update, not an application installer.

## Apply the files

1. Back up your current repository or create a documentation-update branch.
2. Extract `AI_Essentials_Hub_2026-09-13.zip`.
3. Open the extracted `AI_Essentials_Hub_2026-09-13` folder.
4. Copy its **contents** into your repository root, allowing the 17 existing Markdown files to be replaced and the 11 new files to be added.
5. Keep any unrelated repository files and local additions. Preserve the original license notice.
6. Open `README.md`, follow the links, review the diff, and commit the documentation update when satisfied.

Copy the contents into the repository root; do not place the whole dated folder inside the repository if you intend to replace the old root-level guides.

## Review before committing

The [changelog](CHANGELOG.md) maps every existing file to its revision. The [R&D report](RD_Report.md) explains the findings and proposed experiments. The [source register](SOURCES.md) lists the evidence and dates. Relative Markdown links assume all supplied Markdown files remain together at the repository root.

If your working repository has changes beyond the supplied ZIP, merge those edits deliberately instead of replacing them without review. No remote repository has been modified, and this edition does not assert a verified upstream commit history.

## What was checked

- Presence of all 17 original Markdown filenames and the 11 additions.
- Local Markdown link targets, footnote definitions, UTF-8 text, and fenced-code balance.
- Syntax of Python fenced examples.
- Execution of the NumPy/pandas example and four standard-library examples: local experiment recording, generated-output validation, Recall@k, and cost-per-success arithmetic.
- Byte-for-byte preservation of the original license and archive file integrity.

R examples, third-party ML examples, framework installation commands, GPU execution, provider API calls, and cloud deployment were **not run**. Those examples require the stated dependencies, data, or services. No live performance comparison or training experiment is claimed. Public sources were reviewed, but future external-link availability is not guaranteed.

## Version policy

The documentation review spans 12–13 September 2026. Model IDs in [Generative_AI.md](Generative_AI.md) are a dated discovery snapshot. Installation instructions resolve compatible dependencies when executed; create and validate a lockfile in each actual project.

For a working project, test an upgrade in an isolated environment, compare the same evaluation set, and keep the previous version available. Updating a Markdown guide does not automatically update or validate your deployed model.

[Back to AI Essentials Hub](README.md)
