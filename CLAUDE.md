# project-maker

## Purpose
This folder has two tools:
1. **Project Maker** — scaffolds a new Claude-optimized project folder with three context files
2. **Prompt Maker** — interviews you to build a well-structured, copy-pasteable prompt

Trigger words:
- "make a new project" / "new project" / "create a project" → **New Project Interview Protocol**
- "make a prompt" / "build a prompt" / "prompt maker" / "write a prompt" → **Prompt Maker Protocol**

---

When the user says anything like "make a new project", "new project", "create a project", or "start a project" — follow the New Project Interview Protocol below exactly.

---

## New Project Interview Protocol

Ask each question ONE AT A TIME. Wait for the answer before moving to the next.
Do not list all questions upfront. Do not number them or hint at what's coming.
Keep your questions short and conversational.

### Question 1 — Name
> "What's the name of this project?"

### Question 2 — Type
> "What type of project is this?"

After they answer, silently classify it using the Prompt Strategy Table:
- **Simple task** → Task only (minimal CLAUDE.md)
- **Creative** → Identity + Constraints + Output Format
- **Complex** → All five sections
- **Ongoing project** → Identity + Context in files; Task + Constraints go in each prompt

If unclear, ask: "Is this a one-off task, an ongoing project, or something more complex?"

### Question 3 — Identity
> "Who should I be for this project? Describe the role, expertise, tone, and perspective you want."

Examples to suggest if they're stuck: "experienced React Native developer", "direct startup advisor", "professional copywriter who avoids jargon".

### Question 4 — Context
> "What are you building? Give me 2–3 sentences — what it does, who it's for, and where you are in the process."

### Question 5 — Constraints
> "What should I avoid? Any rules, limitations, or things that have gone wrong before."

Examples: "no TypeScript", "plain language only", "always read files before editing", "don't suggest libraries without asking first".

### Question 6 — Output format
> "What should my output look like by default?"

Examples: "code only, no explanation", "short bullet points", "full working files with no placeholders", "step-by-step with reasoning".

### Question 7 — Folder structure
> "Does this project need specific subfolders? For example: /drafts, /final, /references — or anything specific to your workflow. Say 'none' to skip."

### Question 8 — References
> "Any links, docs, examples, or background I should know about? Say 'none' to skip."

---

## File Generation

After all answers are collected, generate three files in `C:\dev\<project-name>\`.
Create the folder if it does not exist. Create any subfolders the user specified in Question 7.

### CLAUDE.md

Template (match this structure exactly):
```markdown
# <project-name>

## Identity
You are helping <name from Question 3, rewritten as "You are helping Richard with <what you do>">

## Folder Structure
<From Question 7 — one bullet per folder with a short description of its purpose.>
<If user said 'none', omit this section entirely.>

## Rules
- Read this file first on every new task
- Ask before creating files outside of the defined folders
- <Constraints from Question 5, each as a bullet>
- When unsure, ask
```

For React Native / Expo projects, also add after the Rules section:
```markdown
## Build Workflow

**Edit on:** Windows (`C:\dev\<name>`)
**Build on:** Ubuntu at `richard@192.168.100.23`

### Build steps (run manually on Ubuntu via SSH)
```bash
ssh richard@192.168.100.23
cd ~/dev/<name>
git pull origin master
npm install
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

### CONTEXT.md

```markdown
# Current Project
<Answer from Question 4 — 2–3 sentences>

## What good looks like
<Answer from Question 6 — described as a positive outcome>

## What to avoid
<Answer from Question 5 — bullet list>
```

---

### REFERENCES.md

```markdown
# References
<Answer from Question 8 — links, examples, notes. If 'none', leave a placeholder comment.>
```

---

## After generating

Confirm with:
> "Done. Project folder created at `C:\dev\<name>`. Open it and run `claude` to start."

If the user wants to adjust anything, edit the files directly — do not re-run the whole interview.

---

## Prompt Maker Protocol

When the user says "make a prompt", "build a prompt", "prompt maker", or "write a prompt" — follow this protocol exactly.

Ask each question ONE AT A TIME. Wait for the answer before continuing.
Do not reveal the full list of questions. Keep it conversational.

### Step 1 — The task
> "What do you want Claude to do? One clear sentence."

This is the core of the prompt. Everything else supports it.

### Step 2 — Prompt type
> "Is this a simple one-off task, a creative task, or something complex?"

Silently map their answer:
- **Simple** → Task only. Skip to output.
- **Creative** → Ask Steps 3, 5, 6.
- **Complex** → Ask all Steps 3–6.

If unsure, ask one follow-up: "Does Claude need background info to do this well, or is the task self-explanatory?"

### Step 3 — Identity *(Creative and Complex only)*
> "Who should Claude be? Describe the role, expertise, and tone."

Examples if stuck: "senior copywriter", "experienced Python developer", "brutally honest startup advisor".

### Step 4 — Context *(Complex only)*
> "What background does Claude need? Paste or describe the relevant info."

This is where source material, data, meeting notes, or product details go. If the user has a lot to paste, tell them: "Paste it after your answer and I'll incorporate it."

### Step 5 — Constraints *(Creative and Complex only)*
> "What should Claude avoid or stay within?"

Examples: "under 100 words", "no jargon", "don't suggest libraries", "avoid passive voice".

### Step 6 — Output format *(Creative and Complex only)*
> "What should the output look like? Describe the shape."

Examples: "3-paragraph essay", "bulleted list", "JSON object", "table with columns X, Y, Z", "working code only, no explanation".

---

## Prompt Output

After all answers are collected, assemble and output the prompt as a clean block the user can copy and paste directly.

**Do not use markdown headers inside the prompt.** Write it as natural flowing text — the kind you'd actually paste into a chat.

### Simple prompt output:
```
[Task sentence from Step 1.]
```

### Creative prompt output:
```
You are [identity].

[Task sentence].

[Constraints as a sentence or short list.]

[Output format description.]
```

### Complex prompt output:
```
You are [identity].

[Task sentence].

Context: [background from Step 4 — summarized or reproduced cleanly].

Constraints:
- [constraint 1]
- [constraint 2]

Output format: [format description].
```

After outputting the prompt, ask:
> "Want to adjust anything — the tone, length, or any section?"

If they say yes, ask what to change and regenerate. Do not re-run the full interview.
