# Git Essentials for AI Projects

Git records changes to code and text so that a team can review, compare, and recover work. Model reproducibility also requires versioned data and environments; a code commit alone is not enough.

## Basic concepts

A repository contains history. A commit identifies a recorded change. A branch is a movable reference to a line of development. The working tree contains current files, while the staging area selects what will enter the next commit. A remote is a configured location for exchanging commits.

Use `git status` to understand your state before changing branches or staging files. Review differences before committing. Prefer focused changes that can be explained independently.

## A simple editing workflow

Run these commands from an existing local checkout. The branch name is an example.

```bash
git status
git switch -c docs/refresh-ai-hub
git diff
git add README.md
git diff --staged
git commit -m "Refresh AI hub navigation"
```

`git switch -c` creates and switches to a branch. The original `checkout` command is still valid; `switch` makes branch intent clearer.[^50] Stage the actual reviewed files. The example stages only README.md, so it will not accidentally include unrelated assets.

To publish that branch to an already-configured remote, use `git push -u origin docs/refresh-ai-hub`, then open a pull request through your hosting service. Review the destination before pushing.

## What belongs in history

| Store in Git | Store elsewhere, with a revision reference |
|---|---|
| Source code, prompts, configuration templates | Large datasets and model checkpoints |
| Markdown guides and small evaluation fixtures | Private production samples |
| Dependency lockfiles and environment notes | Secrets and access tokens |
| Small manifests and experiment summaries | Large generated logs and transient caches |

For model and dataset artifacts, use a suitable artifact or data-versioning store and reference the immutable identifier. For notebooks, review output and remove sensitive content before committing.

## Review practices

Explain the problem, the behavior after the change, how it was checked, and any limitation. For a model change, include evaluation comparisons and representative failures. For a documentation change, verify examples, links, and citations.

Keep automated workflows narrowly permissioned. GitHub recommends pinning third-party actions to full commit SHAs for immutability. Never expose secrets to untrusted contribution code.[^51]

## Recovery and collaboration

Use a new corrective commit for shared-history mistakes when appropriate. Discuss history rewriting with collaborators before force-pushing. When resolving conflicts, understand the intended result rather than mechanically accepting one side.

**Exercise:** Make a documentation branch, review the staged diff, and write a pull-request description with verification evidence. See the [update guide](UPDATE_GUIDE.md) for applying this edition.

[Back to AI Essentials Hub](README.md)

## Sources

[^50]: Git project. [git-switch Documentation](https://git-scm.com/docs/git-switch). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^51]: GitHub. [Secure use reference](https://docs.github.com/en/actions/reference/security/secure-use). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
