# OpenAPI Markdown Generator

[![PyPI version](https://badge.fury.io/py/openapi-markdown.svg)](https://pypi.org/project/openapi-markdown/)
[![Python versions](https://img.shields.io/pypi/pyversions/openapi-markdown.svg)](https://pypi.org/project/openapi-markdown/)
[![License](https://img.shields.io/github/license/vrerv/openapi-markdown)](https://github.com/vrerv/openapi-markdown/blob/main/LICENSE)
[![Build Status](https://github.com/vrerv/openapi-markdown/actions/workflows/ci.yml/badge.svg)](https://github.com/vrerv/openapi-markdown/actions)

A minimal tool that converts OpenAPI 3.x (JSON or YAML) into compact Markdown for documentation.

## Features

- Minimal, compact Markdown output optimized for small files.
- Supports OpenAPI in JSON and YAML formats.
- Jinja2 templates for full customization.
- Optional path filtering to generate docs for selected API subsets.
- CLI and library usage.
- UTF-8 reading/writing.

## Installation

Install from PyPI:

```bash
pip install openapi-markdown
```

For development:

```bash
git clone https://github.com/vrerv/openapi-markdown.git
cd openapi-markdown
pip install -r requirements.txt
pip install -e .
```

## Quickstart (CLI)

Show help:

```bash
openapi2markdown --help
```

Generate Markdown:

```bash
openapi2markdown path/to/openapi.yaml api_doc.md
```

Filter by path prefixes:

```bash
openapi2markdown spec.yaml api_doc.md --filter-paths /api/v1 --filter-paths /auth
```

Use custom templates:

```bash
openapi2markdown spec.yaml api_doc.md --templates-dir ./my-templates
```

## Quickstart (Python)

```python
from openapi_markdown.generator import to_markdown

to_markdown(
    api_file="./data/openapi.yaml",
    output_file="api_doc.md",
    templates_dir="templates",      # optional
    options={'filter_paths': ['/client']}  # optional
)
```

## Templates

Templates live in `src/openapi_markdown/templates`. Key files:

- `api_doc_template.md.j2` — main document template
- `_content.md.j2`, `_object_schema.md.j2`, `_example.md.j2`, `_security_scheme.md.j2` — partials

To customize, copy the `templates` folder and modify Jinja2 templates. The generator will prefer a provided directory when passed via `--templates-dir` or `templates_dir` argument.

## Examples

Basic conversion:

```bash
openapi2markdown https://example.com/openapi.json api_doc.md
```

Only generate docs for `/users` endpoints:

```bash
openapi2markdown openapi.yaml users.md --filter-paths /users
```

## Development

Requirements:

- Python 3.8+
- Dependencies: Jinja2, PyYAML, openapi-core (see `requirements.txt`)

Run tests:

```bash
python -m unittest
```

Coding workflow:

```bash
git checkout -b feature/my-change
# make changes
git add .
git commit -m "Describe change"
git push origin feature/my-change
# open a pull request against upstream/main
```

If you don't have push access to upstream, fork first and push to your fork.

## Contributing

- Fork the repo, create feature branches, add tests, and open a pull request.
- Follow repository code style and include a clear PR description.
- Maintain compatibility with JSON and YAML OpenAPI specs.

## Release

Use the included `pypi.sh` helper or follow standard PyPI release steps with `twine`.

## License

MIT License - see `LICENSE` for details.

## Contact & Support

- Issues and feature requests: https://github.com/vrerv/openapi-markdown/issues
- For quick questions, open an issue and tag a maintainer.

## CLI Reference

openapi2markdown accepts the following flags:

- `-h, --help`        Show help message and exit
- `-t, --templates-dir DIR`  
  Use templates from DIR instead of built-in templates
- `-f, --filter-paths PREFIX`  
  Only include paths that start with PREFIX. Can be repeated.
- `-o, --output FILE`  
  Output Markdown file (default: api_doc.md)

Example:
```bash
openapi2markdown spec.yaml docs.md --templates-dir ./my-templates --filter-paths /api/v1