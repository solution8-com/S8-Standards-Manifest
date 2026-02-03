# Documentation Best Practices

## Overview

Good documentation is essential for maintaining code, onboarding new team members, and ensuring long-term project success. This guide covers best practices for creating clear, comprehensive, and maintainable documentation.

## Why Documentation Matters

### Developer Productivity
- **Faster Onboarding**: New team members become productive quickly
- **Reduced Context Switching**: Answers available without interrupting others
- **Knowledge Preservation**: Critical information survives team changes
- **Fewer Bugs**: Clear documentation reduces misunderstandings

### Project Success
- **Better Maintenance**: Future developers understand design decisions
- **Easier Collaboration**: Teams work more effectively with shared understanding
- **Improved Quality**: Documentation forces clearer thinking about design
- **Reduced Technical Debt**: Well-documented code is easier to refactor

## Types of Documentation

### 1. Code Comments
Explain **why** code does something, not what it does.

### 2. API Documentation
Detailed reference for interfaces, endpoints, and functions.

### 3. README Files
Quick start guides and project overviews.

### 4. Architecture Documentation
High-level system design and decisions.

### 5. User Guides
End-user instructions for using the software.

### 6. Runbooks
Operational procedures for deployment and maintenance.

## Writing Effective Documentation

### Clarity Principles

**Use Simple Language**:
```markdown
❌ BAD: "Utilize the authentication mechanism to facilitate user ingress"
✅ GOOD: "Use the login feature to let users sign in"
```

**Be Concise**:
```markdown
❌ BAD: "In order to be able to install the dependencies that are 
required by this project, you will need to run the npm install command"

✅ GOOD: "Install dependencies: `npm install`"
```

**Use Active Voice**:
```markdown
❌ BAD: "The configuration file should be edited by the user"
✅ GOOD: "Edit the configuration file"
```

### Structure and Organization

**Use Clear Headings**:
```markdown
# Main Topic (H1 - One per document)

## Section (H2 - Main sections)

### Subsection (H3 - Detailed topics)

#### Minor Point (H4 - When needed)
```

**Table of Contents** for long documents:
```markdown
## Table of Contents
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Troubleshooting](#troubleshooting)
```

**Progressive Disclosure**:
- Start with essential information
- Provide links to detailed documentation
- Use expandable sections for advanced topics

## Code Comments vs Documentation

### When to Comment Code

```javascript
// ✅ GOOD: Explain complex algorithms
/**
 * Implements Dijkstra's algorithm to find shortest path.
 * Time complexity: O((V + E) log V) where V = vertices, E = edges
 */
function findShortestPath(graph, start, end) {
  // Priority queue for exploring nodes by distance
  const queue = new PriorityQueue();
  // ... implementation
}

// ✅ GOOD: Document business logic
/**
 * Calculate discount based on customer tier and purchase history.
 * Premium customers get 20% off, regular customers get 10% off.
 * Additional 5% for purchases over $100.
 */
function calculateDiscount(customer, amount) {
  // ... implementation
}

// ✅ GOOD: Explain non-obvious code
// Using bitwise AND to check if number is even (faster than modulo)
if ((num & 1) === 0) {
  // even number
}

// ❌ BAD: Stating the obvious
// Increment counter
counter++;

// ❌ BAD: Commented-out code (use version control instead)
// function oldImplementation() {
//   // ...
// }
```

### JSDoc for Functions

```javascript
/**
 * Creates a new user account with validation.
 * 
 * @param {Object} userData - User registration data
 * @param {string} userData.email - User's email address
 * @param {string} userData.password - User's password (min 8 chars)
 * @param {string} [userData.name] - Optional display name
 * 
 * @returns {Promise<User>} Newly created user object
 * 
 * @throws {ValidationError} If email format is invalid
 * @throws {DuplicateError} If email already exists
 * 
 * @example
 * const user = await createUser({
 *   email: 'user@example.com',
 *   password: 'securePass123',
 *   name: 'John Doe'
 * });
 */
async function createUser(userData) {
  // Implementation
}
```

### Python Docstrings

```python
def calculate_fibonacci(n: int) -> int:
    """
    Calculate the nth Fibonacci number using dynamic programming.
    
    This function uses an iterative approach with O(n) time complexity
    and O(1) space complexity, making it suitable for large values of n.
    
    Args:
        n: The position in the Fibonacci sequence (0-indexed)
    
    Returns:
        The nth Fibonacci number
    
    Raises:
        ValueError: If n is negative
    
    Examples:
        >>> calculate_fibonacci(0)
        0
        >>> calculate_fibonacci(5)
        5
        >>> calculate_fibonacci(10)
        55
    
    Note:
        For very large values of n (>1000), consider using memoization
        or matrix exponentiation for better performance.
    """
    if n < 0:
        raise ValueError("n must be non-negative")
    
    if n <= 1:
        return n
    
    prev, curr = 0, 1
    for _ in range(2, n + 1):
        prev, curr = curr, prev + curr
    
    return curr
```

## README Structure

### Essential README Template

```markdown
# Project Name

Brief description of what this project does and who it's for.

![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Coverage](https://img.shields.io/badge/coverage-85%25-green)
![License](https://img.shields.io/badge/license-MIT-blue)

## Features

- ✅ Feature 1: Brief description
- ✅ Feature 2: Brief description
- ✅ Feature 3: Brief description

## Quick Start

### Prerequisites

- Node.js 18+
- PostgreSQL 14+
- npm or yarn

### Installation

```bash
# Clone repository
git clone https://github.com/username/project.git

# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env with your settings

# Run database migrations
npm run migrate

# Start development server
npm run dev
```

Visit http://localhost:3000 to see the application.

## Configuration

### Environment Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `DATABASE_URL` | PostgreSQL connection string | - | Yes |
| `JWT_SECRET` | Secret for JWT tokens | - | Yes |
| `PORT` | Server port | 3000 | No |
| `LOG_LEVEL` | Logging level | info | No |

### Example Configuration

```env
DATABASE_URL=postgresql://user:pass@localhost:5432/mydb
JWT_SECRET=your-secret-key-change-in-production
PORT=3000
LOG_LEVEL=debug
```

## Usage

### Basic Example

```javascript
const { createClient } = require('project-name');

const client = createClient({
  apiKey: 'your-api-key'
});

const result = await client.fetchData();
console.log(result);
```

### Advanced Usage

See [detailed documentation](docs/usage.md) for more examples.

## API Reference

### Core Methods

#### `createClient(options)`

Creates a new client instance.

**Parameters:**
- `options.apiKey` (string, required): Your API key
- `options.timeout` (number, optional): Request timeout in ms (default: 5000)

**Returns:** Client instance

### REST API

See [API documentation](docs/api.md) for complete endpoint reference.

## Development

### Running Tests

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run specific test file
npm test -- user.test.js
```

### Building

```bash
# Build for production
npm run build

# Build and watch for changes
npm run build:watch
```

### Linting and Formatting

```bash
# Run linter
npm run lint

# Fix auto-fixable issues
npm run lint:fix

# Format code
npm run format
```

## Deployment

See [deployment guide](docs/deployment.md) for detailed instructions.

### Quick Deploy to Heroku

```bash
heroku create
git push heroku main
heroku run npm run migrate
```

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for:
- Code of conduct
- Development process
- How to submit pull requests
- Coding standards

## Troubleshooting

### Common Issues

**Issue: Database connection fails**
```
Error: connect ECONNREFUSED 127.0.0.1:5432
```
Solution: Ensure PostgreSQL is running and DATABASE_URL is correct.

**Issue: Port already in use**
```
Error: listen EADDRINUSE: address already in use :::3000
```
Solution: Change PORT in .env or stop the process using port 3000.

See [FAQ](docs/faq.md) for more troubleshooting.

## License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file.

## Acknowledgments

- Library/framework used
- Contributors
- Inspiration sources

## Contact

- **Issues**: [GitHub Issues](https://github.com/username/project/issues)
- **Discussions**: [GitHub Discussions](https://github.com/username/project/discussions)
- **Email**: support@example.com

---

**Maintained by**: [Your Team/Organization](https://example.com)
```

## API Documentation

### REST API Documentation

```markdown
# API Documentation

## Base URL

```
Production: https://api.example.com/v1
Staging: https://staging-api.example.com/v1
```

## Authentication

All API requests require authentication using JWT tokens.

```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

### Get Access Token

```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiresIn": 3600
}
```

## Endpoints

### Users

#### Get User Profile

```http
GET /users/:id
```

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| id | string | Yes | User ID |

**Headers:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| Authorization | string | Yes | Bearer token |

**Response:**

```json
{
  "id": "123",
  "email": "user@example.com",
  "name": "John Doe",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

**Error Responses:**

| Status | Description |
|--------|-------------|
| 401 | Unauthorized - Invalid or missing token |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - User does not exist |

#### Update User Profile

```http
PATCH /users/:id
Content-Type: application/json

{
  "name": "Jane Doe",
  "bio": "Software developer"
}
```

**Request Body:**

| Field | Type | Required | Description | Constraints |
|-------|------|----------|-------------|-------------|
| name | string | No | Display name | Max 100 chars |
| bio | string | No | User biography | Max 500 chars |
| avatar | string | No | Avatar URL | Valid URL |

**Response:**

```json
{
  "id": "123",
  "name": "Jane Doe",
  "bio": "Software developer",
  "updatedAt": "2024-01-15T11:00:00Z"
}
```

### Rate Limiting

- **Rate Limit**: 1000 requests per hour
- **Headers**: 
  - `X-RateLimit-Limit`: Maximum requests
  - `X-RateLimit-Remaining`: Remaining requests
  - `X-RateLimit-Reset`: Reset timestamp

**Rate Limit Exceeded Response:**

```json
{
  "error": "rate_limit_exceeded",
  "message": "Too many requests. Please try again later.",
  "retryAfter": 3600
}
```

## Error Handling

All errors follow this format:

```json
{
  "error": "error_code",
  "message": "Human-readable error message",
  "details": {
    "field": "Additional error details"
  }
}
```

### Common Error Codes

| Code | Status | Description |
|------|--------|-------------|
| `invalid_request` | 400 | Request validation failed |
| `unauthorized` | 401 | Authentication required |
| `forbidden` | 403 | Insufficient permissions |
| `not_found` | 404 | Resource not found |
| `rate_limit_exceeded` | 429 | Too many requests |
| `server_error` | 500 | Internal server error |

## Examples

### cURL

```bash
curl -X GET https://api.example.com/v1/users/123 \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.example.com/v1/users/123', {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json'
  }
});

const user = await response.json();
```

### Python (requests)

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json'
}

response = requests.get(
    'https://api.example.com/v1/users/123',
    headers=headers
)

user = response.json()
```
```

### OpenAPI/Swagger

```yaml
openapi: 3.0.0
info:
  title: Example API
  version: 1.0.0
  description: API for managing users and resources

servers:
  - url: https://api.example.com/v1
    description: Production
  - url: https://staging-api.example.com/v1
    description: Staging

paths:
  /users/{id}:
    get:
      summary: Get user by ID
      operationId: getUser
      tags:
        - Users
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
          description: User ID
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          description: User not found

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
          example: "123"
        email:
          type: string
          format: email
          example: "user@example.com"
        name:
          type: string
          example: "John Doe"
        createdAt:
          type: string
          format: date-time
  
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - bearerAuth: []
```

## Keeping Documentation Up-to-Date

### Documentation as Code

```markdown
1. Store documentation in version control
2. Review docs in pull requests
3. Update docs with code changes
4. Automate documentation generation where possible
```

### Documentation Review Checklist

```markdown
**When reviewing PRs, verify:**

- [ ] README updated if API changes
- [ ] JSDoc/docstrings added for new functions
- [ ] Changelog updated with changes
- [ ] Migration guide for breaking changes
- [ ] Examples updated or added
- [ ] Architecture docs reflect new design
```

### Automated Documentation

```javascript
// ✅ Generate API docs from code
/**
 * @swagger
 * /users/{id}:
 *   get:
 *     summary: Get user by ID
 *     parameters:
 *       - name: id
 *         in: path
 *         required: true
 *         schema:
 *           type: string
 *     responses:
 *       200:
 *         description: User object
 */
app.get('/users/:id', getUser);
```

```python
# ✅ Generate docs from type hints and docstrings
from typing import Optional
from pydantic import BaseModel

class User(BaseModel):
    """User model with validation."""
    id: str
    email: str
    name: Optional[str] = None

# Automatically generates OpenAPI schema
```

### Documentation CI/CD

```yaml
# .github/workflows/docs.yml
name: Documentation

on:
  push:
    branches: [main]
  pull_request:

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Check for broken links
        uses: gaurav-nelson/github-action-markdown-link-check@v1
      
      - name: Validate OpenAPI spec
        run: |
          npm install -g @apidevtools/swagger-cli
          swagger-cli validate docs/openapi.yaml
      
      - name: Build documentation
        run: |
          npm run docs:build
      
      - name: Deploy to GitHub Pages
        if: github.ref == 'refs/heads/main'
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./docs-build
```

## Architecture Documentation

### Architecture Decision Records (ADRs)

```markdown
# ADR-001: Use PostgreSQL for Primary Database

## Status
Accepted

## Context
We need a reliable, scalable database for our application data.

## Decision
Use PostgreSQL as our primary database.

## Consequences

### Positive
- ACID compliance ensures data integrity
- Rich feature set (JSON, full-text search, etc.)
- Excellent documentation and community support
- Strong ecosystem of tools and libraries

### Negative
- Higher learning curve than simpler databases
- Requires more operational expertise
- Potentially overkill for simple use cases

## Alternatives Considered
- MongoDB: Rejected due to need for strong consistency
- MySQL: Similar to PostgreSQL but fewer features
- SQLite: Too limited for our scale

## References
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Performance Benchmarks](link-to-benchmarks)
```

### System Architecture Diagram

```markdown
# System Architecture

## High-Level Overview

```
┌─────────────┐
│   Client    │
│ (Web/Mobile)│
└──────┬──────┘
       │ HTTPS
       ▼
┌─────────────┐     ┌──────────────┐
│   CDN       │────▶│ Static Files │
│ (CloudFront)│     └──────────────┘
└──────┬──────┘
       │
       ▼
┌─────────────────┐
│  Load Balancer  │
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌─────────┐ ┌─────────┐
│  API 1  │ │  API 2  │
└────┬────┘ └────┬────┘
     │           │
     └─────┬─────┘
           ▼
    ┌──────────────┐
    │  PostgreSQL  │
    │   (Primary)  │
    └──────┬───────┘
           │ Replication
           ▼
    ┌──────────────┐
    │  PostgreSQL  │
    │   (Replica)  │
    └──────────────┘
```

## Component Descriptions

### API Server
- **Technology**: Node.js + Express
- **Responsibilities**: Business logic, data validation
- **Scaling**: Horizontal (multiple instances)

### Database
- **Technology**: PostgreSQL 14
- **High Availability**: Primary-replica setup
- **Backup**: Daily automated backups to S3

### Cache Layer
- **Technology**: Redis
- **Purpose**: Session storage, API response caching
- **Persistence**: RDB + AOF
```

## Documentation Tools

### Static Site Generators

**GitBook**: Documentation platform
**Docusaurus**: React-based documentation
**MkDocs**: Python-based, Markdown
**VitePress**: Vue-powered static site generator

### API Documentation Tools

**Swagger/OpenAPI**: REST API specification
**Postman**: API development and documentation
**API Blueprint**: API design and documentation
**GraphQL Playground**: GraphQL API exploration

### Diagram Tools

**Mermaid**: Text-based diagrams in Markdown
**PlantUML**: UML diagrams from text
**Draw.io**: Visual diagram editor
**Lucidchart**: Collaborative diagramming

### Example: Mermaid Diagram

```markdown
```mermaid
sequenceDiagram
    participant User
    participant API
    participant DB
    
    User->>API: POST /login
    API->>DB: Verify credentials
    DB-->>API: User data
    API-->>User: JWT token
    
    User->>API: GET /profile
    Note over API: Verify JWT
    API->>DB: Fetch profile
    DB-->>API: Profile data
    API-->>User: Profile response
```
```

## Documentation Checklist

### Project Setup
- [ ] README with quick start guide
- [ ] CONTRIBUTING guidelines
- [ ] LICENSE file
- [ ] CODE_OF_CONDUCT
- [ ] CHANGELOG
- [ ] .env.example with all variables

### Code Documentation
- [ ] JSDoc/docstrings for all public APIs
- [ ] Inline comments for complex logic
- [ ] Type definitions (TypeScript, Flow, etc.)
- [ ] Example usage in comments

### API Documentation
- [ ] OpenAPI/Swagger specification
- [ ] Authentication guide
- [ ] Error handling documentation
- [ ] Rate limiting information
- [ ] Example requests/responses

### Deployment
- [ ] Deployment guide
- [ ] Environment configuration
- [ ] Troubleshooting guide
- [ ] Rollback procedures
- [ ] Monitoring and alerting setup

### Maintenance
- [ ] Regular documentation reviews
- [ ] Version compatibility matrix
- [ ] Migration guides for breaking changes
- [ ] Deprecation notices

## Resources

### Writing Guides
- [Google Developer Documentation Style Guide](https://developers.google.com/style)
- [Microsoft Writing Style Guide](https://docs.microsoft.com/en-us/style-guide/)
- [Write the Docs Community](https://www.writethedocs.org/)

### Documentation Tools
- [GitBook](https://www.gitbook.com/)
- [Docusaurus](https://docusaurus.io/)
- [Swagger](https://swagger.io/)
- [Read the Docs](https://readthedocs.org/)

### Best Practices
- [Documentation Guide by Write the Docs](https://www.writethedocs.org/guide/)
- [Divio Documentation System](https://documentation.divio.com/)

---

**Remember**: Documentation is a continuous process. Keep it updated, keep it clear, and keep it accessible. Good documentation is an investment in your project's future.
