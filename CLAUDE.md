# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands

Requires [mise](https://mise.jdx.dev/) and [HackGen font](https://github.com/yuru7/HackGen) (`brew install --cask font-hackgen font-hackgen-nerd`). Typst version is pinned to 0.14.2 via `mise.toml`.

```bash
mise run cv      # Build cv/cv.pdf
mise run resume  # Build resume/resume.pdf
mise run build   # Build both PDFs
```

Each task compiles `main.typ` in its respective directory via `typst compile`.

## Architecture

Two independent document types, each self-contained in its own directory:

### `cv/` — 職務経歴書 (Work History / CV)
- `main.typ` — entry point; imports YAML content and template modules, defines page layout (A4)
- `content/` — YAML data files:
  - `profile.yaml` — personal info, skills, experience breakdown
  - `overview.yaml` — list of companies with period, description, and tech stack
  - `jobs.yaml` — detailed job entries with period, work detail, and used tools/languages
- `template/` — `.typ` display functions: `profile.typ`, `overview.typ`, `jobs.typ`

### `resume/` — 履歴書 (JIS standard resume)
- `main.typ` — entry point; imports YAML content and template modules, defines page layout (JIS-B5)
- `content/` — YAML data files:
  - `profile.yaml` — name (kanji + kana), birthdate, age, photo
  - `address.yaml` — address details
  - `education_history.yaml` — education entries
  - `work_history.yaml` — work history entries
  - `certifications.yaml` — licenses and certifications
  - `motivation_requests.yaml` — 志望動機 and 本人希望
- `template/` — `.typ` display functions: `layout.typ`, `personal_info.typ`, `address.typ`, `history.typ`, `motivation.typ`, `requests.typ`

### Data Flow
Content is pure YAML; templates are pure Typst layout functions. `main.typ` wires them together. To update document content, edit only the YAML files in `content/`. To change layout or styling, edit the `.typ` files in `template/`.
