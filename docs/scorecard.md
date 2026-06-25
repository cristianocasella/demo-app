# Scorecard Metrics

The Scorecard tab in Red Hat Developer Hub provides automated quality and security metrics for your component.

## Available Metrics

### GitHub Open Pull Requests

Displays the number of currently open pull requests for the repository. This metric helps track:
- Active development work
- Review backlog
- Contribution activity

**Source**: GitHub API

### OpenSSF Scorecard

The [OpenSSF Scorecard](https://securityscorecards.dev/) project evaluates open source projects on security best practices. The score ranges from 0 to 10, with checks including:

- **Code Review**: Are all changes reviewed before merging?
- **Maintained**: Is the project actively maintained?
- **Signed Releases**: Are releases cryptographically signed?
- **Dependency Updates**: Are dependencies kept up to date?
- **Branch Protection**: Are branch protection rules configured?
- **Security Policy**: Is there a published security policy?
- **CI Tests**: Does the project run tests in CI?
- **Vulnerabilities**: Are there known unfixed vulnerabilities?
- **Dangerous Workflow**: Are GitHub Actions workflows secure?
- **Token Permissions**: Do workflows use least-privilege tokens?

And several other security-focused checks.

**Source**: OpenSSF Scorecard API  
**Update Frequency**: Weekly via GitHub Actions workflow

### File Checks

Verifies the presence of important repository files:

- **LICENSE**: Open source license file
- **README.md**: Project documentation
- **SECURITY.md**: Security policy and vulnerability reporting instructions
- **CONTRIBUTING.md**: Contribution guidelines
- **CODEOWNERS**: Code ownership and review assignments

These files are considered best practices for open source projects and enterprise software development.

## How Metrics are Collected

### Automated Scanning

A GitHub Actions workflow runs weekly (and on every push to main) to:
1. Execute the OpenSSF Scorecard analysis
2. Upload results to the OpenSSF API
3. Store results in GitHub Security tab (SARIF format)

### RHDH Integration

Red Hat Developer Hub automatically:
- Fetches metrics from GitHub API hourly
- Retrieves OpenSSF Scorecard results from the public API
- Checks repository files for presence of standard documentation
- Displays aggregated results in the Scorecard tab

## Improving Your Score

To improve your OpenSSF Scorecard score:

1. **Enable branch protection** on your main branch
2. **Require code reviews** before merging pull requests
3. **Set up CI/CD** with automated testing
4. **Add security files**: SECURITY.md for vulnerability reporting
5. **Use signed commits** and releases when possible
6. **Keep dependencies updated** with tools like Dependabot
7. **Pin GitHub Actions** to specific commit SHAs instead of tags
8. **Use least-privilege permissions** in workflows
9. **Document your project** with README, CONTRIBUTING, and LICENSE files

## Learn More

- [OpenSSF Scorecard Documentation](https://github.com/ossf/scorecard)
- [Red Hat Developer Hub Scorecard Plugin](https://github.com/redhat-developer/rhdh-plugins/tree/main/workspaces/scorecard)
- [OpenSSF Best Practices Badge](https://bestpractices.coreinfrastructure.org/)
