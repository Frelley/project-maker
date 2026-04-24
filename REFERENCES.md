Last updated: 2026-04-24

# References

## Prompt Structure Framework — the five sections

| Section | Purpose |
|---|---|
| **Identity** | Who Claude is: role, expertise, tone (one line) |
| **Task** | The primary action — precise verb + what to do |
| **Context** | Background info, data, source material Claude needs |
| **Constraints** | What to avoid: limits, rules, negative boundaries |
| **Output Format** | Shape of the result: length, structure, style |

## Prompt Strategy Table

| Type | Sections |
|---|---|
| Simple task | Task only |
| Creative | Identity + Task + Constraints + Format |
| Complex | All five |
| Ongoing project | Identity + Context in files; Task + Constraints in each prompt |

## The three project files

| File | Job | Rule |
|---|---|---|
| `CLAUDE.md` | Routing — where things are and where to go | Keep under 50 lines |
| `CONTEXT.md` | Project brief — what the work is, what good looks like | 80% about the work, 20% behavior |
| `REFERENCES.md` | Background — links, examples, running notes | Passive — Claude reads but doesn't act on directly |

## 7 Common Mistakes (from lesson 3.3)

1. **CLAUDE.md too long** — keep it under 50 lines, routing only
2. **No routing table** — always include task → folder → read table
3. **Too many workspaces** — start with 2–3, add more only when needed
4. **Context files describe Claude's personality, not the work** — flip the ratio: 80% work, 20% behavior
5. **Never updating context files** — treat them like working notes, add "Last updated" header
6. **Everything in one flat folder** — more than 8–10 files at one level needs subfolders
7. **Building the whole system before using it** — build the minimum, use it, then grow

## Workspace rule of thumb
Ask: "Do I shift mental modes between these tasks?" If yes → separate workspace. If no → subfolder inside one workspace.

## Trigger phrases
- **Project Maker:** "make a new project", "new project", "create a project"
- **Prompt Maker:** "make a prompt", "build a prompt", "write a prompt"
