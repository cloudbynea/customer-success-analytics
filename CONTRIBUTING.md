# 🤝 Contributing to Customer Success Analytics

We love contributions! This document explains how to contribute to the project.

---

## Code of Conduct

Be respectful, inclusive, and professional. We're all here to help each other succeed.

---

## Ways to Contribute

### 1. Report Bugs 🐛
- Found a calculation error?
- Dashboard not working?
- Documentation unclear?

**How:** Open a GitHub Issue with:
```
Title: [BUG] Brief description
Description:
- What did you expect?
- What actually happened?
- How to reproduce it?
- Screenshots if applicable
```

### 2. Suggest Improvements 💡
- New metric idea?
- Better visualization?
- Missing documentation?

**How:** Open a GitHub Issue with:
```
Title: [FEATURE] Your idea
Description:
- What problem does this solve?
- How would it work?
- Any examples?
- Why is it valuable?
```

### 3. Improve Documentation 📚
- Fix typos
- Add examples
- Clarify confusing sections
- Translate to other languages

**How:** 
1. Click the file in GitHub
2. Click the pencil icon (Edit)
3. Make changes
4. Click "Commit changes"
5. Create pull request

### 4. Add Code/Tools 💻
- Python calculators
- HTML dashboards
- Excel templates
- API integrations

**How:**
1. Fork the repository
2. Create a branch: `git checkout -b feature/your-feature`
3. Add your code
4. Test thoroughly
5. Create a pull request with description

### 5. Share Your Experience 🎤
- How you use these tools
- What metrics matter most
- Real examples from your company
- Lessons learned

**How:** 
- Comment on issues
- Join discussions
- Share in Slack/email
- Email: support@yourdomain.com

---

## Development Setup

### For Local Development:

```bash
# Clone repository
git clone https://github.com/your-username/customer-success-analytics.git
cd customer-success-analytics

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Make your changes
# Test thoroughly
# Commit with clear messages
```

### File Structure:

```
When adding new tools:
├── tools/your-tool/
│   ├── README.md (documentation)
│   ├── main_calculator.py (core logic)
│   ├── requirements.txt (dependencies)
│   ├── example_data.csv (sample input)
│   └── dashboard.html (visualization)
```

---

## Pull Request Process

### Before You Start:
1. Check existing issues/PRs (don't duplicate)
2. Discuss large changes first (open an issue)
3. Keep changes focused (one feature per PR)

### Creating a Pull Request:

1. **Fork the repository**
   - Click "Fork" button in GitHub

2. **Create a branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Keep commits small and focused
   - Write clear commit messages

4. **Test thoroughly**
   ```bash
   python -m pytest tests/
   ```

5. **Update documentation**
   - Update README if needed
   - Add examples
   - Document any new dependencies

6. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Open Pull Request**
   - Click "New pull request" on GitHub
   - Add clear title and description
   - Reference any related issues (#123)

### PR Description Template:

```markdown
## Description
Brief summary of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Performance improvement

## How to Test
Steps to verify the changes work

## Related Issues
Fixes #123

## Checklist
- [ ] Code follows style guide
- [ ] Documentation updated
- [ ] Tests added/updated
- [ ] No breaking changes
```

---

## Coding Standards

### Python Code

```python
# Follow PEP 8
# Use clear variable names
# Add docstrings
# Comment complex logic

def calculate_clv(
    monthly_arpu: float,
    monthly_churn_rate: float,
    gross_margin: float,
    cac: float
) -> dict:
    """
    Calculate Customer Lifetime Value
    
    Args:
        monthly_arpu: Average revenue per user
        monthly_churn_rate: % customers lost per month
        gross_margin: % of revenue after COGS
        cac: Customer acquisition cost
        
    Returns:
        dict with CLV metrics
    """
    # Implementation...
    return results
```

### Documentation

```markdown
# Clear Headings
Use H1 for titles, H2 for sections

## Subsection
- Use bullet points for lists
- Number steps in procedures

**Bold** for emphasis
`code` for inline code

    code blocks for examples
```

### File Naming

```
✅ Good:
- health_score_calculator.py
- customer_segmentation.py
- retention_analysis.py

❌ Avoid:
- HealthScoreCalc.py
- file1.py
- my_script.py
```

---

## Documentation Standards

### README Requirements

```markdown
# Clear Title

## Overview
What is this?

## Installation
How to use it?

## Usage Example
Show it working

## Configuration
How to customize?

## Testing
How to verify?

## Troubleshooting
Common issues & solutions

## Contributing
How to improve it?

## License
MIT
```

### Comment Your Code

```python
# ❌ Bad - no explanation
mrr_retention = beginning_mrr * 0.95

# ✅ Good - clear purpose
# Apply 5% churn rate (historical avg for this cohort)
mrr_retention = beginning_mrr * (1 - 0.05)
```

---

## Testing Requirements

### Before submitting a PR, ensure:

```bash
# Run tests
pytest tests/

# Check code style
pylint tools/

# Test with sample data
python tools/health-score/calculator.py --file sample_data.csv

# Verify documentation renders
# (open HTML files in browser)

# Manual testing
# (try with your own data)
```

### Writing Tests

```python
import unittest

class TestHealthScore(unittest.TestCase):
    def test_perfect_health(self):
        """Score should be 100 with all factors maxed"""
        result = calculate_score(
            adoption=100,
            sentiment=100,
            payment=100,
            velocity=100
        )
        self.assertEqual(result, 100)
    
    def test_at_risk(self):
        """Score should be <60 with bad metrics"""
        result = calculate_score(
            adoption=30,
            sentiment=40,
            payment=20,
            velocity=25
        )
        self.assertLess(result, 60)
```

---

## Commit Message Guidelines

```
❌ Bad commits:
- "fixed stuff"
- "update"
- "asdf"

✅ Good commits:
- "Add CLV calculator with discount rate support"
- "Fix NRR rounding error in quarterly calculations"
- "Update documentation for health score weights"
- "Add test coverage for edge cases in payback period"

Format:
[TYPE] Brief description

Optional details:
- What was changed
- Why it was changed
- Any breaking changes
- Related issues

Types:
- feat: New feature
- fix: Bug fix
- docs: Documentation
- test: Tests
- refactor: Code reorganization
```

---

## Review Process

### What We Look For:

✅ **Does it work?**
- Runs without errors
- Handles edge cases
- Validates input

✅ **Is it well-documented?**
- Clear comments
- Usage examples
- Parameter explanations

✅ **Does it follow standards?**
- Code style consistent
- Naming conventions
- Structure logical

✅ **Is it tested?**
- Unit tests included
- Edge cases covered
- Works with sample data

✅ **Does it solve the problem?**
- Addresses the issue
- Doesn't break existing features
- Provides value

### Timeline:

- **Small changes** (docs, typos): 1-2 days
- **Medium PRs** (new features): 3-5 days  
- **Large changes**: May need discussion first

---

## Community

### Where to Connect:

- **GitHub Issues**: Bug reports and feature requests
- **GitHub Discussions**: Q&A and general discussion
- **Email**: support@yourdomain.com
- **Slack**: (if your company has a community Slack)

### How to Ask for Help:

```markdown
Hi team!

I'm working on [what you're doing] and hit [specific problem].

I've tried [what you've tried] but [what happened].

Here's [code snippet, error message, screenshot].

Any suggestions? 🙏
```

---

## Recognition

### We Give Credit To:

- **Code contributors**: Listed in CONTRIBUTORS.md
- **Issue reporters**: Named in CHANGELOG
- **Documentation improvers**: Listed in README
- **Community members**: Featured in announcements

### Levels of Contribution:

```
🥉 Bronze: 1 merged PR
🥈 Silver: 5 merged PRs
🥇 Gold: 10 merged PRs + community impact
👑 Champion: 25+ PRs + major features
```

---

## Common Questions

**Q: Do I need permission to contribute?**
A: No! Fork and submit a PR. We review everything.

**Q: How long does review take?**
A: 3-7 days for most PRs. Complex changes may take longer.

**Q: Can I contribute code even if I'm not technical?**
A: Yes! Documentation, examples, and feedback are equally valuable.

**Q: What if my PR is rejected?**
A: We provide feedback. Iterate and resubmit. Most PRs improve after feedback.

**Q: Can I make breaking changes?**
A: Preferably not. If necessary, discuss first in an issue.

**Q: How do I report a security vulnerability?**
A: Email security@yourdomain.com (don't post publicly).

---

## Resources

- **GitHub Flow**: https://guides.github.com/introduction/flow/
- **Markdown Guide**: https://www.markdownguide.org/
- **Python Style**: https://pep8.org/
- **Git Cheat Sheet**: https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf

---

## Thank You! 🙏

Every contribution makes this tool better for the entire CS community.

**Together, we're building the best open-source CS analytics platform.**

---

Questions? Open an issue or email support@yourdomain.com
