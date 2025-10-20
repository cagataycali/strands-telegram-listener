# Contributing to Strands Telegram Listener

Thank you for your interest in contributing to the Strands Telegram Listener tool! This document provides guidelines and information about contributing to this project.

## 🚀 Getting Started

### Development Setup

1. **Fork and clone the repository:**
   ```bash
   git clone https://github.com/strands-agents/strands-telegram-listener.git
   cd strands-telegram-listener
   ```

2. **Create a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install development dependencies:**
   ```bash
   pip install -e ".[dev,test]"
   ```

4. **Install pre-commit hooks:**
   ```bash
   pre-commit install
   ```

### Development Workflow

1. **Create a feature branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes and test:**
   ```bash
   # Run tests
   make test
   
   # Run linting
   make lint
   
   # Run formatting
   make format
   ```

3. **Commit and push:**
   ```bash
   git add .
   git commit -m "feat: add your feature description"
   git push origin feature/your-feature-name
   ```

4. **Create a Pull Request**

## 🧪 Testing

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=strands_telegram_listener

# Run specific test file
pytest tests/test_telegram_listener.py

# Run with verbose output
pytest -v
```

### Testing Background Processes

Since this tool involves background threading, special considerations apply:

```python
import threading
import time
from unittest.mock import Mock

def test_background_processing():
    """Test background listener functionality."""
    mock_agent = Mock()
    listener = TelegramListener(agent=mock_agent)
    
    # Start listener in background
    listener.start()
    
    # Wait for initialization
    time.sleep(0.1)
    
    # Test functionality
    assert listener.is_running
    
    # Clean up
    listener.stop()
```

### Writing Tests

- Write tests for all new functionality
- Follow the existing test patterns in `tests/`
- Use descriptive test names
- Mock external API calls using `responses` library
- Handle threading properly in tests
- Aim for high test coverage (>80%)

### Test Structure

```python
import pytest
import responses
import threading
from strands_telegram_listener import telegram_listener

class TestTelegramListener:
    def setup_method(self):
        """Set up test fixtures."""
        self.mock_agent = Mock()
    
    @responses.activate
    def test_listener_success(self):
        """Test successful listener operation."""
        # Mock API response
        responses.add(...)
        
        # Test the listener
        result = telegram_listener(...)
        
        # Assert results
        assert result["status"] == "success"
```

## 🏗️ Code Style

We use several tools to maintain code quality:

- **Black** for code formatting
- **isort** for import sorting
- **flake8** for linting
- **mypy** for type checking

### Style Guidelines

1. **Code Formatting:**
   ```bash
   black src tests
   isort src tests
   ```

2. **Type Hints:**
   - Use type hints for all function parameters and return values
   - Import types from `typing` module when needed

3. **Thread Safety:**
   - Use proper locking mechanisms
   - Handle thread cleanup properly
   - Document thread safety considerations

4. **Docstrings:**
   - Use Google-style docstrings
   - Document all public functions and classes
   - Include examples when helpful

```python
def example_function(param: str, optional_param: int = 10) -> Dict[str, Any]:
    """Brief description of the function.

    Args:
        param: Description of the parameter
        optional_param: Description with default value

    Returns:
        Dict containing the result with status and content

    Raises:
        ValueError: When param is invalid

    Example:
        >>> result = example_function("test")
        >>> print(result["status"])
        success
    """
    pass
```

## 🧵 Threading Guidelines

Since this tool uses background threading:

### Thread Safety

1. **Use proper locking:**
   ```python
   import threading
   
   class ThreadSafeClass:
       def __init__(self):
           self._lock = threading.Lock()
           self._data = {}
       
       def update_data(self, key, value):
           with self._lock:
               self._data[key] = value
   ```

2. **Clean up resources:**
   ```python
   def stop(self):
       """Stop background processing and clean up resources."""
       self._running = False
       if self._thread and self._thread.is_alive():
           self._thread.join(timeout=5.0)
   ```

3. **Handle exceptions:**
   ```python
   def background_task(self):
       """Background processing with proper exception handling."""
       try:
           while self._running:
               # Process messages
               pass
       except Exception as e:
           logger.error(f"Background task error: {e}")
       finally:
           # Cleanup code
           pass
   ```

## 📝 Commit Guidelines

We follow [Conventional Commits](https://www.conventionalcommits.org/) specification:

### Commit Message Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Types

- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code style changes (formatting, etc.)
- **refactor**: Code refactoring
- **test**: Adding or updating tests
- **chore**: Maintenance tasks

### Examples

```bash
feat: add message filtering capabilities
fix: handle threading shutdown properly
docs: update README with new examples
test: add tests for background processing
```

## 🐛 Bug Reports

When reporting bugs, please include:

1. **Clear description** of the issue
2. **Steps to reproduce** the problem
3. **Expected vs actual behavior**
4. **Environment details** (Python version, OS, etc.)
5. **Threading information** (if relevant)
6. **Code examples** if applicable

Use the bug report template when creating issues.

## 💡 Feature Requests

For feature requests:

1. **Describe the use case** and motivation
2. **Provide examples** of how it would be used
3. **Consider threading implications**
4. **Consider backward compatibility**
5. **Check existing issues** to avoid duplicates

## 📋 Pull Request Guidelines

### Before Submitting

- [ ] Tests pass locally (including threading tests)
- [ ] Code follows style guidelines
- [ ] Documentation is updated
- [ ] CHANGELOG.md is updated (if applicable)
- [ ] Commit messages follow convention
- [ ] Thread safety considered

### PR Description

Include in your PR description:

1. **Summary** of changes
2. **Motivation** for the changes
3. **Testing** performed (including threading tests)
4. **Breaking changes** (if any)
5. **Related issues** (if applicable)

## 🏆 Recognition

Contributors will be recognized in:

- README.md contributors section
- CHANGELOG.md for significant contributions
- GitHub releases notes

## 📞 Getting Help

- **Discussions**: Use GitHub Discussions for questions
- **Issues**: Use GitHub Issues for bugs and feature requests
- **Discord**: Join the Strands Agents Discord community

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

Thank you for contributing to Strands Telegram Listener! 🚀