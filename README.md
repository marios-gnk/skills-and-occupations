# Skills and occupations

Assignment 3 for Knowledge Graphs with Large Language Models, MSc in AI and
Data Science (2026–2027).

The project will populate a skills knowledge graph from ESCO and job postings,
then evaluate an LLM judge for skill extraction.

## Repository layout

```text
assignment3_Giannakopoulos/   Local assignment instructions and lecture notes (ignored)
data/
  raw/
    job_posts.csv           Original job-post dataset
ontologies/
  skills-and-occupations.rdf Original RDF/OWL schema
src/
  skills_and_occupations/    Python package for pipeline code
pyproject.toml              Project metadata and dependencies
```

Keep original input files in `data/raw/` unchanged. Put generated datasets in
`data/processed/` and populated graphs or evaluation outputs in `outputs/` as
the pipelines are implemented. These generated directories are ignored by Git.

## Setup

Use Python 3.13 or newer. With [uv](https://docs.astral.sh/uv/), run:

```bash
uv sync
cp .env.example .env
```

No pipeline or command-line entry point is implemented yet.

### Data paths in notebooks

The `.env.example` template sets `DATA_DIR=data`. Your local `.env` is ignored by
Git; edit it if your data lives elsewhere. Relative paths are resolved against
the directory containing `.env`, and absolute paths are also supported.

Run notebooks with the project's Python environment and a working directory
inside this repository (either the root or a subdirectory), then use:

```python
import os
from pathlib import Path
from dotenv import find_dotenv, load_dotenv

env_file = find_dotenv(usecwd=True)
if not env_file:
    raise FileNotFoundError("Copy .env.example to .env before running.")

project_root = Path(env_file).resolve().parent
load_dotenv(env_file)
data_dir = project_root / Path(os.environ["DATA_DIR"]).expanduser()

job_posts_path = data_dir / "raw" / "job_posts.csv"
```

Environment variables already set in your session take precedence over `.env`.
When sharing this repository, include `.env.example` so each person can create
their own `.env` without machine-specific paths.

## Assignment tasks

1. Populate the local `Skill` class using ESCO v1.2.1, preserving skill IRIs
   and importing only the required labels, descriptions, and definitions.
2. Extract skills from job posts and incorporate them as synonyms of existing
   skills or as new skill entities.
3. Evaluate an LLM skill-extraction judge against at least 50 human-labelled terms.

See the local assignment PDF for the full requirements. Keep API keys out of
source files; use local environment variables if a pipeline requires credentials.
