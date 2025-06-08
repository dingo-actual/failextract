# FailExtract Documentation

This directory contains the documentation source files for FailExtract. This guide explains how to work with, build, and contribute to the FailExtract documentation.

## 🚀 Quick Start

### First-time Setup
```bash
cd docs
make setup        # Install dependencies + build + generate API docs
make serve        # Start local server
```

Open http://localhost:8000 to view the documentation.

### Daily Development
```bash
# Auto-rebuild on file changes with live reload
make watch

# Quick one-time build
make html

# Quality check before committing
make quality
```

## 📁 Documentation Structure

```
docs/
├── requirements.txt     # Sphinx and extension dependencies
├── Makefile            # Build automation and development commands
├── README.md           # This contributor guide
├── build/              # Generated documentation (git-ignored)
│   └── html/           # Built HTML files deployed to GitHub Pages
└── source/             # Source files for documentation
    ├── conf.py         # Sphinx configuration
    ├── index.rst       # Main documentation index
    ├── tutorials/      # Learning-oriented step-by-step guides
    ├── how-to/         # Task-oriented practical instructions
    ├── discussions/    # Understanding-oriented explanations
    ├── reference/      # Information-oriented API documentation
    ├── _static/        # Custom CSS, images, and static assets
    └── _templates/     # Custom Sphinx templates
```

## 🛠️ Building Documentation

### Prerequisites
- Python 3.11+
- FailExtract installed in development mode (`uv pip install -e .`)

### Install Documentation Dependencies
```bash
# Install documentation build dependencies
uv pip install -r docs/requirements.txt

# Or install FailExtract with docs extra
uv pip install -e .[docs]

# Or use the Makefile helper
cd docs && make install-deps
```

### Build Commands

| Command | Description |
|---------|-------------|
| `make setup` | Complete first-time setup (deps + API docs + build) |
| `make html` | Build HTML documentation once |
| `make clean` | Remove all build artifacts |
| `make serve` | Serve built docs at http://localhost:8000 |
| `make watch` | Auto-rebuild with live reload server |
| `make quality` | Run quality checks (linkcheck + doctest) |

### Advanced Commands

| Command | Description |
|---------|-------------|
| `make linkcheck` | Check all external links for validity |
| `make doctest` | Test all code examples in documentation |
| `make coverage` | Generate documentation coverage report |
| `make apidoc` | Regenerate API documentation from source |
| `make production` | Full production build with all formats |
| `make spelling` | Spell check documentation (if enabled) |

## 📝 Writing FailExtract Documentation

### Documentation Framework (Diátaxis)

FailExtract documentation follows the [Diátaxis](https://diataxis.fr/) framework:

- **Tutorials** (`tutorials/`): Step-by-step learning experiences
  - Getting started with FailExtract
  - Integrating with pytest
  - Creating custom formatters
  
- **How-to Guides** (`how-to/`): Solutions to specific problems
  - Installing FailExtract
  - Setting up CI/CD integration
  - Generating reports
  - Troubleshooting issues
  
- **Discussions** (`discussions/`): Background knowledge and context
  - Architectural philosophy
  - Design decisions and tradeoffs
  - Testing strategy
  - Performance considerations
  
- **Reference** (`reference/`): Technical specifications
  - API documentation (auto-generated)
  - Configuration options
  - File formats

### Content Guidelines

**Code Examples**
- Always use working, tested examples
- Include complete imports and setup
- Test examples with `make doctest`

```python
# ✅ Good: Complete, working example
from failextract import extract_on_failure

@extract_on_failure
def test_example():
    assert 1 == 2, "This failure will be captured"

# ❌ Bad: Incomplete or hypothetical example  
@some_decorator
def test_something():
    # This doesn't actually work
    pass
```

**File Formats**
- Use `.rst` files for structured documentation with cross-references
- Use `.md` files for discussions and explanatory content
- Include cross-references between related sections

**Writing Style**
- Use clear, concise language
- Include practical examples
- Link to related sections
- Keep content synchronized with code changes

### Code Documentation Standards

**Docstrings**
- Use Google-style docstrings for all public APIs
- Include usage examples in docstrings
- Document all parameters, return values, and exceptions

```python
def extract_failure_data(test_result: TestResult) -> Dict[str, Any]:
    """Extract failure information from a test result.
    
    Args:
        test_result: The test result containing failure information.
        
    Returns:
        Dictionary containing extracted failure data with keys:
        - 'test_name': Name of the failed test
        - 'error_message': The failure message
        - 'traceback': Full traceback information
        
    Raises:
        ValueError: If test_result is not a valid test result object.
        
    Example:
        >>> result = run_test(failing_test)
        >>> data = extract_failure_data(result)
        >>> print(data['test_name'])
        'test_example_failure'
    """
```

**API Documentation**
- API docs are auto-generated from source code docstrings
- Regenerate with `make apidoc` after adding new modules
- Use type hints for all public functions and classes

## 🔧 Development Workflow

### 1. Local Development
```bash
cd docs
make watch  # Start auto-rebuild server
# Open http://localhost:8000 in browser
# Edit files in source/ directory
# Changes automatically rebuild and refresh browser
```

### 2. Adding New Content
```bash
# Add new tutorial
touch source/tutorials/new_feature.rst
# Edit source/tutorials/index.rst to include it

# Add new discussion
touch source/discussions/new_topic.md
# Edit source/discussions/index.rst to include it

# Regenerate API docs after code changes
make apidoc
```

### 3. Quality Assurance
```bash
# Check documentation quality before committing
make quality

# Check specific aspects
make linkcheck    # Verify external links
make doctest      # Test code examples
make coverage     # Check documentation coverage
```

### 4. Commit Process
```bash
# Run quality checks
make quality

# Commit changes
git add docs/
git commit -m "docs: improve installation guide"

# Push - GitHub Actions will build and deploy automatically
git push
```

## 🌐 Deployment

### Automatic Deployment
Documentation deploys automatically via GitHub Actions:

- **Pull Requests**: Build and validate (no deployment)
- **Push to main**: Build and deploy to GitHub Pages  
- **Releases**: Build and deploy to GitHub Pages

The deployed site is available at: https://dingo-actual.github.io/failextract/

### GitHub Actions Workflow
The `.github/workflows/docs.yml` workflow:
1. Installs Python and dependencies
2. Builds documentation with `make html`
3. Runs quality checks (`make linkcheck`, `make doctest`)
4. Uploads build artifacts
5. Deploys to GitHub Pages (main branch only)

### Manual Deployment
```bash
# Build for production deployment
make production

# The docs/build/html/ directory contains deployable files
# Upload to your hosting service or GitHub Pages
```

## 🐛 Troubleshooting

### Common Build Issues

**"Module not found" errors during build**
```bash
# Install FailExtract in development mode
uv pip install -e .

# Ensure documentation dependencies are installed
uv pip install -r docs/requirements.txt
```

**"sphinx-build not found"**
```bash
# Install Sphinx and extensions
uv pip install -r docs/requirements.txt

# Or use the Makefile
make install-deps
```

**API documentation is missing or outdated**
```bash
# Regenerate API documentation from current source
make apidoc

# Clean and rebuild everything
make clean
make html
```

**External link check failures**
```bash
# Run link checker
make linkcheck

# Check output for broken links
cat build/linkcheck/output.txt

# Some external links may be temporarily unavailable
# This won't prevent building documentation
```

**Build cache issues**
```bash
# Clean all build artifacts
make clean

# Rebuild from scratch
make html
```

**Local server port conflicts**
```bash
# Use different port if 8000 is in use
cd build/html && python -m http.server 8080

# Or kill existing server
lsof -ti:8000 | xargs kill
```

### GitHub Pages Issues

**GitHub Pages shows only README.md**
- Check that GitHub Pages source is set to "GitHub Actions" in repo settings
- Verify the GitHub Actions workflow completed successfully
- Check that `docs/build/html/` contains built files after local build

**Documentation not updating after push**
- Check the GitHub Actions workflow logs for errors
- Ensure all dependencies are correctly specified in `requirements.txt`
- Verify that changes are in the main branch

## 📚 Documentation Tools and Resources

### Sphinx Resources
- [Sphinx Documentation](https://www.sphinx-doc.org/)
- [reStructuredText Primer](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html)
- [Sphinx Extensions](https://www.sphinx-doc.org/en/master/usage/extensions/index.html)

### MyST Markdown
- [MyST Parser Documentation](https://myst-parser.readthedocs.io/)
- [MyST Syntax Guide](https://myst-parser.readthedocs.io/en/latest/syntax/syntax.html)

### Documentation Philosophy
- [Diátaxis Framework](https://diataxis.fr/)
- [Write the Docs Community](https://www.writethedocs.org/)

### FailExtract-Specific Tools
- **Theme**: sphinx-rtd-theme (Read the Docs theme)
- **API Docs**: sphinx.ext.autodoc with Google-style docstrings
- **Code Examples**: sphinx.ext.doctest for testing examples
- **Cross-references**: sphinx.ext.intersphinx for linking to external docs

## 🤝 Contributing to Documentation

### Before You Start
1. Read this guide completely
2. Set up local development environment (`make setup`)
3. Review existing documentation structure
4. Check out [Diátaxis framework](https://diataxis.fr/) principles

### Contribution Checklist
- [ ] Follow the Diátaxis documentation structure
- [ ] Include working, tested code examples
- [ ] Use appropriate file format (.rst vs .md)
- [ ] Run quality checks (`make quality`)
- [ ] Test documentation builds locally (`make html`)
- [ ] Update this README if adding new workflows or tools
- [ ] Link to related sections appropriately

### Review Process
1. Create documentation changes in a feature branch
2. Run `make quality` to check for issues
3. Open a pull request with description of changes
4. GitHub Actions will build and validate documentation
5. Maintainers will review content and structure
6. Merge triggers automatic deployment to GitHub Pages

### Getting Help
- Check existing documentation and this README first
- Review Sphinx and MyST documentation for syntax questions
- Open an issue for documentation bugs or unclear content
- Join discussions about documentation improvements

Happy documenting! 📖