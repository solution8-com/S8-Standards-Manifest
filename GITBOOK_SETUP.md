# GitBook Setup Guide

This guide explains how to publish this documentation to GitBook.

## Prerequisites

- A GitBook account (free or paid plan)
- GitHub repository with documentation
- Admin access to the GitHub repository

## Quick Start

### Option 1: GitBook GitHub Integration (Recommended)

1. **Sign in to GitBook**
   - Go to [gitbook.com](https://www.gitbook.com)
   - Sign in or create an account

2. **Create a New Space**
   - Click "New Space"
   - Choose "Import from GitHub"

3. **Connect GitHub Repository**
   - Select this repository: `solution8-com/S8-Standards-Manifest`
   - Choose the branch (e.g., `main`)
   - GitBook will automatically detect the `.gitbook.yaml` configuration

4. **Configure Sync**
   - Enable "GitHub Sync" for automatic updates
   - Set sync direction (GitHub → GitBook recommended)

5. **Publish**
   - Click "Publish" to make your documentation live
   - Share the URL with your team

### Option 2: Manual Setup

If you prefer not to use GitHub integration:

1. **Create a Space**
   - Create a new space in GitBook
   - Choose "Start from scratch"

2. **Import Content**
   - Use GitBook's import feature
   - Upload your markdown files
   - Manually configure the structure

3. **Maintain Sync**
   - Update content in GitBook editor
   - Or export and re-import when needed

## Configuration

### .gitbook.yaml

The `.gitbook.yaml` file configures GitBook settings:

```yaml
root: ./

structure:
  readme: README.md
  summary: SUMMARY.md
```

**Options:**
- `root`: Root directory for documentation (`./ = repository root`)
- `structure.readme`: Main introduction page
- `structure.summary`: Table of contents

### SUMMARY.md

The `SUMMARY.md` file defines the navigation structure:

```markdown
# Table of Contents

* [Introduction](README.md)

## Section Name

* [Page Title](path/to/page.md)
```

**Guidelines:**
- Use `##` for main sections
- Use `*` for individual pages
- Paths are relative to repository root
- Order in SUMMARY.md determines navigation order

## Publishing Workflow

### Automatic Publishing

With GitHub Sync enabled:

1. **Make Changes** - Edit files in your repository
2. **Commit & Push** - Push changes to GitHub
3. **Auto-Sync** - GitBook automatically syncs and publishes
4. **Live Updates** - Changes appear in GitBook within minutes

### Manual Publishing

Without GitHub Sync:

1. **Make Changes** - Edit files locally or in GitHub
2. **Export Content** - Export from repository
3. **Import to GitBook** - Upload to GitBook
4. **Publish** - Manually publish updates

## Customization

### Branding

In GitBook settings, customize:

- **Logo**: Upload company logo
- **Favicon**: Set custom favicon
- **Colors**: Choose brand colors
- **Domain**: Use custom domain (paid plans)

### Features

Enable/disable features:

- **Search**: Full-text search
- **Integrations**: Slack, GitHub, Analytics
- **Page Insights**: Track page views
- **PDF Export**: Allow PDF downloads

### Access Control

Configure who can access:

- **Public**: Anyone with the link
- **Members Only**: Require GitBook login
- **Private**: Specific team members only
- **SSO**: Single sign-on (Enterprise)

## Navigation Structure

Our documentation is organized into these main sections:

1. **Coding Standards** - Language-specific standards and guidelines
2. **Development Methods** - Agile, workflows, testing, CI/CD
3. **Certifications & Compliance** - Security, ISO, audits
4. **Team & Culture** - Values, communication, onboarding
5. **Tools & Technologies** - Development tools, infrastructure
6. **Best Practices** - Security, performance, accessibility

## Maintenance

### Keeping Documentation Updated

1. **Regular Reviews** - Review content quarterly
2. **Version Updates** - Update when tools/versions change
3. **Link Checks** - Verify all links work
4. **Feedback Loop** - Act on user feedback

### Content Updates

When updating content:

1. **Edit Files** - Update markdown files in repository
2. **Test Locally** - Preview changes locally
3. **Commit** - Commit with clear message
4. **Push** - Push to GitHub
5. **Verify** - Check GitBook updates correctly

## Advanced Features

### Custom Domain

To use a custom domain (e.g., docs.solution8.com):

1. Go to Space Settings
2. Click "Custom Domain"
3. Enter your domain
4. Configure DNS records as instructed
5. Verify domain ownership

### Integrations

#### GitHub Integration
- Enables automatic sync
- Two-way sync available
- Branch selection
- Pull request previews

#### Slack Integration
- Notifications for updates
- Search from Slack
- Share pages to channels

#### Analytics
- Google Analytics
- Segment
- Custom analytics

### PDF Export

Enable PDF export to allow users to download documentation:

1. Go to Space Settings
2. Enable "PDF Export"
3. Customize PDF settings
4. Users can download via export button

## Troubleshooting

### Sync Issues

**Problem**: Changes not appearing in GitBook

**Solutions:**
- Check GitHub sync status
- Manually trigger sync
- Verify branch is correct
- Check for syntax errors in markdown

### Broken Links

**Problem**: Links not working

**Solutions:**
- Use relative paths (not absolute)
- Check file paths are correct
- Verify file names match exactly
- Use lowercase for consistency

### Formatting Issues

**Problem**: Content not displaying correctly

**Solutions:**
- Validate markdown syntax
- Check code block languages
- Verify heading hierarchy
- Test with GitBook markdown editor

## Best Practices

### Structure
- Keep files organized in logical directories
- Use clear, descriptive file names
- Maintain consistent heading levels
- Update SUMMARY.md when adding pages

### Content
- Write clear, concise content
- Use examples liberally
- Include code snippets with syntax highlighting
- Link to related pages

### Maintenance
- Review content regularly
- Remove outdated information
- Keep examples up-to-date
- Monitor user feedback

### Collaboration
- Use pull requests for changes
- Review changes before merging
- Document major updates in changelog
- Communicate updates to team

## Resources

### GitBook Documentation
- [GitBook Docs](https://docs.gitbook.com)
- [GitBook GitHub Integration](https://docs.gitbook.com/integrations/git-sync)
- [Markdown Guide](https://docs.gitbook.com/content-editor/markdown)

### Our Documentation
- [CONTRIBUTING.md](CONTRIBUTING.md) - How to contribute
- [SUMMARY.md](SUMMARY.md) - Table of contents
- [README.md](README.md) - Introduction

## Support

For help with GitBook:
- [GitBook Support](https://www.gitbook.com/support)
- [GitBook Community](https://github.com/GitbookIO/gitbook/discussions)
- Internal documentation team

---

**Last Updated**: February 2026  
**Maintained By**: Solution8 Documentation Team
