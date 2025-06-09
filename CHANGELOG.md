# Changelog

All notable changes to FailExtract will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-06-09

### Added

#### Core Features
- **Test Failure Extraction**: Comprehensive failure data collection with pytest integration
- **Multiple Output Formats**: JSON (built-in), XML, CSV, Markdown, and YAML formatters
- **Decorator Support**: Simple `@extract_on_failure` decorator for any test function
- **Command-Line Interface**: Full CLI with report generation, statistics, and management commands
- **Fixture Analysis**: Deep pytest fixture dependency analysis and context extraction
- **Thread-Safe Operation**: Safe for concurrent test execution environments

#### Architecture
- **Progressive Enhancement**: Core functionality with optional feature layers
- **Plugin Architecture**: Extensible formatter system with registry pattern
- **Multi-Modal Performance**: Static, Profile, and Trace modes with different overhead profiles
- **Graceful Degradation**: Optional features fail helpfully with clear upgrade paths

#### Formatters
- **JSON Formatter**: Built-in structured data output
- **XML Formatter**: Built-in XML report generation  
- **CSV Formatter**: Built-in tabular data export
- **Markdown Formatter**: Built-in human-readable reports
- **YAML Formatter**: Optional YAML output (requires `[formatters]` extra)

#### CLI Features
- **Feature Detection**: Adapts to installed optional dependencies
- **Report Generation**: Multiple format support with configurable options
- **Statistics Display**: Failure analysis and summary information
- **Data Management**: List, clear, and manage stored failure data
- **Rich Output**: Enhanced terminal formatting (requires `[cli]` extra)

#### Configuration
- **Flexible Configuration**: Simple strings to complex OutputConfig objects
- **Enhanced Validation**: Optional Pydantic-based config validation (requires `[config]` extra)
- **Progressive Options**: Start simple, add complexity as needed

#### Documentation
- **Comprehensive Documentation**: Full Sphinx-based documentation site
- **Diátaxis Framework**: Tutorials, how-to guides, discussions, and reference
- **API Documentation**: Complete API reference with examples
- **GitHub Pages**: Automated documentation deployment

#### Development Infrastructure
- **Comprehensive Testing**: Unit, integration, performance, and property-based tests
- **Code Quality**: Ruff formatting, MyPy type checking, 96% test coverage
- **CI/CD Pipeline**: Automated documentation builds and deployment
- **Development Tools**: Complete development environment setup

### Changed
- **License**: Switched to Apache 2.0 license for broader compatibility
- **Documentation Structure**: Reorganized following Diátaxis framework principles
- **Test Organization**: Restructured from 12 monolithic to 36 focused test files

### Technical Details

#### Performance Characteristics
- **Static Mode**: <5% overhead for production use
- **Profile Mode**: ~50% overhead for development debugging
- **Trace Mode**: ~300% overhead for detailed analysis

#### Dependencies
- **Core**: Zero required dependencies for basic functionality
- **Optional**: Minimal optional dependencies for enhanced features
  - `pyyaml>=5.4` for YAML formatting
  - `pydantic>=1.10` for enhanced configuration
  - `rich>=12.0, typer>=0.7` for enhanced CLI

#### Requirements
- **Python**: 3.11+ required
- **Framework**: pytest integration (pytest not required as dependency)
- **Operating System**: Cross-platform compatibility

#### Installation Options
```bash
# Core functionality
pip install failextract

# With optional features  
pip install failextract[formatters]  # YAML support
pip install failextract[config]     # Enhanced configuration
pip install failextract[cli]        # Rich CLI output
pip install failextract[all]        # All features
pip install failextract[development] # Development tools
```

### Security
- **No Secret Exposure**: Never logs or exposes sensitive information
- **Safe Defaults**: Secure configuration defaults
- **Input Validation**: Robust handling of malformed test data

### Documentation Links
- **Homepage**: https://github.com/dingo-actual/failextract
- **Documentation**: https://dingo-actual.github.io/failextract/
- **Issues**: https://github.com/dingo-actual/failextract/issues
- **PyPI**: https://pypi.org/project/failextract/

---

## Development History

This release represents the culmination of intensive development from June 3-6, 2025, transforming an empty module into a production-ready library with:

- **1,128 lines** of focused, high-quality code
- **311 comprehensive tests** with 96% coverage
- **Professional documentation** with automated deployment
- **Strategic feature focus** prioritizing user value over technical complexity

For detailed development insights, see the [Development Journey](https://dingo-actual.github.io/failextract/discussions/development_journey.html) documentation.

---

## Format Guide

### Categories
- **Added**: New features
- **Changed**: Changes in existing functionality  
- **Deprecated**: Soon-to-be removed features
- **Removed**: Removed features
- **Fixed**: Bug fixes
- **Security**: Vulnerability fixes

### Version Format
- **[MAJOR.MINOR.PATCH]** following semantic versioning
- **MAJOR**: Breaking changes
- **MINOR**: New features, backward compatible
- **PATCH**: Bug fixes, backward compatible