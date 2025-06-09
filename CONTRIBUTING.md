# Contributing to FailExtract

We welcome contributions to FailExtract! This guide will help you get started with development, testing, and submitting contributions.

## Quick Start

### Development Setup

```bash
# Clone the repository
git clone https://github.com/dingo-actual/failextract.git
cd failextract

# Create virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install in development mode with all features
pip install -e .[development,docs,all]

# Verify installation
pytest
```

### Making Your First Contribution

1. **Fork the repository** on GitHub
2. **Create a feature branch**: `git checkout -b feature/your-feature-name`
3. **Make your changes** following our coding standards
4. **Run tests**: `pytest`
5. **Run linting**: `ruff check src/ tests/ && ruff format src/ tests/`
6. **Submit a pull request** with a clear description

## Development Environment

### Required Tools

- **Python 3.11+** (required)
- **Git** for version control
- **Virtual environment** (recommended)

### Optional Tools (for full development experience)

- **ruff** for linting and formatting (included in `[development]`)
- **mypy** for type checking (included in `[development]`)
- **sphinx** for documentation (included in `[docs]`)

### Installation Commands

```bash
# Minimal development setup
pip install -e .[development]

# Full development + documentation
pip install -e .[development,docs,all]

# Individual feature sets
pip install -e .[formatters]  # YAML support
pip install -e .[config]     # Enhanced configuration
pip install -e .[cli]        # Rich CLI output
```

## Code Quality Standards

### Code Style

We use **Black** formatting with 88-character line length via **ruff**:

```bash
# Format code
ruff format src/ tests/

# Check for style violations
ruff check src/ tests/

# Fix auto-fixable issues
ruff check --fix src/ tests/
```

### Type Checking

All code must include type hints:

```bash
# Run type checking
mypy src/
```

### Documentation Standards

- **Google-style docstrings** for all public APIs
- **Type hints** for all function signatures
- **Examples** in docstrings for complex functions
- **Module docstrings** explaining purpose and key components

Example:
```python
def extract_failures(config: OutputConfig) -> List[FailureData]:
    """Extract test failure information with specified configuration.
    
    Args:
        config: Output configuration specifying format and options
        
    Returns:
        List of extracted failure data objects
        
    Raises:
        ExtractionError: If failure extraction fails
        
    Example:
        >>> config = OutputConfig("report.json", format="json")
        >>> failures = extract_failures(config)
        >>> len(failures)
        3
    """
```

## Testing Strategy

### Test Organization

Our test suite follows a comprehensive structure:

```
tests/
├── unit/           # Fast, isolated unit tests
├── integration/    # Component interaction tests  
├── performance/    # Performance benchmarks
├── property/       # Property-based tests
└── fixtures/       # Shared test data
```

### Running Tests

```bash
# Full test suite
pytest

# Specific test categories
pytest tests/unit/          # Unit tests only
pytest tests/integration/   # Integration tests only
pytest tests/performance/   # Performance tests only

# With coverage
pytest --cov=src/failextract

# Specific test patterns
pytest -k "test_json_formatter"
pytest tests/unit/formatters/test_json_formatter.py

# Performance tests with statistics
pytest tests/performance/ --hypothesis-show-statistics
```

### Writing Tests

#### Test Naming Convention

```python
def test_feature_behavior_when_condition():
    """Test names should describe the behavior being tested."""
```

#### Test Categories

1. **Unit Tests**: Test individual components in isolation
2. **Integration Tests**: Test component interactions
3. **Property Tests**: Use Hypothesis for property-based testing
4. **Performance Tests**: Verify performance characteristics

#### Example Test Structure

```python
def test_json_formatter_outputs_valid_json_with_failure_data():
    """JSON formatter should produce valid JSON with complete failure data."""
    # Arrange
    formatter = JSONFormatter()
    failure_data = [create_test_failure()]
    
    # Act
    result = formatter.format(failure_data)
    
    # Assert
    parsed = json.loads(result)  # Should not raise
    assert "failures" in parsed
    assert len(parsed["failures"]) == 1
```

### Performance Testing

Performance is a first-class concern. All contributions should maintain performance characteristics:

```bash
# Run performance tests
pytest tests/performance/

# Profile performance (if py-spy is available)
py-spy record -f speedscope -o profile.json -- python -m pytest tests/performance/

# Memory profiling (if memray is available)
memray run -o memory.bin python -m pytest tests/performance/
memray stats memory.bin
```

## Architecture Guidelines

### Core Principles

1. **Progressive Enhancement**: Start simple, enable sophistication
2. **Minimal Dependencies**: Keep core lightweight
3. **Plugin Architecture**: Enable extension without core modification
4. **Performance Awareness**: Different modes for different contexts

### Module Organization

```python
# src/failextract/
├── __init__.py              # Public API exports only
├── api/                     # Protocols and interfaces
├── core/                    # Core functionality
│   ├── analysis/           # Context analysis
│   ├── extraction/         # Failure extraction
│   └── formatters/         # Output formatters
├── integrations/           # Framework integrations
└── cli.py                  # Command-line interface
```

### Adding New Features

#### 1. Formatters

```python
# src/failextract/core/formatters/my_formatter.py
from .base import OutputFormatter

class MyFormatter(OutputFormatter):
    """Custom formatter implementation."""
    
    def format(self, failures: List[Dict[str, Any]]) -> str:
        """Format failures for output."""
        # Implementation here
```

#### 2. Analysis Components

```python
# src/failextract/core/analysis/my_analyzer.py
from typing import Any, Dict

def analyze_context(test_context: Dict[str, Any]) -> Dict[str, Any]:
    """Analyze test context for additional insights."""
    # Implementation here
```

#### 3. Integration Points

```python
# src/failextract/integrations/my_framework.py
def extract_framework_data(test_item) -> Dict[str, Any]:
    """Extract framework-specific test information."""
    # Implementation here
```

## Documentation

### Building Documentation

```bash
# Install documentation dependencies
pip install -e .[docs]

# Build documentation
cd docs
make html

# Auto-rebuild during development
sphinx-autobuild source build/html
```

### Documentation Structure (Diátaxis Framework)

- **Tutorials**: Step-by-step learning experiences
- **How-To Guides**: Problem-solving instructions
- **Reference**: Technical API documentation
- **Discussions**: Background and design decisions

### Writing Documentation

1. **Start with user needs**: What problem does this solve?
2. **Provide working examples**: All code must be tested
3. **Be honest about limitations**: Don't oversell capabilities
4. **Link related concepts**: Help users discover relevant features

## Contribution Process

### Before You Start

1. **Check existing issues** for similar work
2. **Open an issue** to discuss significant changes
3. **Review our development philosophy** in the docs

### Pull Request Guidelines

#### Title Format
- `feat: Add YAML formatter support`
- `fix: Resolve fixture extraction error`
- `docs: Update installation guide`
- `perf: Optimize failure data collection`

#### Description Template
```markdown
## Summary
Brief description of changes and motivation.

## Changes
- List of specific changes made
- Include any breaking changes

## Testing
- How was this tested?
- Any new test cases added?

## Documentation
- Documentation updated if needed
- Examples provided for new features
```

#### Before Submitting

```bash
# Run full quality checks
ruff check src/ tests/
ruff format src/ tests/
mypy src/
pytest
```

### Code Review Process

1. **Automated checks** must pass (linting, type checking, tests)
2. **Manual review** focuses on:
   - Architecture alignment
   - User experience impact
   - Documentation quality
   - Test coverage
3. **Performance impact** for core functionality changes
4. **Breaking change review** for API modifications

### Breaking Changes

If your change modifies the public API:

1. **Discuss in an issue first**
2. **Provide migration path** in documentation
3. **Update version number** appropriately
4. **Add to changelog**

## Release Process

### Version Numbers

We follow semantic versioning (SemVer):
- **Major** (1.x.x): Breaking changes
- **Minor** (x.1.x): New features, backward compatible
- **Patch** (x.x.1): Bug fixes, backward compatible

### Changelog

All changes should be documented in the appropriate format:

```markdown
## [1.1.0] - 2025-06-XX

### Added
- New YAML formatter support
- Enhanced CLI with progress indicators

### Changed
- Improved fixture analysis performance

### Fixed
- Resolved thread safety issue in extraction
```

## Community Guidelines

### Communication

- **Be respectful** and constructive in discussions
- **Ask questions** if anything is unclear
- **Share context** when reporting issues
- **Help others** when you can

### Getting Help

1. **Check the documentation** first
2. **Search existing issues** for similar problems
3. **Open a new issue** with detailed information
4. **Join discussions** in pull requests and issues

### Reporting Issues

Include:
- **Python version** and operating system
- **FailExtract version** and installed extras
- **Complete error messages** and tracebacks
- **Minimal reproduction example**
- **Expected vs actual behavior**

## Advanced Development

### Performance Optimization

When optimizing performance:

1. **Measure first**: Use profiling tools to identify bottlenecks
2. **Maintain modes**: Preserve different performance/information trade-offs
3. **Test impact**: Verify optimizations don't break functionality
4. **Document trade-offs**: Explain performance characteristics

### Extension Development

For significant extensions:

1. **Design for pluggability**: Use abstract base classes
2. **Maintain isolation**: Don't modify core for extension needs
3. **Document extension points**: Help others build on your work
4. **Provide examples**: Show how to use extension APIs

### Debugging

Development debugging tools:

```bash
# Interactive debugging
python -m pdb script.py

# Performance profiling
py-spy record -f speedscope -o profile.json -- python script.py

# Memory profiling
memray run -o output.bin script.py
memray stats output.bin
```

## Questions?

- **Documentation**: https://dingo-actual.github.io/failextract/
- **Issues**: https://github.com/dingo-actual/failextract/issues
- **Discussions**: Check existing issues and pull requests

We appreciate your interest in contributing to FailExtract!
