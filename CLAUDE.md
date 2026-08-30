# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a personal notes repository ("notebook") of technical interview-style Q&A markdown files, not a software project. There is no build, lint, or test tooling — the only artifacts are Markdown documents organized by topic:

- `os/os.md` — Operating Systems theory
- `databases/db.md` — Database concepts, architectures, storage engines, ACID/BASE
- `go/go.md` — Go language internals (e.g. GC)
- `ai-llm/ai.md` — AI/ML fundamentals, neural networks

New topics get their own directory with a single `<topic>/<topic>.md` file, following this existing naming convention.

## Governing Instructions

`AGENTS.md` at the repo root is the authoritative content/style spec for this repository and **must be read and followed** before writing or editing any answer in any topic file. Key rules from it:

- **Persona**: Answer as a senior/staff engineer giving an authoritative, technical interview response — go past definitions into internals, mechanics, and trade-offs.
- **Prose, not bullets**: Answers are written as cohesive paragraphs (the three-part structure below), not bullet lists. Reserve bullets/lists for strictly enumerable items (flags, error states, config keys) where prose would be unnatural.
- **Minimal code**: Include a code/query snippet only when prose genuinely cannot express the nuance — keep it as short as possible.
- **No question numbering**: Question headings (`##`) are never numbered (no "1.", "Q1:", etc.).
- **Three-part answer structure** for every question: (1) a direct thesis/definition with no preamble, (2) deep technical mechanics/internals, (3) practical trade-offs and senior-level production considerations.
- **Strict scope**: Only answer questions that already exist in the target file with empty answers, or that the user explicitly points to. Never invent new questions or add content the user didn't ask for — this includes not padding topic files with extra Q&A on your own initiative.

When asked to add or edit an answer, locate the existing `##` question heading in the relevant topic file and write the answer beneath it in this style; when asked to add a new question, follow the same heading/section conventions already used in that file (`##` for the question, `---` separating entries).
