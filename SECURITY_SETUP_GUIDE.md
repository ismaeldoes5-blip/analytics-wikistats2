# 🔒 Enterprise Security Setup Guide - Web UI Instructions

Complete guide for finalizing your enterprise-grade security infrastructure using GitHub's web interface.

---

## Table of Contents
1. [Create Pull Request](#1-create-pull-request)
2. [Enable Branch Protection](#2-enable-branch-protection)
3. [Configure CodeQL](#3-configure-codeql)
4. [Setup CODEOWNERS Review](#4-setup-codeowners-review)
5. [Configure Dependabot](#5-configure-dependabot)
6. [Security Dashboard](#6-security-dashboard)
7. [Monitoring & Alerts](#7-monitoring--alerts)

---

## 1. Create Pull Request

### Step 1.1: Navigate to Pull Requests
1. Go to: https://github.com/ismaeldoes5-blip/analytics-wikistats2
2. Click the **"Pull requests"** tab
3. Click **"New pull request"** button

### Step 1.2: Configure PR
1. **Base branch**: Select `master` (or `main`)
2. **Compare branch**: Select `security-ci-updates`
3. Click **"Create pull request"**

### Step 1.3: Fill PR Details
```
Title: 
chore: add enterprise-grade security and CI improvements

Description:
## Security Enhancements 🔒

This PR introduces comprehensive enterprise-grade security infrastructure:

### What's Included

#### 🔍 Security Automation
- **Dependency Scanning**: Weekly npm audit with SBOM generation
- **Secret Detection**: TruffleHog scanning + credential pattern detection
- **SAST Analysis**: ESLint security rules + anti-pattern detection
- **Vulnerability Scanning**: OSV database checks
- **Build Artifact Scanning**: Ensures no secrets leak into build

#### 🛡️ CI/CD Pipeline
- **Automated Linting**: ESLint + Prettier checks
- **Test Coverage**: Jest with 70% threshold enforcement
- **Build Verification**: Automated build testing
- **Quality Gates**: Pass/fail checks before merge

#### 📦 Dependency Management
- **Weekly Updates**: Dependabot automation
- **Security Grouping**: Immediate security updates
- **Dev/Prod Separation**: Different update strategies
- **License Compliance**: Automated checking

#### 📋 Code Quality
- **ESLint Configuration**: 30+ security rules
- **Prettier Formatting**: Consistent code style
- **CODEOWNERS**: Access control & review requirements
- **Comprehensive .gitignore**: Security-focused exclusions

### Files Added
- `.github/workflows/security.yml` - Enterprise security scanning
- `.github/workflows/ci.yml` - CI/CD pipeline
- `.github/workflows/dependency-check.yml` - License & compliance checks
- `.github/dependabot.yml` - Automated dependency updates
- `.eslintrc.json` - Security-focused linting rules
- `.prettierrc.json` - Code formatting standards
- `.github/CODEOWNERS` - Access control rules
- `SECURITY.md` - Security policy & best practices
- `.gitignore` - Enhanced with security exclusions

### Security Improvements
- ✅ Automated vulnerability detection
- ✅ Secret scanning on every commit
- ✅ Code quality enforcement
- ✅ Test coverage monitoring
- ✅ Build artifact verification
- ✅ Dependency management automation
- ✅ License compliance checking

### How to Review
1. Check workflows in `.github/workflows/`
2. Review security configuration in `.eslintrc.json`
3. Read security policy in `SECURITY.md`
4. Enable branch protection rules (see instructions below)
5. Merge and configure recommended settings

### Next Steps After Merge
1. Enable branch protection rules
2. Configure CodeQL scanning
3. Setup Dependabot alerts
4. Configure deployment protection rules

Closes #N/A
```

4. Click **"Create pull request"**

### Step 1.4: Monitor PR Checks
- Wait for all CI checks to pass
- Review any security warnings
- Approve and merge when ready

---

## 2. Enable Branch Protection

### Step 2.1: Access Settings
1. Go to: https://github.com/ismaeldoes5-blip/analytics-wikistats2
2. Click **Settings** tab
3. Click **Branches** (left sidebar)
4. Click **Add rule** or select existing rule for `master`/`main`

### Step 2.2: Configure Branch Protection Rules

```
Branch name pattern: master (or main)
```

#### ✅ Required Status Checks
- [x] Require status checks to pass before merging
- [x] Require branches to be up to date before merging

**Select required checks:**
- ✅ Lint & Format Check
- ✅ Unit Tests & Coverage
- ✅ Build Project
- ✅ Security Summary Report
- ✅ Quality Gate Check

#### ✅ Pull Request Reviews
- [x] Require pull request reviews before merging
- **Number of required reviewers**: 1
- [x] Require review from Code Owners (CODEOWNERS)
- [x] Dismiss stale pull request approvals when new commits are pushed
- [x] Require approval of the most recent reviewable push

#### ✅ Additional Rules
- [x] Require status checks to pass before merging
- [x] Require branches to be up to date before merging
- [x] Require conversation resolution before merging
- [x] Require deployments to succeed before merging (if applicable)
- [x] Lock branch (optional - for critical repos)
- [x] Restrict who can push to matching branches (optional)

### Step 2.3: Save Configuration
- Click **Create** or **Update**

---

## 3. Configure CodeQL

### Step 3.1: Access Code Scanning
1. Go to: https://github.com/ismaeldoes5-blip/analytics-wikistats2
2. Click **Security** tab (top)
3. Click **Code scanning** (left sidebar)
4. Click **Set up code scanning** or **Add tool**

### Step 3.2: Enable CodeQL
1. Click **CodeQL**
2. Click **Set up this workflow**
3. Choose language: **JavaScript**
4. Review the generated workflow file

### Step 3.3: CodeQL Workflow Configuration

The system will create `.github/workflows/codeql.yml`. Update it:

```yaml
name: CodeQL

on:
  push:
    branches: [master, main, develop]
  pull_request:
    branches: [master, main, develop]
  schedule:
    - cron: '0 3 * * 0'  # Weekly on Sunday

permissions:
  contents: read
  security-events: write

jobs:
  analyze:
    name: Analyze
    runs-on: ubuntu-latest
    permissions:
      actions: read
      contents: read
      security-events: write

    strategy:
      fail-fast: false
      matrix:
        language: ['javascript']

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Initialize CodeQL
        uses: github/codeql-action/init@v2
        with:
          languages: ${{ matrix.language }}
          queries: security-and-quality

      - name: Autobuild
        uses: github/codeql-action/autobuild@v2

      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v2
        with:
          category: "/language:${{ matrix.language }}"
```

### Step 3.4: Commit and Activate
1. Click **Commit changes**
2. Select **Create a new branch for this commit**
3. Name: `add-codeql-scanning`
4. Click **Commit changes**
5. Click **Create Pull Request**
6. Merge once checks pass

---

## 4. Setup CODEOWNERS Review

### Step 4.1: Verify CODEOWNERS File
1. Go to: https://github.com/ismaeldoes5-blip/analytics-wikistats2/blob/security-ci-updates/.github/CODEOWNERS
2. File should contain:
```
* @ismaeldoes5-blip

.github/workflows/security.yml @ismaeldoes5-blip
.github/workflows/ci.yml @ismaeldoes5-blip
.github/dependabot.yml @ismaeldoes5-blip
.eslintrc.json @ismaeldoes5-blip
SECURITY.md @ismaeldoes5-blip
package.json @ismaeldoes5-blip
```

### Step 4.2: Test CODEOWNERS
1. Create a test PR with changes to a CODEOWNERS-protected file
2. Verify that review is required from `@ismaeldoes5-blip`
3. This confirms CODEOWNERS is working

---

## 5. Configure Dependabot

### Step 5.1: Access Dependabot Settings
1. Go to: https://github.com/ismaeldoes5-blip/analytics-wikistats2
2. Click **Settings** tab
3. Click **Code security and analysis** (left sidebar)
4. Scroll to **Dependabot**

### Step 5.2: Enable Dependabot Features

#### Dependabot Alerts
- [x] Enable "Dependabot alerts"
- This alerts you to vulnerable dependencies

#### Dependabot Updates
- [x] Enable "Dependabot security updates"
- [x] Enable "Dependabot version updates"
- Already configured in `.github/dependabot.yml`

#### Dependabot Grouping
Verify your `.github/dependabot.yml` includes:
```yaml
groups:
  security:
    dependency-types: ['all']
    update-types: ['patch']
    patterns: ['*security*', '*cve*']
  development:
    dependency-types: ['dev']
    update-types: ['minor', 'patch']
  production:
    dependency-types: ['prod']
    update-types: ['patch']
```

### Step 5.3: Configure Notifications
1. Go to **Settings** → **Notifications**
2. Select **Repository notifications**
3. [x] Enable alerts for:
   - Dependabot alerts
   - Security alerts
   - CodeQL alerts

---

## 6. Security Dashboard

### Step 6.1: Access Security Tab
1. Go to: https://github.com/ismaeldoes5-blip/analytics-wikistats2
2. Click **Security** tab

### Step 6.2: Monitor Security Features

#### Code Scanning
- View CodeQL results
- Filter by severity
- Track remediation progress

#### Dependabot Alerts
- Review vulnerable dependencies
- Auto-generate PRs for fixes
- Track security updates

#### Secret Scanning
- Monitor for exposed secrets
- Review detected patterns
- Configure custom patterns

#### Deploy Keys
- Manage deployment keys
- Restrict access by environment

---

## 7. Monitoring & Alerts

### Step 7.1: Configure Repository Alerts
1. Go to **Settings** → **Code security and analysis**
2. Enable all alerts:
   - [x] Dependabot alerts
   - [x] Dependabot security updates
   - [x] Secret scanning
   - [x] Push protection

### Step 7.2: Setup Notifications
1. Go to **Settings** → **Notifications**
2. Configure email notifications for:
   - Security alerts
   - CodeQL scanning
   - Dependabot updates

### Step 7.3: Monitor Workflows
1. Go to **Actions** tab
2. View workflow runs
3. Monitor:
   - ✅ Security checks status
   - ✅ CI pipeline results
   - ✅ Dependency checks

### Step 7.4: Create Security Dashboard

#### Via GitHub CLI:
```bash
gh repo view ismaeldoes5-blip/analytics-wikistats2 --web
```

#### Via Web UI:
1. Navigate to Security tab
2. Review all active scanners
3. Configure alerts in Settings

---

## Verification Checklist

### ✅ Pre-Deployment
- [ ] All 9 security files created
- [ ] PR created and reviewed
- [ ] All CI checks passing
- [ ] Security scan results reviewed

### ✅ Post-Merge
- [ ] Branch protection enabled
- [ ] CodeQL configured and running
- [ ] Dependabot active
- [ ] Secret scanning enabled
- [ ] CODEOWNERS working
- [ ] Notifications configured

### ✅ Ongoing
- [ ] Security dashboard monitored weekly
- [ ] Dependabot PRs reviewed
- [ ] CodeQL alerts addressed
- [ ] Security policy documented

---

## Quick Links

### GitHub Repository
- **Main**: https://github.com/ismaeldoes5-blip/analytics-wikistats2
- **Security Branch**: https://github.com/ismaeldoes5-blip/analytics-wikistats2/tree/security-ci-updates

### Settings Pages
- **Branch Protection**: https://github.com/ismaeldoes5-blip/analytics-wikistats2/settings/branches
- **Code Security**: https://github.com/ismaeldoes5-blip/analytics-wikistats2/settings/security_analysis
- **Notifications**: https://github.com/ismaeldoes5-blip/analytics-wikistats2/settings/notifications

### Monitoring Pages
- **Security**: https://github.com/ismaeldoes5-blip/analytics-wikistats2/security
- **Code Scanning**: https://github.com/ismaeldoes5-blip/analytics-wikistats2/security/code-scanning
- **Alerts**: https://github.com/ismaeldoes5-blip/analytics-wikistats2/security/dependabot
- **Actions**: https://github.com/ismaeldoes5-blip/analytics-wikistats2/actions

---

## Troubleshooting

### CodeQL Not Running
**Solution:**
1. Check `.github/workflows/codeql.yml` exists
2. Go to **Actions** tab
3. Manually trigger CodeQL workflow
4. Check for any error messages

### Dependabot Not Creating PRs
**Solution:**
1. Verify `.github/dependabot.yml` is valid YAML
2. Go to **Settings** → **Code security and analysis**
3. Enable **Dependabot version updates**
4. Wait up to 24 hours for first run

### Branch Protection Not Blocking
**Solution:**
1. Go to **Settings** → **Branches**
2. Verify rule is enabled
3. Check "Include administrators" option
4. Ensure required status checks are enabled

### CODEOWNERS Not Requiring Review
**Solution:**
1. Verify `.github/CODEOWNERS` file exists
2. Correct formatting: `path @username`
3. Username must match GitHub handle exactly
4. File must be in `.github/` directory

---

## Additional Resources

- [GitHub Branch Protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches)
- [CodeQL Documentation](https://codeql.github.com/docs/)
- [Dependabot Guide](https://docs.github.com/en/code-security/dependabot)
- [Secret Scanning](https://docs.github.com/en/code-security/secret-scanning)
- [OWASP Top 10](https://owasp.org/Top10/)

---

## Support

For issues or questions:
- 📧 Email: security@wikimedia.org
- 💬 GitHub Issues: Create an issue in this repository
- 📚 Documentation: See SECURITY.md

---

**Last Updated**: August 2026
**Version**: 1.0
**Status**: ✅ Complete
