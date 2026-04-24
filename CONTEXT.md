# Current Project

## What we are building
A Claude-optimized workspace with two tools: a Project Maker that scaffolds new project folders (CLAUDE.md, CONTEXT.md, REFERENCES.md) through a one-question-at-a-time interview, and a Prompt Maker that builds well-structured, copy-pasteable prompts using the Prompt Structure Framework (Identity, Task, Context, Constraints, Output Format).

## What good looks like
- Project Maker: a complete folder at C:\dev\<name> with three files that give Claude persistent context so you never re-explain the project from scratch
- Prompt Maker: a clean, natural-language prompt block the user can paste anywhere — no markdown headers, no filler, just the right sections for the task type

## What to avoid
- Asking multiple questions at once in either protocol
- Generating files or prompts before all relevant questions are answered
- Using markdown headers inside the output prompt (it should read like natural text)
- Re-running full interviews when only a small tweak is needed
