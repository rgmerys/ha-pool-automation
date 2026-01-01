# GitHub Issue Templates

Create these files in `.github/ISSUE_TEMPLATE/` directory:

## 1. Bug Report Template

**File:** `.github/ISSUE_TEMPLATE/bug_report.md`

```markdown
---
name: Bug Report
about: Report a bug or issue
title: '[BUG] '
labels: bug
assignees: ''
---

## Bug Description
<!-- A clear and concise description of the bug -->

## Home Assistant Environment
- **Home Assistant Version:** 
- **Installation Type:** (OS/Supervised/Container/Core)
- **Python Version:** 
- **Browser (if UI issue):** 

## Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. See error

## Expected Behavior
<!-- What you expected to happen -->

## Actual Behavior
<!-- What actually happened -->

## Configuration
<!-- Paste relevant configuration (REMOVE PASSWORDS/TOKENS) -->
```yaml
# Your configuration here
```

## Logs
<!-- Paste relevant logs from Settings → System → Logs -->
```
Your logs here
```

## Screenshots
<!-- If applicable, add screenshots -->

## Additional Context
<!-- Add any other context about the problem -->

## Checklist
- [ ] I have checked existing issues
- [ ] I have tested on latest version
- [ ] I have included relevant logs
- [ ] I have sanitized sensitive data
```
