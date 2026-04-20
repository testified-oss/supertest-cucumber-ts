# HEARTBEAT.md — Testified-OSS improvement (random repo + worktrees + issues)

**Workspace root:** `/Users/luucrew/.openclaw/workspaces/testified-oss-coder`

## A — Install playbooks

- `TOOLS.md` — startup order
- `tools/conventions.md`
- `tools/target-repos.md`
- `tools/git-worktrees.md`
- `tools/github-issues.md`
- `tools/github-prs.md`
- `tools/scan-paths.md`

## B — Auth

```bash
gh auth status
```
On failure: log to `memory/YYYY-MM-DD.md` and stop.

## C — Random repo (exactly one)

Pick **`OWNER/REPO`** uniformly at random from `tools/target-repos.md` (use `shuf`). Set:

```bash
REPO="OWNER/REPO"
```
Every `gh` call must use `--repo "$REPO"`.

## D — Mirror + default-branch worktree (always)

- Ensure `git/mirrors/OWNER__REPO.git` exists (`clone --mirror` + `fetch --prune`).
- Resolve default branch: `gh repo view "$REPO" --json defaultBranchRef -q .defaultBranchRef.name`
- `git worktree add git/wt/OWNER__REPO-<BR>-scan <BR>` (on default branch)
- All file reads use this local tree.

## E — Open issues? (existence probe, `-L 1`)

```bash
has_open_issues=$(gh issue list --repo "$REPO" --state open -L 1 --json number --jq 'length')
```
Hard gate: probe must succeed; log raw command + output to `memory/YYYY-MM-DD.md`.

- If **`has_open_issues` is `0`** → continue to **E.2**
- If **non‑zero** → continue to **E.2** (routing decided after both probes)

## E.2 — Open PRs? (existence probe, `-L 1`)

```bash
has_open_prs=$(gh pr list --repo "$REPO" --state open -L 1 --json number --jq 'length')
```
Hard gate: probe must succeed; log to `memory/YYYY-MM-DD.md`.

## F — Create new issue (`run_path=create`)

**Precondition:** both probes returned `0`. If not, abort to J or G.

**Preflight (repeat probes):** re-run E and E.2; if issues/PRs appear, abort to J or G.

Steps:
1. Scan files per `scan-paths.md` in the worktree.
2. Dedupe per `github-issues.md`.
3. `gh issue create --repo "$REPO" --title "..." --template "..." --body-file scratch/issue-body-OWNER-REPO.md`
4. Suggested commit lines must match `conventions.md`.

## G — Work on existing issue (`run_path=work`)

**Precondition:** `has_open_issues != 0` and `has_open_prs = 0`.

1. `gh issue list --repo "$REPO" --state open --json number,title,labels -L 30` — pick one.
2. `gh issue view N`.
3. Worktree + topic branch: `git worktree add -b feat/issue-N-triage` from mirror.
4. Implement fix; commit with conventional commits.
5. `gh pr create --draft` (if ≥1 commit) per `github-prs.md`.
6. `gh issue comment N` with PR link and summary.

## H — Teardown

```bash
git worktree remove --force git/wt/...
```
Keep mirrors. Append `memory/YYYY-MM-DD.md`.

## I — Final Discord message

Full summary: repo, run_path, probe values, worktree paths, issue/PR links, checks summary. Send to channel **1069257061533233182**.
