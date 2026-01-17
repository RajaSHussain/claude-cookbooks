# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Quick Start

```bash
uv sync --all-extras          # Install dependencies
uv run pre-commit install     # Install pre-commit hooks
cp .env.example .env          # Set up API key (edit with your key)
```

## Development Commands

```bash
make format        # Format code with ruff
make lint          # Run linting
make check         # Run format-check + lint
make fix           # Auto-fix issues + format
make test          # Run pytest
```

### Notebook Testing

```bash
make test-notebooks                                    # Structure tests (fast, no API)
make test-notebooks NOTEBOOK=path/to/notebook.ipynb   # Test single notebook
make test-notebooks-exec                               # Execute notebooks (slow, needs API key)
uv run tox -e structure                                # Run in isolated tox environment
```

## Code Style

- **Line length:** 100 characters
- **Quotes:** Double quotes
- **Formatter:** Ruff
- Notebooks have relaxed rules for mid-file imports (E402), redefinitions (F811), and variable naming (N803, N806)

## Key Rules

1. **API Keys:** Use `dotenv.load_dotenv()` and `.env` files. Never use `os.environ["KEY"] = "value"`.

2. **Dependencies:** Use `uv add <package>` or `uv add --dev <package>`. Never edit pyproject.toml directly.

3. **Models:** Define as constant at top of notebook: `MODEL = "claude-sonnet-4-5"`
   - Sonnet: `claude-sonnet-4-5-20250929`
   - Haiku: `claude-haiku-4-5-20251001`
   - Opus: `claude-opus-4-5-20251101`

4. **Notebooks:** Keep outputs (intentional for demonstration), one concept per notebook.

5. **Quality:** Run `make check` before committing.

## Notebook Structure Requirements

Notebooks must follow problem-first pedagogy (see `.claude/skills/cookbook-audit/style_guide.md`):

1. **Introduction:** Hook with problem (not machinery), explain why it matters, list 2-4 learning objectives as bullets
2. **Prerequisites & Setup:** Use `%%capture` for pip installs, `dotenv.load_dotenv()` for keys, define `MODEL` constant
3. **Core Content:** Explanatory text before AND after code blocks
4. **Conclusion:** Map back to learning objectives, suggest applications

## Slash Commands

- `/notebook-review <path>` - Comprehensive notebook quality review using cookbook-audit skill
- `/model-check` - Validate Claude model references are current
- `/link-review` - Check links in changed files

## Project Structure

```
capabilities/      # Core capabilities (RAG, classification, summarization, text-to-sql)
skills/            # Advanced skill-based notebooks
tool_use/          # Tool use and integration patterns
multimodal/        # Vision and image processing
third_party/       # External integrations (Pinecone, Voyage, ElevenLabs)
extended_thinking/ # Extended reasoning patterns
.claude/           # Claude Code commands, agents, and skills
  commands/        # Slash command definitions
  agents/          # Agent definitions (code-reviewer)
  skills/          # Skill definitions (cookbook-audit)
```

## Adding a New Cookbook

1. Create notebook in appropriate directory following structure requirements above
2. Add entry to `registry.yaml`: title, description, path, authors, date, categories
3. Add author to `authors.yaml` if new contributor (run `make sort-authors`)
4. Run `make check` and `/notebook-review` before submitting PR

## Git Workflow

Branch: `<username>/<feature-description>`

Commits: `feat(scope): description`, `fix(scope): description`, `docs(scope): description`
