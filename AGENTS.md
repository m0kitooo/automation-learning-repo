# AGENTS.md

## Project Purpose

This project is configured to automatically format code using Prettier and github-actions when pushing to remote repository branches. The goal is to ensure consistent code formatting across all commits without requiring manual intervention.

## Automated Code Formatting

### Overview
This repository uses Prettier for automatic code formatting that triggers when code is pushed to remote branches. This ensures:
- Consistent code style across the entire codebase
- Automatic formatting without manual developer intervention
- Clean, readable code in all commits

### Implementation Options

#### Option 1: GitHub Actions (Recommended)
Create `.github/workflows/prettier.yml`:

```yaml
name: Format Code with Prettier
on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  format:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run Prettier
        run: npx prettier --write .
      
      - name: Commit changes
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add .
          git diff --staged --quiet || git commit -m "Auto-format code with Prettier"
          git push
```