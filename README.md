# ndcmsT3
Repository with the documentation for the ND CMS T3 Infrastructure, which is automatically built and deployed to GitHub Pages at https://crcresearch.github.io/ndcmsT3/.


## How to use this documentation repository

### 1. Set Up Local Development Environment

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


### 2. Configure MkDocs

Modify the `mkdocs.yml` configuration file according to any new pages you might have.

### 3. Current documentation structure

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

### 4. GitHub actions

GitHub actions have been defined for automatic build and deployment into GitHub Pages

### 5. Test your updates locally

```bash
# Serve documentation locally
mkdocs serve

# Build documentation
mkdocs build
```

Visit `http://localhost:8000` to preview your documentation.

### 6. Deploy your documentation to GitHub Pages

Your documentation will be available at `https://YOUR-USERNAME.github.io/ndcmsT3/`


## Converting reStructuredText to Markdown

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

Note: After conversion, you will likely need to manually adjust the following elements:

1. Sphinx-specific directives:
   - `.. toctree::` directives (table of contents)
   - `.. note::`, `.. warning::`, etc. (admonitions)
   - `.. code-block::` with complex options
   - Custom Sphinx directives

2. Cross-references and links:
   - `:ref:` references
   - `:doc:` references
   - Internal document links
   - Custom role references

3. Advanced formatting:
   - Complex tables with merged cells
   - Sidebar content
   - Figure directives with options
   - Math equations (may need reformatting)

4. Code blocks:
   - Language specifications
   - Line highlighting
   - Line number references
   - Code block captions

5. Extensions and custom roles:
   - Domain-specific roles (e.g., `:py:class:`)
   - Custom extension directives
   - Substitutions and replacements

6. Meta information:
   - Document metadata
   - Version information
   - Copyright notices in special formats