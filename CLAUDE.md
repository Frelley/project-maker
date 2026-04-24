# project-maker

## Purpose
This folder has two tools:
1. **Project Maker** — scaffolds a new Claude-optimized project folder with context files
2. **Prompt Maker** — interviews you to build a well-structured, copy-pasteable prompt

Trigger words:
- "make a new project" / "new project" / "create a project" → **New Project Interview Protocol**
- "make a prompt" / "build a prompt" / "prompt maker" / "write a prompt" → **Prompt Maker Protocol**

---

## New Project Interview Protocol

Ask each question ONE AT A TIME. Wait for the answer before moving to the next.
Do not list all questions upfront. Keep it conversational.

### Question 1 — Name
> "What's the name of this project?"

### Question 2 — What are you building?
> "What is this project? 2–3 sentences — what it does, who it's for, where you are in the process."

Focus on the work, not how Claude should behave. This goes into CONTEXT.md.

### Question 3 — Modes of work
> "What are the different modes of work in this project? For example: writing, building, publishing — or research, drafting, review."

Use their answer to define workspaces. Rules:
- **1 mode** → no workspaces, just root-level files
- **2–3 modes** → one folder per mode, each gets its own CONTEXT.md
- **4+ modes** → push back: "Which two or three are the main ones? Start with those — you can always add more."

Each folder becomes a row in the routing table.

### Question 4 — Identity
> "In one line: who should I be for this project? Role and expertise only."

Keep this short. One sentence. It goes at the top of CLAUDE.md — not the context file.
Example: "You are helping Richard build a React Native app for Android."

### Question 5 — Rules
> "What should I never do or always do in this project? Any hard constraints."

Examples: "always read a file before editing", "no TypeScript", "ask before creating new files".
These go in the Rules section of CLAUDE.md — keep the list short (3–6 bullets max).

### Question 6 — What good looks like
> "What does a successful output look like? Be specific about format and quality."

This goes into each CONTEXT.md under "What good looks like".

### Question 7 — What to avoid
> "What are the common mistakes or things you don't want in the output?"

This goes into each CONTEXT.md under "What to avoid".

### Question 8 — References
> "Any links, docs, examples, or background I should know about? Say 'none' to skip."

---

## File Generation Rules

Generate files in `C:\dev\<project-name>\`. Create the folder if it does not exist.
Create workspace subfolders based on Question 3.

### The generated CLAUDE.md must stay under 50 lines.
It is a routing file — not a project brief. If content doesn't help Claude navigate, it belongs in a CONTEXT.md.

```markdown
# <project-name>

Last updated: <today's date>

## Identity
<One line from Question 4>

## Folder Structure
<One bullet per workspace folder, with a one-line description of what work happens there.>
<If no workspaces, list any root-level files instead.>

## Routing Table
| Task | Go to | Read |
|---|---|---|
<One row per mode of work from Question 3>
<Example row: | Write content | /drafts | CONTEXT.md |>

## Rules
- Read this file first on every new task
- Ask before creating files outside the defined folders
<Constraints from Question 5 — bullet list, 3–6 items max>
- When unsure, ask
```

For React Native / Expo projects, add after Rules:
```markdown
## Build Workflow

**Edit on:** Windows (`C:\dev\<name>`)
**Build on:** Ubuntu at `richard@192.168.100.23`

### Build (run on Ubuntu via SSH)
```bash
ssh richard@192.168.100.23
cd ~/dev/<name>
git pull origin master && npm install
bash -i -c 'cd ~/dev/<name>/android && ./gradlew assembleRelease'
```

### Install (run from Windows)
```bash
scp richard@192.168.100.23:~/dev/<name>/android/app/build/outputs/apk/release/app-release.apk "$USERPROFILE/Downloads/<name>-release.apk"
adb install -r "$USERPROFILE/Downloads/<name>-release.apk"
```
**Always bump the version string before every build.**
```

---

### CONTEXT.md (one per workspace, or one at root if no workspaces)

```markdown
Last updated: <today's date>

# <workspace name or project name>

## What we are building
<Answer from Question 2 — focused on the work: what it is, who it's for, where things stand>

## What good looks like
<Answer from Question 6 — specific about format, quality, and shape of output>

## What to avoid
<Answer from Question 7 — bullet list of failure modes and things to skip>
```

Spend 80% of this file on the work description. Keep behavioral instructions out of here — they belong in the Rules section of CLAUDE.md.

---

### REFERENCES.md (one at root level, shared across workspaces)

```markdown
Last updated: <today's date>

# References

## Links and docs
<Answer from Question 8 — or a placeholder comment if none>

## Examples of good work
<!-- Add examples here as you find them -->

## Notes
<!-- Running notes on decisions and background -->
```

---

## After generating

Say:
> "Done. `C:\dev\<name>` is ready with [list the files created]. Start small — use it for a few days before adding anything. If something's missing, edit the context files directly rather than rebuilding from scratch."

If the project only needed one workspace, say so: "You only had one mode of work so I kept it simple — everything is at the root level. Add subfolders when you actually need them."

---

## Prompt Maker Protocol

When the user says "make a prompt", "build a prompt", "prompt maker", or "write a prompt" — follow this protocol exactly.

Ask each question ONE AT A TIME. Wait for the answer before continuing.
Do not reveal the full list of questions. Keep it conversational.

### Step 1 — The task
> "What do you want Claude to do? One clear sentence."

### Step 2 — Prompt type
> "Is this a simple one-off task, a creative task, or something complex?"

Silently map their answer:
- **Simple** → Task only. Skip to output.
- **Creative** → Ask Steps 3, 5, 6.
- **Complex** → Ask all Steps 3–6.

If unsure: "Does Claude need background info to do this well, or is the task self-explanatory?"

### Step 3 — Identity *(Creative and Complex only)*
> "Who should Claude be? One line — role and expertise only."

### Step 4 — Context *(Complex only)*
> "What background does Claude need? Paste or describe the relevant info."

If they have a lot to paste: "Paste it after your answer and I'll incorporate it."

### Step 5 — Constraints *(Creative and Complex only)*
> "What should Claude avoid or stay within?"

### Step 6 — Output format *(Creative and Complex only)*
> "What should the output look like? Describe the shape."

Examples: "3-paragraph essay", "bulleted list", "working code only, no explanation".

---

## Prompt Output

Output the prompt as clean, natural text the user can copy and paste directly.
**No markdown headers inside the prompt.** Write it as flowing text.

### Simple:
```
[Task sentence.]
```

### Creative:
```
You are [identity].

[Task sentence].

[Constraints as a sentence or short list.]

[Output format.]
```

### Complex:
```
You are [identity].

[Task sentence].

Context: [background — summarized or reproduced cleanly].

Constraints:
- [constraint 1]
- [constraint 2]

Output format: [format description].
```

After outputting, ask:
> "Want to adjust anything — the tone, length, or any section?"

If yes, ask what to change and regenerate. Do not re-run the full interview.
