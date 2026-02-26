# Development Tools

## Overview

Development tools are the foundation of daily software engineering work. The right tools enhance productivity, code quality, and developer satisfaction. This guide covers essential tools for modern software development.

## IDEs and Code Editors

### Visual Studio Code

**Recommended for:** Web development, TypeScript, Python, Go, multi-language projects

**Why We Use It:**
- Lightweight yet powerful
- Excellent extension ecosystem
- Built-in Git integration
- Remote development capabilities
- Free and open-source

**Essential Extensions:**
- **GitLens**: Enhanced Git integration
- **ESLint/Prettier**: Code formatting and linting
- **Remote - SSH**: Remote development
- **Docker**: Container management
- **Thunder Client**: API testing
- **Error Lens**: Inline error display
- **Path Intellisense**: File path autocomplete
- **Live Share**: Real-time collaboration

**Recommended Settings:**
```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.autoSave": "onFocusChange",
  "editor.rulers": [80, 120],
  "editor.tabSize": 2,
  "files.trimTrailingWhitespace": true,
  "git.autofetch": true
}
```

### JetBrains IDEs

**IntelliJ IDEA** - Java, Kotlin, Scala
**PyCharm** - Python
**WebStorm** - JavaScript, TypeScript
**GoLand** - Go
**Rider** - .NET, C#

**Why We Use Them:**
- Superior code intelligence and refactoring
- Built-in database tools
- Excellent debugging capabilities
- Strong framework integration
- Professional support available

**Best Practices:**
- Use shared code style configurations
- Enable save actions (format, optimize imports)
- Configure pre-commit checks
- Use Live Templates for common patterns
- Enable EditorConfig support

### Vim/Neovim

**Recommended for:** Terminal-based development, remote work, power users

**Why Some Use It:**
- Exceptional performance
- Powerful keyboard-driven workflow
- Available everywhere
- Highly customizable
- No mouse dependency

**Modern Neovim Setup:**
- **LSP Integration**: Language server support
- **Treesitter**: Advanced syntax highlighting
- **Telescope**: Fuzzy finder
- **nvim-cmp**: Autocompletion
- **vim-fugitive**: Git integration

## Version Control Tools

### Git

**Industry Standard Version Control**

**Essential Configuration:**
```bash
# User configuration
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"

# Editor and diff tools
git config --global core.editor "code --wait"
git config --global merge.tool vscode

# Useful aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

**Best Practices:**
- Write meaningful commit messages
- Commit early and often
- Use branches for features and fixes
- Keep commits atomic and focused
- Sign commits with GPG keys
- Use `.gitignore` effectively

### GitHub/GitLab/Bitbucket

**Git Hosting Platforms**

**Key Features to Use:**
- **Pull/Merge Requests**: Code review workflow
- **Issues**: Bug tracking and task management
- **Actions/Pipelines**: CI/CD automation
- **Code Owners**: Automatic reviewer assignment
- **Branch Protection**: Enforce quality gates
- **Wiki**: Project documentation

### Git GUI Tools

**Recommended Tools:**
- **GitKraken**: Visual Git client with intuitive interface
- **Sourcetree**: Free Git GUI from Atlassian
- **GitHub Desktop**: Simple Git workflow for GitHub
- **Fork**: Fast and friendly Git client
- **Tower**: Professional Git client (Mac/Windows)

## Local Development Setup

### Package Managers

**macOS:**
```bash
# Homebrew - Essential package manager
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install common tools
brew install git node python go docker
```

**Linux (Ubuntu/Debian):**
```bash
# APT for system packages
sudo apt update && sudo apt upgrade

# For development tools
sudo apt install build-essential git curl
```

**Windows:**
```powershell
# Chocolatey or Scoop
Set-ExecutionPolicy Bypass -Scope Process -Force
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# Install tools
choco install git nodejs python docker-desktop vscode
```

### Version Managers

**Node.js - nvm (Node Version Manager):**
```bash
# Install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Use specific version
nvm install 18
nvm use 18
nvm alias default 18
```

**Python - pyenv:**
```bash
# Install pyenv
curl https://pyenv.run | bash

# Install and use Python version
pyenv install 3.11.0
pyenv global 3.11.0
```

**Ruby - rbenv:**
```bash
# Install rbenv
brew install rbenv

# Install Ruby version
rbenv install 3.2.0
rbenv global 3.2.0
```

**Java - SDKMAN:**
```bash
# Install SDKMAN
curl -s "https://get.sdkman.io" | bash

# Install Java version
sdk install java 17.0.5-tem
sdk use java 17.0.5-tem
```

### Docker Desktop

**Why We Use It:**
- Consistent development environments
- Easy service dependencies (databases, caching)
- Matches production environment
- Isolated environments per project

**Essential Setup:**
```yaml
# docker-compose.yml for local development
version: '3.8'
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: dev_password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
  
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
  
  mailhog:
    image: mailhog/mailhog
    ports:
      - "1025:1025"
      - "8025:8025"

volumes:
  postgres_data:
```

### Development Databases

**Local Database Tools:**
- **PostgreSQL**: `brew install postgresql@15`
- **MySQL**: `brew install mysql`
- **MongoDB**: `brew install mongodb-community`
- **Redis**: `brew install redis`

**Database Clients:**
- **DBeaver**: Universal database tool (free)
- **TablePlus**: Modern database GUI (Mac/Windows)
- **DataGrip**: JetBrains database IDE
- **Postico**: PostgreSQL client (Mac)
- **MongoDB Compass**: MongoDB GUI

## Browser Developer Tools

### Chrome DevTools

**Essential Features:**
- **Elements**: Inspect and modify DOM/CSS
- **Console**: JavaScript debugging and logging
- **Network**: Monitor requests and responses
- **Performance**: Profile runtime performance
- **Application**: Inspect storage, service workers
- **Lighthouse**: Audit performance and best practices

**Useful Extensions:**
- **React Developer Tools**: Debug React applications
- **Vue.js devtools**: Debug Vue applications
- **Redux DevTools**: Debug Redux state
- **JSON Viewer**: Format and inspect JSON
- **Wappalyzer**: Identify technologies on websites

### Firefox Developer Tools

**Unique Features:**
- CSS Grid Inspector
- Font editor
- Accessibility inspector
- Screenshot full page

### Safari Web Inspector

**For macOS/iOS Development:**
- iOS device debugging
- Responsive design mode
- Resource loading timeline

## Testing Tools

### Unit Testing Frameworks

**JavaScript/TypeScript:**
- **Jest**: Full-featured testing framework
- **Vitest**: Fast Vite-native test framework
- **Mocha + Chai**: Flexible testing combination

**Python:**
- **pytest**: Modern Python testing
- **unittest**: Built-in testing framework

**Java:**
- **JUnit 5**: Standard Java testing
- **TestNG**: Alternative testing framework

**Go:**
- **testing**: Built-in Go testing package
- **testify**: Assertion and mocking library

### Integration Testing

**Tools:**
- **Supertest**: HTTP assertion library (Node.js)
- **RestAssured**: API testing (Java)
- **Requests**: HTTP library for testing (Python)

### End-to-End Testing

**Modern E2E Frameworks:**

**Playwright:**
```javascript
// Recommended for new projects
import { test, expect } from '@playwright/test';

test('homepage has title', async ({ page }) => {
  await page.goto('https://example.com');
  await expect(page).toHaveTitle(/Example/);
});
```

**Cypress:**
- Great developer experience
- Real-time test running
- Time travel debugging

**Selenium:**
- Multi-language support
- Browser automation
- Mobile testing with Appium

### Testing Utilities

- **Faker.js**: Generate realistic test data
- **Mock Service Worker**: API mocking
- **Testcontainers**: Docker containers for integration tests
- **Storybook**: Component development and testing
- **Chromatic**: Visual regression testing

## Code Quality Tools

### Linters

**JavaScript/TypeScript:**
```json
// .eslintrc.json
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "prettier"
  ],
  "rules": {
    "no-console": "warn",
    "no-unused-vars": "error"
  }
}
```

**Python:**
```toml
# pyproject.toml
[tool.ruff]
line-length = 88
select = ["E", "F", "I", "N", "W"]
ignore = ["E501"]
```

**Other Languages:**
- **ESLint**: JavaScript/TypeScript
- **Pylint/Ruff**: Python
- **RuboCop**: Ruby
- **Checkstyle**: Java
- **golangci-lint**: Go

### Code Formatters

**Prettier (JavaScript/TypeScript):**
```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100
}
```

**Other Formatters:**
- **Black**: Python formatter
- **gofmt**: Go formatter
- **rustfmt**: Rust formatter
- **Rubocop**: Ruby formatter

### Static Analysis

**SonarQube/SonarCloud:**
- Code smell detection
- Security vulnerability scanning
- Code coverage tracking
- Technical debt measurement

**Language-Specific:**
- **TypeScript**: Built-in type checking
- **mypy**: Python static type checker
- **Flow**: JavaScript type checker
- **SpotBugs**: Java bug detection

### Pre-commit Hooks

**Husky (JavaScript):**
```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  }
}
```

**pre-commit (Python):**
```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
```

### Code Review Tools

- **GitHub Pull Requests**: Built-in code review
- **Gerrit**: Code review for Git
- **Crucible**: Atlassian code review
- **Review Board**: Open-source code review

## API Development Tools

### API Clients

**Postman:**
- Visual API testing
- Collection management
- Environment variables
- Automated testing

**Insomnia:**
- Clean interface
- GraphQL support
- Environment management

**HTTPie:**
- Command-line HTTP client
- Intuitive syntax
- JSON support

**curl:**
- Universal HTTP client
- Scriptable
- Available everywhere

### API Mocking

- **Mock Service Worker**: Browser/Node.js mocking
- **WireMock**: Standalone mock server
- **Postman Mock Server**: Cloud-based mocking
- **JSON Server**: Quick REST API from JSON

## Terminal and Shell

### Modern Terminals

**iTerm2 (macOS):**
- Split panes
- Search and autocomplete
- Customizable profiles
- tmux integration

**Windows Terminal:**
- Multiple tabs
- GPU-accelerated
- Unicode support
- PowerShell/WSL integration

**Alacritty:**
- GPU-accelerated
- Cross-platform
- Minimal and fast

### Shell Enhancement

**Oh My Zsh:**
```bash
# Install
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Useful plugins
plugins=(git docker kubectl node npm)
```

**Starship:**
- Cross-shell prompt
- Fast and minimal
- Git integration
- Language version display

### CLI Productivity Tools

- **fzf**: Fuzzy file finder
- **ripgrep**: Fast text search
- **bat**: Better `cat` with syntax highlighting
- **exa**: Modern `ls` replacement
- **httpie**: User-friendly curl alternative
- **jq**: JSON processor
- **tldr**: Simplified man pages

## Best Practices

### Development Environment

1. **Keep It Automated**: Use scripts to set up development environment
2. **Document Setup**: Maintain clear README with setup instructions
3. **Version Everything**: Pin tool versions in configuration
4. **Use Docker**: Containerize dependencies when possible
5. **Backup Configurations**: Version control dotfiles

### Tool Selection

1. **Team Alignment**: Agree on core tools to reduce friction
2. **Personal Choice**: Allow flexibility for editors and terminal
3. **Regular Updates**: Keep tools and extensions up-to-date
4. **Security First**: Vet tools for security issues
5. **Trial Period**: Evaluate new tools before team-wide adoption

### Performance

1. **Minimize Extensions**: Only install necessary IDE extensions
2. **Exclude Directories**: Configure IDE to ignore `node_modules`, `.git`
3. **Increase Resources**: Allocate sufficient memory to IDEs
4. **Clean Caches**: Regularly clear build caches
5. **Use SSD**: Development on solid-state drives

### Security

1. **Don't Commit Secrets**: Use `.env` files in `.gitignore`
2. **Use SSH Keys**: Configure SSH for Git authentication
3. **Enable 2FA**: Secure all developer accounts
4. **Review Extensions**: Audit IDE extensions for security
5. **Update Regularly**: Keep all tools patched

---

**Related:**
- [Infrastructure](infrastructure.md) - Deployment and platform tools
- [Monitoring](monitoring.md) - Observability and debugging
- [Documentation](documentation.md) - Knowledge management tools
