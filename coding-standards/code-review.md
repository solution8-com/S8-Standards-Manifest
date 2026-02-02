# Code Review Guidelines

## Overview

Code reviews are a critical part of our development process. They ensure code quality, share knowledge, and maintain consistency across our codebase.

## Code Review Process

### 1. Before Submitting for Review

**Checklist for Authors:**
- [ ] Code compiles without errors or warnings
- [ ] All tests pass locally
- [ ] New tests added for new functionality
- [ ] Code follows our coding standards
- [ ] Self-review completed
- [ ] Documentation updated if necessary
- [ ] No debugging code or commented-out code
- [ ] Secrets and credentials removed

### 2. Creating a Pull Request

**PR Description Should Include:**
- Clear title describing the change
- Summary of what changed and why
- Link to related issue/ticket
- Screenshots for UI changes
- Testing instructions
- Breaking changes (if any)

**Example PR Template:**
```markdown
## Description
Brief description of the changes

## Related Issue
Fixes #123

## Changes Made
- Added user authentication
- Updated API endpoints
- Modified database schema

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests pass
- [ ] Manual testing completed

## Screenshots (if applicable)
[Add screenshots here]

## Breaking Changes
None
```

### 3. Review Process

**Reviewers Should:**
- Review within 24 hours
- Test the changes locally if necessary
- Provide constructive feedback
- Approve or request changes with clear explanations

**Review Checklist:**
- [ ] Code is readable and maintainable
- [ ] Logic is correct and efficient
- [ ] Error handling is appropriate
- [ ] Security concerns addressed
- [ ] Tests are comprehensive
- [ ] Documentation is clear
- [ ] No performance issues
- [ ] Follows coding standards

### 4. Addressing Feedback

**Authors Should:**
- Respond to all comments
- Make requested changes or explain why not
- Request re-review after changes
- Keep discussions professional and constructive

## What to Look For

### Code Quality

**Readability**
- Clear variable and function names
- Appropriate comments for complex logic
- Consistent formatting
- Logical code organization

**Maintainability**
- Single Responsibility Principle
- DRY (Don't Repeat Yourself)
- SOLID principles followed
- Proper abstraction levels

**Correctness**
- Logic is sound
- Edge cases handled
- No obvious bugs
- Proper error handling

### Security

- No hardcoded credentials
- Input validation
- SQL injection prevention
- XSS prevention
- Proper authentication/authorization
- Sensitive data encrypted

### Performance

- No unnecessary database queries
- Efficient algorithms
- Proper caching where appropriate
- No memory leaks
- Appropriate use of async/await

### Testing

- Adequate test coverage (80%+ target)
- Tests are meaningful and clear
- Edge cases covered
- Integration tests where appropriate
- Tests are maintainable

## Review Comments

### Writing Good Comments

**Be Specific and Constructive:**
```
❌ Bad: "This is wrong"
✅ Good: "This could cause a null pointer exception. Consider adding a null check before accessing user.email"

❌ Bad: "Rewrite this"
✅ Good: "This function is doing too many things. Consider extracting the validation logic into a separate function for better readability"
```

**Use Question Format for Suggestions:**
```
"Have you considered using a switch statement here instead of multiple if-else? It might be more readable."

"What do you think about moving this logic to a service class? It would make this controller cleaner."
```

**Recognize Good Work:**
```
"Nice refactoring! This is much more readable."
"Great test coverage on this feature!"
"Clever solution to handle edge cases."
```

### Comment Categories

Use these prefixes to categorize comments:

- **[MUST]**: Critical issue that must be fixed
- **[SHOULD]**: Important suggestion that should be addressed
- **[CONSIDER]**: Optional suggestion for improvement
- **[QUESTION]**: Clarification needed
- **[PRAISE]**: Positive feedback

**Examples:**
```
[MUST] Add input validation to prevent SQL injection
[SHOULD] Consider extracting this into a separate method
[CONSIDER] Could we use a more descriptive variable name?
[QUESTION] Why did we choose this approach over...?
[PRAISE] Excellent error handling here!
```

## Best Practices for Authors

### Keep PRs Small
- Aim for PRs under 400 lines of code
- Break large features into smaller, reviewable chunks
- One logical change per PR

### Provide Context
- Write clear commit messages
- Explain the "why" not just the "what"
- Link to relevant documentation or discussions

### Respond Promptly
- Address review comments quickly
- Clarify misunderstandings
- Request help if needed

### Be Open to Feedback
- View reviews as learning opportunities
- Don't take criticism personally
- Ask questions to understand suggestions

## Best Practices for Reviewers

### Be Timely
- Review within 24 hours
- Block time for reviews daily
- Prioritize urgent reviews

### Be Thorough but Practical
- Focus on important issues
- Don't nitpick style if linters handle it
- Balance perfectionism with progress

### Be Kind and Constructive
- Assume positive intent
- Explain the reasoning behind feedback
- Offer solutions, not just problems

### Test When Necessary
- Pull and test complex changes
- Verify bug fixes actually fix the issue
- Check edge cases

## Common Issues to Watch For

### Security Issues
- Exposed secrets or API keys
- Missing input validation
- Insecure direct object references
- Missing authentication checks
- Vulnerable dependencies

### Performance Issues
- N+1 query problems
- Unnecessary database calls
- Large data loaded into memory
- Missing indexes
- Inefficient algorithms

### Logic Issues
- Race conditions
- Off-by-one errors
- Null pointer exceptions
- Division by zero
- Incorrect comparisons

### Design Issues
- Tight coupling
- God objects/classes
- Lack of abstraction
- Code duplication
- Violation of SOLID principles

## Automation

### Automated Checks
Before human review, ensure these automated checks pass:
- Linting
- Unit tests
- Integration tests
- Code coverage threshold
- Security scanning
- Build verification

### CI/CD Integration
- All checks must pass before merge
- Branch protection rules enforced
- Require at least one approval
- Up-to-date with base branch

## Merge Criteria

A PR can be merged when:
- [ ] All automated checks pass
- [ ] At least one approval from a team member
- [ ] All review comments addressed or resolved
- [ ] Documentation updated
- [ ] No merge conflicts
- [ ] Author has responded to all feedback

## After Merge

### Monitoring
- Watch for issues in production
- Monitor error logs
- Check performance metrics

### Follow-up
- Create issues for future improvements
- Document lessons learned
- Update standards if needed

## Resources

- [Google's Code Review Guidelines](https://google.github.io/eng-practices/review/)
- [Microsoft's Code Review Guide](https://docs.microsoft.com/en-us/azure/devops/repos/git/pull-requests)
- Our internal coding standards (this documentation)
