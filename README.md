# ndcms-docs
Documentation for the ND CMS T3 Infrastructure


# How to use this documentation repository

## 1. Set Up Local Development Environment

To build the documentation locally, you'll need the following Python packages:

```bash:mkdocs.yml
# Install MkDocs and the Material theme
pip install mkdocs \
            mkdocs-git-revision-date-localized-plugin \
            pymdown-extensions
```

These packages provide:
- `mkdocs`: The core documentation generator
- `mkdocs-git-revision-date-localized-plugin`: For showing last update dates
- `pymdown-extensions`: For enhanced Markdown features (code highlighting, admonitions, etc.)


## 2. Configure MkDocs

Modify the `mkdocs.yml` configuration file according to any new pages you might have.

## 3. Current documentation structure

The initial and current document structure is:

```bash:directory-structure
.
├── .github/
│   └── workflows/
│       └── ci.yml
├── docs/
│   ├── index.md
│   ├── research/
│   │   ├── projects.md
│   │   └── publications.md
│   └── resources/
│       ├── computing.md
│       └── data.md
└── mkdocs.yml
```

## 4. GitHub actions

GitHub actions have been defined for automatic build and deployment into GitHub Pages

## 5. Test your updates locally

```bash
# Serve documentation locally
mkdocs serve

# Build documentation
mkdocs build
```

Visit `http://localhost:8000` to preview your documentation.

## 6. Deploy your documentation to GitHub Pages

Your documentation will be available at `https://YOUR-USERNAME.github.io/ndcms-docs/`


# Converting reStructuredText to Markdown

First, install pandoc on your system:

### MacOS
```bash
brew install pandoc
```

### Ubuntu/Debian
```bash
sudo apt-get update
sudo apt-get install pandoc
```

### RHEL/CentOS/Fedora
```bash
sudo dnf install pandoc
```

Then, run the following command to convert an `.rst` file to `.md`.

```bash
# Basic conversion
pandoc -f rst -t gfm input.rst -o output.md

# With additional options for better GitHub-Flavored Markdown compatibility
pandoc --wrap=none -f rst -t gfm input.rst -o output.md

# For handling Sphinx documentation
pandoc --wrap=none --from=rst --to=gfm-raw_html input.rst -o output.md
```

Note: After conversion, you may need to manually adjust:
- Table of contents (toctree directives)
- Sphinx-specific roles and directives
- Cross-references
- Code block language specifications