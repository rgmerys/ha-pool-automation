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

---

## 2. Feature Request Template

**File:** `.github/ISSUE_TEMPLATE/feature_request.md`

```markdown
---
name: Feature Request
about: Suggest a new feature or enhancement
title: '[FEATURE] '
labels: enhancement
assignees: ''
---

## Feature Description
<!-- Clear description of the proposed feature -->

## Problem it Solves
<!-- What problem does this feature solve? -->

## Use Case
<!-- When and how would you use this feature? -->

## Proposed Implementation
<!-- How could this work? (optional) -->

## Alternatives Considered
<!-- Other ways to achieve the same goal (optional) -->

## Additional Context
<!-- Add any other context, mockups, or examples -->

## Benefits
<!-- Why is this useful for the community? -->

## Checklist
- [ ] I have checked existing feature requests
- [ ] This feature would benefit most users
- [ ] I can help test this feature
```

---

## 3. Question Template

**File:** `.github/ISSUE_TEMPLATE/question.md`

```markdown
---
name: Question
about: Ask a question about the project
title: '[QUESTION] '
labels: question
assignees: ''
---

## Question
<!-- Your question here -->

## What I've Tried
<!-- Steps you've already taken to find the answer -->

## Context
<!-- Any relevant context about your setup -->

## Home Assistant Version
<!-- Your Home Assistant version -->

## Additional Information
<!-- Any other details that might help -->
```

---

## 4. Pull Request Template

**File:** `.github/PULL_REQUEST_TEMPLATE.md`

```markdown
## Description
<!-- Brief description of your changes -->

## Related Issues
<!-- Reference related issues: Fixes #123, Closes #456 -->

## Type of Change
- [ ] Bug fix (non-breaking change that fixes an issue)
- [ ] New feature (non-breaking change that adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update
- [ ] Code refactoring
- [ ] Performance improvement

## Testing Done
- [ ] Tested on Home Assistant version: ___
- [ ] All automations work correctly
- [ ] No errors in Home Assistant logs
- [ ] Dashboard displays correctly
- [ ] All three operating modes tested
- [ ] Season detection verified
- [ ] Manual override tested
- [ ] Energy monitoring confirmed (if applicable)

## Screenshots
<!-- If applicable, add screenshots to demonstrate changes -->

## Documentation
- [ ] README.md updated
- [ ] TROUBLESHOOTING.md updated (if needed)
- [ ] Code comments added for complex logic
- [ ] Changelog updated

## Checklist
- [ ] My code follows the project's style guidelines
- [ ] I have performed a self-review of my code
- [ ] I have commented my code in complex areas
- [ ] I have made corresponding changes to documentation
- [ ] My changes generate no new warnings or errors
- [ ] I have tested in a clean Home Assistant installation
- [ ] All new and existing tests pass

## Breaking Changes
<!-- If this PR introduces breaking changes, describe them here -->

## Additional Notes
<!-- Any additional information reviewers should know -->
```

---

## 5. Configuration Template

**File:** `.github/ISSUE_TEMPLATE/config.yml`

```yaml
blank_issues_enabled: false
contact_links:
  - name: Home Assistant Community Forum
    url: https://community.home-assistant.io/
    about: For general questions and discussions
  - name: Documentation
    url: https://github.com/yourusername/ha-pool-automation/blob/main/README.md
    about: Read the documentation first
  - name: Troubleshooting Guide
    url: https://github.com/yourusername/ha-pool-automation/blob/main/TROUBLESHOOTING.md
    about: Common issues and solutions
```

---

## How to Use These Templates

1. **Create directory structure:**
   ```bash
   mkdir -p .github/ISSUE_TEMPLATE
   ```

2. **Create each file:**
   - Copy content for each template
   - Save in `.github/ISSUE_TEMPLATE/` directory
   - Commit and push to repository

3. **Templates will appear automatically:**
   - When users click "New Issue"
   - They'll see a menu with all options
   - Selecting a template pre-fills the form

4. **Customize as needed:**
   - Adjust fields for your needs
   - Add/remove sections
   - Update labels and assignees

---

## Benefits

✅ **Consistency:** All issues follow same format
✅ **Completeness:** Users provide necessary information
✅ **Efficiency:** Less back-and-forth for details
✅ **Organization:** Easy to categorize and triage
✅ **Professional:** Shows project is well-maintained
