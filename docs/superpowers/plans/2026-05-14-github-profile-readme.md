# GitHub Profile README Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Publish a minimal ASCII-only `README.md` to a new public repo `Vex9th/Vex9th` so the file renders on `https://github.com/Vex9th`.

**Architecture:** Single file. No build, no tools, no dependencies. Local repo at `C:\Users\Administrator\github-profile` already exists with the design spec committed; we add the README, then create the remote and push.

**Tech Stack:** git, GitHub CLI (`gh`), Markdown. No code.

**Spec reference:** `docs/superpowers/specs/2026-05-14-github-profile-readme-design.md`

---

## File Structure

| Path | Action | Responsibility |
|------|--------|----------------|
| `README.md` | Create | The Profile README content. Only ASCII name + tagline. |

That's the entire change. Everything else is git/gh plumbing.

> **Note on the ASCII art:** The ASCII shown in the spec and reproduced verbatim in Task 2 below was visually approved during brainstorming. It is figlet **"Standard"** font output for `Vex9th` (the spec mentions "ANSI Shadow" — that label was a misname; the visual that was approved is Standard). Copy it verbatim. Do **not** regenerate or "improve" it — the user has already approved this exact glyph layout.

---

### Task 1: Verify local repo state

**Files:** none (verification only)

- [ ] **Step 1: Confirm repo location and branch**

Run from `C:\Users\Administrator\github-profile`:
```bash
git rev-parse --show-toplevel
git branch --show-current
```
Expected:
```
C:/Users/Administrator/github-profile
main
```

If branch is not `main`, run `git branch -m main`.

- [ ] **Step 2: Confirm spec commit exists**

```bash
git log --oneline
```
Expected to include a line like:
```
d341a39 docs: add profile README design spec
```

- [ ] **Step 3: Confirm git identity matches Vex9th**

```bash
git config user.name
git config user.email
```
Expected:
```
Vex9th
ihixgo@gmail.com
```

If either is wrong, fix with `git config --global user.name Vex9th` / `git config --global user.email ihixgo@gmail.com` (per memory: global identity is already aligned, so this should pass).

- [ ] **Step 4: Confirm working tree is clean**

```bash
git status --short
```
Expected: empty output (no `??` or `M` lines).

---

### Task 2: Create README.md with approved content

**Files:**
- Create: `C:\Users\Administrator\github-profile\README.md`

- [ ] **Step 1: Write README.md**

The exact content (10 lines including blank line and fences):

````markdown
```
 __     __         ___  _   _
 \ \   / /__ __  _/ _ \| |_| |__
  \ \ / / _ \\ \/ / (_) | __| '_ \
   \ V /  __/>  < \__, | |_| | | |
    \_/ \___/_/\_\  /_/ \__|_| |_|
```

`// signal lost in the tropics.`
````

Use the Write tool to create the file with exactly the above content (3 backticks open fence on line 1, ASCII art lines 2-6, 3 backticks close fence on line 7, blank line 8, tagline line 9, trailing newline).

**Watch for:** Editors auto-trimming the trailing spaces in the ASCII lines. The ASCII has meaningful leading whitespace (lines start with 1-4 spaces) but no trailing spaces — that part is safe. Some glyph rows end with `|` or `_` — preserve exactly.

---

### Task 3: Verify file content byte-exact

**Files:** none (verification only)

- [ ] **Step 1: Read the file back and visually diff against the spec**

```bash
cat README.md
```

Compare line-by-line against the block in Task 2 Step 1. Pay special attention to:
- Line 3: `\ \   / /__ __  _/ _ \| |_| |__` — single backslash + space + single backslash + 3 spaces + `/`
- Line 4: `\ \ / / _ \\ \/ / (_) | __| '_ \` — note the `\\` (two adjacent backslashes) after `_`
- Line 7 starts with 3 backticks alone on a line (no language tag)
- Line 9 is exactly: `` `// signal lost in the tropics.` ``

- [ ] **Step 2: Count lines and bytes**

```bash
wc -l README.md
```
Expected: `9 README.md` (9 newline-terminated lines).

If any of the above is off, re-write the file using the Write tool with the exact content from Task 2 Step 1.

---

### Task 4: Commit README.md

**Files:**
- Modify: git history

- [ ] **Step 1: Stage and commit**

```bash
git add README.md
git commit -m "feat: add profile README"
```

- [ ] **Step 2: Verify commit**

```bash
git log --oneline -2
```
Expected:
```
<hash> feat: add profile README
d341a39 docs: add profile README design spec
```

---

### Task 5: Verify gh CLI is authenticated as Vex9th

**Files:** none

- [ ] **Step 1: Check `gh` availability**

```bash
gh --version
```
Expected: a version line like `gh version 2.x.x ...`.

If `gh` is not installed, stop here and report to the user. They will need to install it from https://cli.github.com/ (or use the fallback in Task 6 Step 2).

- [ ] **Step 2: Check auth status**

```bash
gh auth status
```
Expected: a line containing `Logged in to github.com account Vex9th`.

If logged in as a different account or not logged in, stop and ask the user to run `gh auth login` themselves (it is interactive). Do **not** invoke `gh auth login` from a tool call — it requires terminal input the harness cannot provide.

---

### Task 6: Create the remote `Vex9th/Vex9th` repository

**Files:** none (remote-only action)

> ⚠️ **User confirmation gate:** This step creates a public repository visible to the world. Confirm with the user before proceeding past Step 1.

- [ ] **Step 1: Verify the repo does not already exist**

```bash
gh api repos/Vex9th/Vex9th --silent && echo "EXISTS" || echo "NOT FOUND"
```
Expected: `NOT FOUND` (HTTP 404 from the API).

If the response is `EXISTS`, stop and ask the user whether to push to the existing repo (skip to Task 7) or abort.

- [ ] **Step 2: Create the public repo (no auto-init, no description)**

```bash
gh repo create Vex9th/Vex9th --public --description "" --confirm
```
Expected: a single line like `https://github.com/Vex9th/Vex9th`.

**Fallback if `gh` is unavailable or not authenticated:** Ask the user to manually create the repo at https://github.com/new with:
- Owner: `Vex9th`
- Repository name: `Vex9th`
- Visibility: **Public**
- Do **not** check "Add a README", "Add .gitignore", or "Choose a license"
- Click "Create repository"

Then continue to Task 7.

- [ ] **Step 3: Confirm creation**

```bash
gh api repos/Vex9th/Vex9th --jq .full_name
```
Expected: `Vex9th/Vex9th`.

---

### Task 7: Push to remote

**Files:** none

> ⚠️ **User confirmation gate:** This step publishes the README publicly. Confirm with the user before running Step 2.

- [ ] **Step 1: Add the remote**

```bash
git remote add origin https://github.com/Vex9th/Vex9th.git
git remote -v
```
Expected:
```
origin  https://github.com/Vex9th/Vex9th.git (fetch)
origin  https://github.com/Vex9th/Vex9th.git (push)
```

If the remote already exists, run `git remote set-url origin https://github.com/Vex9th/Vex9th.git` instead.

- [ ] **Step 2: Push main**

```bash
git push -u origin main
```
Expected: lines ending with `* [new branch]      main -> main` and `branch 'main' set up to track 'origin/main'`.

If push fails with an auth error, stop and ask the user to run `gh auth login` or configure a credential helper. Do **not** retry with `--force` or other flags.

---

### Task 8: Verify rendered output on github.com/Vex9th

**Files:** none (remote verification)

- [ ] **Step 1: Fetch the rendered profile page**

Use the WebFetch tool:
- URL: `https://github.com/Vex9th`
- Prompt: `Does this page contain ASCII art that spells "Vex9th" (using \ and / and _ characters across 5 rows) followed by an inline-code line saying "// signal lost in the tropics."? Answer yes/no and quote the exact text shown.`

Expected: a "yes" answer with the ASCII art and tagline quoted back.

- [ ] **Step 2: Fetch the raw README**

```bash
gh api repos/Vex9th/Vex9th/contents/README.md --jq .content | base64 -d
```
Expected: byte-exact match to Task 2 Step 1 content.

- [ ] **Step 3: Verify single-screen rendering manually**

Ask the user to open `https://github.com/Vex9th` in a browser and confirm:
- ASCII glyphs are aligned (no row wraps to the next line)
- The README fits within one viewport without scrolling
- Dark mode shows the ASCII in the standard code-block color

If glyphs wrap or alignment is off, the most likely cause is the ASCII being rendered outside a code fence. Re-check Task 3 Step 1.

- [ ] **Step 4: Final commit (if any local changes were made during verification)**

```bash
git status --short
```
If output is empty: done.
If anything is shown, stop and ask the user before committing.

---

## Self-Review

**1. Spec coverage:**
- Goal "create README for github.com/Vex9th" → Task 2-7 ✓
- README content (ASCII + tagline) → Task 2 ✓
- ASCII fenced + tagline as inline code → Task 2 Step 1, Task 3 Step 1 ✓
- Dark-mode rendering check → Task 8 Step 3 ✓
- ≤10 lines source → Task 3 Step 2 (wc -l = 9) ✓
- Public repo `Vex9th/Vex9th` → Task 6 ✓
- Excluded elements (stats / badges / etc.) → enforced by Task 2 being the literal content; no task adds anything beyond what is shown ✓
- "Implementation phase should regenerate with figlet" — explicitly overridden in the File Structure note (verbatim copy was approved; do not regenerate). This is a deliberate departure from the spec's wording, documented in this plan.

**2. Placeholder scan:** No TBD / TODO / "fill in later". Every step has exact commands or exact content.

**3. Type consistency:** N/A (no code). Path consistency: `C:\Users\Administrator\github-profile` and `Vex9th/Vex9th` used uniformly.
