# Python Coding Standards

## Overview

Python is used for backend services, data processing, and automation at Solution8. We follow PEP 8 with some additional standards.

## Python Version

- Use **Python 3.11+** for all new projects (Python 3.12+ recommended)
- Stay updated with the latest stable Python version
- Leverage modern Python features and performance improvements

## Code Style

### PEP 8 Compliance
- Follow [PEP 8](https://www.python.org/dev/peps/pep-0008/) style guide
- Use tools like `black`, `flake8`, and `pylint` for enforcement

### Formatting
```python
# Use 4 spaces for indentation
def calculate_total(items):
    total = 0
    for item in items:
        total += item.price
    return total

# Maximum line length: 88 characters (Black default)
# Use double quotes for strings
message = "Hello, World!"

# Use trailing commas in multi-line structures
user_data = {
    "name": "John Doe",
    "email": "john@example.com",
    "age": 30,
}
```

## Naming Conventions

```python
# snake_case for functions and variables
def get_user_data(user_id):
    user_name = fetch_name(user_id)
    return user_name

# PascalCase for classes
class UserService:
    pass

# UPPER_CASE for constants
MAX_RETRY_COUNT = 3
API_ENDPOINT = "https://api.example.com"

# _leading_underscore for internal/private
class MyClass:
    def __init__(self):
        self._internal_value = 0
    
    def _private_method(self):
        pass
```

## Type Hints

Use type hints for all function signatures and class attributes:

```python
from typing import List, Optional, Dict, Any

def get_user(user_id: int) -> Optional[Dict[str, Any]]:
    """Fetch user data by ID."""
    user = database.query(user_id)
    return user if user else None

def process_items(items: List[str], limit: int = 10) -> List[str]:
    """Process a list of items with an optional limit."""
    return items[:limit]

class User:
    def __init__(self, name: str, email: str, age: int) -> None:
        self.name: str = name
        self.email: str = email
        self.age: int = age
```

## Documentation

### Docstrings
Use Google-style or NumPy-style docstrings:

```python
def calculate_discount(price: float, discount_rate: float) -> float:
    """Calculate the final price after applying a discount.
    
    Args:
        price: The original price of the item
        discount_rate: The discount rate (0.0 to 1.0)
    
    Returns:
        The final price after discount
    
    Raises:
        ValueError: If discount_rate is not between 0 and 1
    
    Example:
        >>> calculate_discount(100.0, 0.2)
        80.0
    """
    if not 0 <= discount_rate <= 1:
        raise ValueError("Discount rate must be between 0 and 1")
    
    return price * (1 - discount_rate)
```

## Error Handling

```python
# Use specific exception types
try:
    user = get_user(user_id)
except UserNotFoundError as e:
    logger.error(f"User not found: {user_id}", exc_info=True)
    raise
except DatabaseError as e:
    logger.error("Database error occurred", exc_info=True)
    return None

# Create custom exceptions
class ValidationError(Exception):
    """Raised when data validation fails."""
    pass

# Use context managers for resource management
with open("data.txt", "r") as file:
    data = file.read()
```

## Modern Python Features

### F-Strings
```python
# Use f-strings for string formatting
name = "Alice"
age = 30
message = f"User {name} is {age} years old"

# For complex formatting
price = 19.99
formatted = f"Price: ${price:.2f}"
```

### Comprehensions
```python
# List comprehensions
squares = [x**2 for x in range(10)]
even_numbers = [x for x in range(20) if x % 2 == 0]

# Dictionary comprehensions
user_ages = {user.name: user.age for user in users}

# Set comprehensions
unique_values = {item.value for item in items}
```

### Dataclasses
```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class User:
    name: str
    email: str
    age: int
    roles: List[str] = field(default_factory=list)
    
    def is_admin(self) -> bool:
        return "admin" in self.roles
```

### Pattern Matching (Python 3.10+)
```python
def process_command(command: str, args: List[str]) -> str:
    match command:
        case "start":
            return start_service()
        case "stop":
            return stop_service()
        case "status":
            return get_status()
        case _:
            return "Unknown command"
```

## Project Structure

```
project/
├── src/
│   ├── __init__.py
│   ├── main.py
│   ├── models/
│   │   ├── __init__.py
│   │   └── user.py
│   ├── services/
│   │   ├── __init__.py
│   │   └── user_service.py
│   └── utils/
│       ├── __init__.py
│       └── helpers.py
├── tests/
│   ├── __init__.py
│   ├── test_models.py
│   └── test_services.py
├── requirements.txt
├── setup.py
└── README.md
```

## Dependency Management

### Use Poetry or pip-tools
```toml
# pyproject.toml (Poetry)
[tool.poetry]
name = "my-project"
version = "0.1.0"

[tool.poetry.dependencies]
python = "^3.10"
fastapi = "^0.100.0"
sqlalchemy = "^2.0.0"

[tool.poetry.dev-dependencies]
pytest = "^7.0.0"
black = "^23.0.0"
```

### requirements.txt
```
# Pin major versions, allow minor updates
fastapi>=0.100.0,<0.101.0
sqlalchemy>=2.0.0,<3.0.0
pydantic>=2.0.0,<3.0.0
```

## Testing

### Use pytest
```python
import pytest
from myapp.services import UserService

class TestUserService:
    @pytest.fixture
    def user_service(self):
        return UserService()
    
    def test_create_user_success(self, user_service):
        user_data = {
            "name": "John Doe",
            "email": "john@example.com"
        }
        user = user_service.create_user(user_data)
        
        assert user.id is not None
        assert user.name == user_data["name"]
    
    def test_create_user_invalid_email(self, user_service):
        user_data = {
            "name": "John Doe",
            "email": "invalid-email"
        }
        
        with pytest.raises(ValidationError):
            user_service.create_user(user_data)
    
    @pytest.mark.asyncio
    async def test_async_operation(self, user_service):
        result = await user_service.async_operation()
        assert result is not None
```

### Test Coverage
- Aim for 80%+ code coverage
- Use `pytest-cov` for coverage reports
- Run tests: `pytest --cov=src tests/`

## Linting and Formatting

### Required Tools
```bash
# Install development tools
pip install black flake8 mypy pylint isort

# Format code
black src/

# Sort imports
isort src/

# Check style
flake8 src/

# Type checking
mypy src/

# Lint code
pylint src/
```

### Configuration (.flake8)
```ini
[flake8]
max-line-length = 88
extend-ignore = E203, W503
exclude = .git,__pycache__,venv
```

### Configuration (pyproject.toml)
```toml
[tool.black]
line-length = 88
target-version = ['py310']

[tool.isort]
profile = "black"

[tool.mypy]
python_version = "3.10"
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = true
```

## Async/Await

```python
import asyncio
from typing import List

async def fetch_user(user_id: int) -> User:
    """Fetch user asynchronously."""
    async with httpx.AsyncClient() as client:
        response = await client.get(f"/users/{user_id}")
        return User(**response.json())

async def fetch_multiple_users(user_ids: List[int]) -> List[User]:
    """Fetch multiple users concurrently."""
    tasks = [fetch_user(uid) for uid in user_ids]
    return await asyncio.gather(*tasks)
```

## Security Best Practices

- Never commit secrets or credentials
- Use environment variables for configuration
- Validate and sanitize all inputs
- Use parameterized queries to prevent SQL injection
- Keep dependencies up-to-date

```python
import os
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

DATABASE_URL = os.getenv("DATABASE_URL")
API_KEY = os.getenv("API_KEY")

# Validate input
def validate_email(email: str) -> bool:
    import re
    pattern = r'^[\w\.-]+@[\w\.-]+\.\w+$'
    return bool(re.match(pattern, email))
```
