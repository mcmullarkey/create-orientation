---
name: orient
description: Generates a repo-specific orientation.md resource for the agent-learning-opportunities skill. Only invoke via slash command (/orient:orient). Do not trigger automatically.
disable-model-invocation: true
allowed-tools: Read, Glob, Grep, Bash, Write
---

# Create Orientation

## Purpose

Generate a repo-specific `orientation.md` file inside the `agent-learning-opportunities` skill's `resources/` directory. This file is used by that skill when invoked as `/learning-opportunities orientation` to run a structured learning exercise for someone new to the codebase.

---

## Step 1: Find where to write orientation.md

Always write to the **project level**, regardless of where the `agent-learning-opportunities` skill is installed:

```
.claude/skills/agent-learning-opportunities/resources/orientation.md
```

(relative to the current working directory)

Create the directory `.claude/skills/agent-learning-opportunities/resources/` if it does not exist.

This keeps orientation files co-located with the repo they describe — they can be committed to version control, shared with teammates, and never collide across projects.

---

## Step 2: Detect the repo's primary language(s)

Check for these manifest/config files at the project root and note all that exist. A repo may use multiple languages.

| Language   | Signal files                                              |
|:-----------|:----------------------------------------------------------|
| Python     | `pyproject.toml`, `setup.py`, `setup.cfg`, `Pipfile`, `requirements.txt` |
| JavaScript | `package.json` (no `tsconfig.json`)                      |
| TypeScript | `package.json` + `tsconfig.json`                         |
| R          | `DESCRIPTION`, `NAMESPACE`, any `*.Rproj`                |
| Ruby       | `Gemfile`, any `*.gemspec`                                |
| Go         | `go.mod`                                                  |
| Rust       | `Cargo.toml`                                              |
| C/C++      | `CMakeLists.txt`, `configure.ac`, root-level `Makefile`  |
| Java/Kotlin| `pom.xml`, `build.gradle`, `build.gradle.kts`            |
| C#         | any `*.csproj` or `*.sln`                                |

Record all detected languages. For each detected language, read its primary manifest file in full — it contains declared purpose, dependencies, entry points, and scripts/commands that are essential for orientation.

---

## Step 3: Explore the repo

Use the following sequence, drawn from research on expert program comprehension strategies. Experts read **strategically and selectively**, not exhaustively. The goal is a mental model of structure, not line-by-line understanding.

### 3a. README and top-level docs
Read `README.md`, `README.rst`, or `README` at the project root. Also check for a `docs/` directory — read its index or table of contents if present. This gives the stated purpose and intended audience.

*Source: Spinellis, "Code Reading: The Open Source Perspective" (2003) — start with the build system and README before reading any application code.*

### 3b. Directory tree
Run `find . -maxdepth 3 -not -path '*/.git/*' -not -path '*/node_modules/*' -not -path '*/__pycache__/*' -not -path '*/.venv/*'` to get the top-level structure. Read the directory tree as an architectural table of contents — naming conventions (`src/`, `lib/`, `tests/`, `cmd/`, `pkg/`) reveal intent before any code is read.

*Source: Spinellis (2003) — "directory tree as table of contents."*

### 3c. Entry points
Identify and read the main entry points based on detected language:

- **Python**: `__main__.py`, `cli.py`, `main.py`, or the `[tool.poetry.scripts]` / `[project.scripts]` section of `pyproject.toml`
- **JavaScript/TypeScript**: `main` field in `package.json`, `index.js`, `src/index.ts`
- **Go**: files in `cmd/*/main.go` or root `main.go`
- **Rust**: `src/main.rs` or `src/lib.rs`
- **R**: `R/` directory, the `DESCRIPTION` file's `Imports`
- **Ruby**: files in `bin/`, `lib/<gem-name>.rb`
- **C/C++**: `main.c`, `main.cpp`, or the primary target in `CMakeLists.txt`

*Source: Hermans, "The Programmer's Brain" (2021, Manning) — follow the entry point and call graph one level at a time.*

### 3d. Test files
Read 2–3 test files, prioritizing integration or end-to-end tests over unit tests. Tests are executable specifications — reading test names and assertions is one of the fastest ways to understand what a module is meant to do.

*Source: Storey et al., "How Software Developers Use Tools, Cognitive Strategies, and Representations to Navigate Code" (IEEE TSE, 2006) — use the test suite as a specification.*

### 3e. Core modules
Identify the 5–8 most important source files based on what you have learned. Read their top-level structure (class/function names, imports, docstrings) without necessarily reading every implementation in full.

### 3f. Recent git history (if git is available)
Run `git log --oneline -20` to see recent activity. Run `git log --format="%f" | sort | uniq -c | sort -rn | head -10` to identify the most-edited files. High-churn files are usually the core of the system.

*Source: Spolsky practitioner writing — "find the biggest, most-edited file; read git history to understand why code is the way it is."*

---

## Step 4: Synthesize and write orientation.md

Write the file to the path identified in Step 1. Use this exact structure:

```markdown
# Repo Orientation: [repo name]

> Generated by /orient:orient. Re-run to update.

## One-line purpose
[Single sentence: what this repo does and why it exists. Written for someone with no prior context.]

## Primary language(s)
[List languages detected, with the dominant one first.]

## Pipeline / workflow stages
[Ordered list of the main stages data or requests flow through. One line each. If the repo has no pipeline, describe the main modules and their relationships instead.]

## Key files
[6–10 entries in this format:]
- `path/to/file.py` — [what it does] | [why a new developer should read it]

## Core concepts
[3–5 domain or architectural concepts essential to working in this codebase. For each:]
**[Concept name]**: [Plain-English definition. Where in the code it lives.]

## Common gotchas
[2–3 things that commonly trip up new developers. Be specific — reference actual file paths or function names.]

## Suggested exercise sequence
[EXACTLY 2 exercises. These are orientation exercises — their job is to build a high-level mental model of the repo, not to drill implementation details.

Orientation exercises follow this pattern: direct the learner to read one specific, short artifact first, then ask them to synthesize or explain what they just read. Never ask them to predict something they couldn't know without reading — the goal is comprehension and synthesis, not prior knowledge.

Good orientation exercises:
- "Open README.md and read the Features section. Then close it and explain to a non-developer what this tool produces and why someone would use it."
- "Open `models.py`. Find the dataclass that represents everything the pipeline produces for one audio file. What fields does it have, and what does that tell you about the pipeline's stages?"
- "Open `config/default.yaml` and skim it. What are the two or three settings you'd most likely need to change for a new project, and why?"

Bad orientation exercises (save these for later sessions):
- "Without opening any files, predict the pipeline stages" — learner has no basis for this
- Predicting specific function outputs, column names, or algorithmic behavior
- Tracing through individual method implementations
- Debugging specific logic (e.g. merge suffix behavior, metadata propagation)

For each exercise, specify: the exact file to open, what to read, and what synthesis question to answer after reading.]

## Sources consulted
[List the files and paths you actually read while generating this file.]
```

Keep each section concise. This is a teaching scaffold, not documentation. Prioritize clarity over completeness.

---

## Step 5: Confirm to the user

> **Note for skill maintainers**: Academic and practitioner sources for the exploration methodology in Steps 3a–3f are documented in [resources/bibliography.md](resources/bibliography.md). Load that file only if you need to update or cite sources — it is not needed during normal skill execution.

Tell the user:
- Where the file was written
- How many key files and concepts were identified
- How to use it: `/learning-opportunities orientation`
- That they can re-run `/orient:orient` at any time to regenerate it as the codebase evolves
