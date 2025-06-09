# FailExtract: Test Failure Analysis Library

<img src="images/failextract_logo.png" alt="FailExtract Logo" width="830" height="415">
<br>

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![PyPI version](https://badge.fury.io/py/failextract.svg)](https://badge.fury.io/py/failextract)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Documentation](https://img.shields.io/badge/docs-GitHub%20Pages-blue.svg)](https://dingo-actual.github.io/failextract/index.html)

**FailExtract** is a lightweight test failure extraction and reporting library for pytest environments. It automatically captures detailed failure context including fixture information, source code, and exception details to improve debugging efficiency.

📖 **[Read the Full Documentation](https://dingo-actual.github.io/failextract/index.html)** for tutorials, guides, and API reference.

## Key Features

- **🎯 Simple Installation**: Easy to get started with minimal dependencies
- **📊 Multiple Output Formats**: JSON (built-in), XML, CSV, YAML, and Markdown
- **⚡ Lightweight Core**: Fast extraction with configurable output options
- **🎨 Command-Line Interface**: Generate reports and analyze failures from CLI
- **🧵 Thread-Safe**: Works with concurrent test execution
- **⚙️ Decorator Support**: Simple `@extract_on_failure` decorator for any test
- **🔧 Configurable**: Flexible output configuration and formatting options

## Installation Options

### Installation 

```bash
# Standard installation - includes all core formatters
pip install failextract
```

**Includes**: Failure extraction, JSON/XML/CSV/Markdown formatters, and CLI commands. See the [installation guide](https://dingo-actual.github.io/failextract/how-to/install_failextract.html) for details.

### Optional Features

```bash
# Add YAML output support
pip install failextract[formatters]

# Enhanced configuration file support (pydantic)
pip install failextract[config]

# Rich CLI output formatting
pip install failextract[cli]
```

### Development Installation

```bash
# All optional features for development
pip install failextract[development]

# Install with all features
pip install failextract[all]
```

### Check Available Features

```bash
# See what features are installed
failextract features

# Get installation suggestions for missing features
failextract --help
```

### Basic Usage

Transform any test function into a failure-capturing test with a simple decorator:

```python
from failextract import extract_on_failure

@extract_on_failure
def test_user_registration():
    user = create_user("john@example.com")
    assert user.is_active == True, "User should be active after registration"
```

When this test fails, FailExtract automatically captures:
- Complete fixture dependency chain
- Source code context
- Exception details with full traceback
- Local variable states
- Test execution metadata

Learn more in the [getting started tutorial](https://dingo-actual.github.io/failextract/tutorials/getting_started.html).

### Generate Reports (Core - Always Available)

```python
from failextract import FailureExtractor, OutputConfig

# Extract failures and generate reports
extractor = FailureExtractor()

# Generate JSON report (always available)
json_config = OutputConfig("failures.json", format="json")
extractor.save_report(json_config)
```

### Multiple Output Formats

```python
# XML output (built-in)
xml_config = OutputConfig("failures.xml", format="xml")
extractor.save_report(xml_config)

# Markdown output (built-in)
md_config = OutputConfig("failures.md", format="markdown")
extractor.save_report(md_config)

# YAML output (requires formatters extra)
yaml_config = OutputConfig("failures.yaml", format="yaml")
extractor.save_report(yaml_config)
```

See the [multiple formats tutorial](https://dingo-actual.github.io/failextract/tutorials/multiple_formats.html) for examples with all supported formats.

### Command-Line Interface

FailExtract includes a feature-aware CLI that adapts to your installed extras:

```bash
# Check what features you have installed
failextract features

# Generate JSON report (always available)
failextract report --format json --output failures.json

# Show failure statistics
failextract stats

# List all captured failures
failextract list --format table

# Clear stored failure data
failextract clear --confirm
```

For complete CLI documentation, see the [CLI reference](https://dingo-actual.github.io/failextract/reference/api.html#command-line-interface).

### CI/CD Integration Examples

**Basic CI/CD (Core Only):**
```yaml
# GitHub Actions example - basic JSON reporting
- name: Generate failure report
  if: always()
  run: |
    failextract report --format json --output test-failures.json
    failextract stats
```

**Enhanced CI/CD (Multiple Formats):**
```yaml
# GitHub Actions with multiple formats
- name: Generate comprehensive failure reports
  if: always()
  run: |
    failextract report --format json --output test-failures.json
    failextract report --format markdown --output test-failures.md
    failextract report --format xml --output test-failures.xml
```

See the [CI/CD setup guide](https://dingo-actual.github.io/failextract/how-to/setup_ci_cd.html) for complete integration examples.

### Progressive Enhancement Examples

**Start Simple:**
```python
# Just the basics - lightweight and fast
from failextract import extract_on_failure

@extract_on_failure  
def test_basic():
    assert 1 == 2, "This will capture failure details"
```

**Add YAML Formatting:**
```bash
pip install failextract[formatters]
```
```python
# Now use YAML output format
from failextract import FailureExtractor, OutputConfig

extractor = FailureExtractor()
config = OutputConfig("report.yaml", format="yaml", include_source=True)
extractor.save_report(config)
```

**Add Rich CLI Output:**
```bash
pip install failextract[cli]
```
```bash
# Now get enhanced terminal output
failextract report --format json --output failures.json
failextract stats  # Enhanced with rich formatting
```

Learn about all configuration options in the [configuration tutorial](https://dingo-actual.github.io/failextract/tutorials/configuration.html).

## Documentation

Complete documentation is available at **[dingo-actual.github.io/failextract](https://dingo-actual.github.io/failextract/index.html)**:

- **[Getting Started Tutorial](https://dingo-actual.github.io/failextract/tutorials/getting_started.html)** - 5-minute walkthrough
- **[How-To Guides](https://dingo-actual.github.io/failextract/how-to/index.html)** - Practical solutions for common tasks
- **[API Reference](https://dingo-actual.github.io/failextract/reference/api.html)** - Complete API documentation
- **[Architecture Discussions](https://dingo-actual.github.io/failextract/discussions/index.html)** - Design patterns and philosophy

## Contributing

We welcome contributions! Please see our [contributing guidelines](./CHANGELOG.md) for details on setting up the development environment and submitting pull requests.

### Development Setup

```bash
# Clone the repository
git clone <repository-url>
cd failextract

# Create virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install in development mode with all features
pip install -e .[development,docs,all]

# Run tests
pytest

# Run linting and formatting
ruff check src/ tests/
ruff format src/ tests/

# Build documentation
cd docs && make html
```

## Project Structure

```
failextract/
├── src/failextract/           # Main package source
│   ├── __init__.py           # Public API exports
│   ├── cli.py                # Command-line interface
│   ├── configuration.py      # Configuration management
│   ├── failextract.py        # Core extraction functionality
│   ├── api/                  # API protocols and events
│   ├── core/                 # Core functionality
│   │   ├── analysis/         # Context analysis
│   │   ├── extraction/       # Failure extraction
│   │   └── formatters/       # Output formatters
│   └── integrations/         # Framework integrations
├── tests/                    # Comprehensive test suite
│   ├── unit/                 # Unit tests
│   ├── integration/          # Integration tests
│   ├── performance/          # Performance tests
│   └── property/             # Property-based tests
├── examples/                 # Working examples
├── docs/                     # Documentation source
└── pyproject.toml           # Project configuration
```

## Features by Extra

- **Core** (always available): JSON, XML, CSV, Markdown formatters, CLI, basic extraction
- **`[formatters]`**: YAML output format (`pyyaml`)
- **`[config]`**: Enhanced configuration with Pydantic validation
- **`[cli]`**: Rich terminal output formatting (`rich`, `typer`)
- **`[development]`**: All development tools (`pytest`, `ruff`, `mypy`, etc.)
- **`[docs]`**: Documentation building tools (`sphinx`, themes, etc.)
- **`[all]`**: All optional features combined

## License

FailExtract is released under the Apache 2.0 License. See [LICENSE](LICENSE) for details.
