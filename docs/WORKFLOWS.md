# GitHub Workflows Documentation

This document provides detailed documentation for all GitHub Actions workflows in the Next.js repository.

**Last Updated:** October 17, 2025

---

## Table of Contents

1. [Build & Test Workflows](#build--test-workflows)
2. [Turbopack Workflows](#turbopack-workflows)
3. [Rspack Workflows](#rspack-workflows)
4. [Documentation & Deployment](#documentation--deployment)
5. [Issue Management](#issue-management)
6. [Release Management](#release-management)
7. [Maintenance & Updates](#maintenance--updates)
8. [Utility Workflows](#utility-workflows)

---

## Build & Test Workflows

### build_and_test.yml

**Purpose:** Main CI workflow for building and testing Next.js on pull requests and pushes to canary.

**Triggers:**
- Push to `canary` branch
- Pull request opened or synchronized

**Key Features:**
- Determines if changes are docs-only to skip unnecessary tests
- Checks if the commit is a release
- Runs comprehensive test suites across multiple Node versions
- Supports Webpack, Turbopack, and Rspack testing modes
- Tests across different browsers (Chrome, Firefox, Safari)

**Environment Variables:**
- `NODE_MAINTENANCE_VERSION: 20`
- `NODE_LTS_VERSION: 22`

**Jobs:**
- `optimize-ci` - CI optimization using Graphite
- `changes` - Determine what files changed
- `build` - Build Next.js packages (calls build_reusable.yml)
- `test-unit` - Run unit tests
- `test-dev` - Development mode tests
- `test-start` - Production build tests
- `test-integration` - Integration tests
- `test-firefox-safari` - Browser compatibility tests

---

### build_reusable.yml

**Purpose:** Reusable workflow for building Next.js packages.

**Usage:** Called by other workflows that need a Next.js build.

**Key Steps:**
1. Install pnpm and dependencies
2. Build Next.js packages
3. Cache build artifacts
4. Upload build artifacts for use in other jobs

---

### build_and_deploy.yml

**Purpose:** Build and deploy Next.js for release candidates and production releases.

**Triggers:**
- Manual workflow dispatch
- Triggered by release workflows

**Key Features:**
- Builds all packages
- Runs deployment tests
- Publishes to npm (if configured)
- Creates deployment artifacts

---

### integration_tests_reusable.yml

**Purpose:** Reusable workflow for running integration tests.

**Features:**
- Parameterized test suite selection
- Supports different Next.js modes (dev, start, deploy)
- Parallel test execution
- Test result aggregation

---

### test_examples.yml

**Purpose:** Test all example applications in the examples/ directory.

**Triggers:**
- Pull requests affecting examples/
- Scheduled runs

**Key Features:**
- Validates each example builds successfully
- Checks for broken dependencies
- Ensures examples follow best practices

---

### test_e2e_deploy_release.yml

**Purpose:** End-to-end tests for deployment scenarios.

**Triggers:**
- Release workflow completion
- Manual trigger

**Features:**
- Tests actual deployment scenarios
- Validates production builds
- Checks deployment performance

---

### test_e2e_project_reset_cron.yml

**Purpose:** Scheduled cleanup and reset of test projects.

**Triggers:**
- Scheduled (cron)
- Manual trigger

**Features:**
- Cleans up test artifacts
- Resets test databases
- Ensures clean test environment

---

## Turbopack Workflows

### turbopack-benchmark.yml

**Purpose:** Benchmark Turbopack performance.

**Triggers:**
- Pull requests affecting Turbopack code
- Scheduled runs
- Manual trigger

**Metrics Collected:**
- Build time
- Hot reload time
- Memory usage
- Bundle size

**Output:** Performance comparison comments on PRs

---

### turbopack-nextjs-build-integration-tests.yml

**Purpose:** Integration tests for Turbopack build mode.

**Features:**
- Tests production builds with Turbopack
- Validates output correctness
- Compares with Webpack builds
- Tests different optimization levels

---

### turbopack-nextjs-dev-integration-tests.yml

**Purpose:** Integration tests for Turbopack development mode.

**Features:**
- Tests dev server startup
- Validates Hot Module Replacement (HMR)
- Tests fast refresh functionality
- Validates dev-only features

---

### turbopack-update-tests-manifest.yml

**Purpose:** Update test manifest for Turbopack tests.

**Triggers:**
- Changes to test files
- Manual trigger

**Features:**
- Generates test manifest
- Updates test metadata
- Tracks test coverage

---

### test-turbopack-rust-bench-test.yml

**Purpose:** Rust-level benchmark tests for Turbopack.

**Features:**
- Native Rust benchmarks
- Performance regression detection
- Memory profiling
- Compiler performance tests

---

## Rspack Workflows

### rspack-nextjs-build-integration-tests.yml

**Purpose:** Integration tests for Rspack build mode.

**Similar to Turbopack build tests but for the Rspack bundler alternative.**

---

### rspack-nextjs-dev-integration-tests.yml

**Purpose:** Integration tests for Rspack development mode.

**Similar to Turbopack dev tests but for Rspack.**

---

### rspack-update-tests-manifest.yml

**Purpose:** Update test manifest for Rspack tests.

**Maintains test metadata for Rspack integration.**

---

### release-next-rspack.yml

**Purpose:** Release Next.js builds with Rspack integration.

**Triggers:**
- Manual workflow dispatch
- Release workflow

**Features:**
- Builds Next.js with Rspack
- Publishes Rspack-specific packages
- Creates release artifacts

---

## Documentation & Deployment

### deploy_docs.yml

**Purpose:** Deploy documentation to Vercel on documentation changes.

**Triggers:**
- Pull requests affecting `apps/docs/**`

**Process:**
1. Checkout repository
2. Install Vercel CLI
3. Build documentation with fumadocs
4. Deploy to Vercel preview or production
5. Comment deployment URL on PR (currently commented out)

**Environment Variables:**
- `VERCEL_API_TOKEN` - Required for deployment
- `DEPLOY_ENVIRONMENT` - `preview` or `production`

**Script Used:** `scripts/deploy-docs.sh`

---

### setup-nextjs-build.yml

**Purpose:** Setup action for Next.js build environment.

**Usage:** Called by other workflows to prepare build environment.

**Setup Steps:**
- Install Node.js
- Setup pnpm
- Cache dependencies
- Configure build tools

---

## Issue Management

### triage.yml

**Purpose:** Automatically triage new issues and comments.

**Triggers:**
- Issue opened
- Issue labeled
- Issue comment created

**Features:**
- Uses [Nissuer](https://github.com/balazsorban44/nissuer) for automation
- Adds labels based on issue content
- Validates reproduction links
- Posts helpful comments from `.github/comments/`

**Comment Templates:**
- `good-first-issue.md` - Welcome message for good first issues
- `invalid-reproduction.md` - Request valid reproduction
- `simplify-reproduction.md` - Request simpler reproduction
- `verify-canary.md` - Request testing with canary
- `resolved.md` - Issue resolution message

**Validation Rules:**
- Reproduction must be from allowed hosts (GitHub, CodeSandbox, StackBlitz, etc.)
- Reproduction cannot be from vercel/next.js itself
- Link must be in the correct section

---

### issue_lock.yml

**Purpose:** Automatically lock resolved issues after a period.

**Triggers:**
- Scheduled (likely daily)

**Features:**
- Locks issues closed for X days
- Prevents necro-posting
- Keeps issue tracker manageable

---

### issue_stale.yml

**Purpose:** Mark and close stale issues.

**Triggers:**
- Scheduled runs

**Features:**
- Marks issues inactive for X days
- Closes issues stale for additional Y days
- Exempts issues with certain labels
- Posts stale warning comment

---

### issue_wrong_template.yml

**Purpose:** Detect and flag issues using wrong templates.

**Triggers:**
- Issue opened

**Features:**
- Validates issue follows template
- Requests template usage
- Adds labels for wrong template

---

## Release Management

### trigger_release.yml

**Purpose:** Trigger a new Next.js release.

**Triggers:**
- Manual workflow dispatch
- Scheduled (for canary releases)

**Process:**
1. Version bump
2. Changelog generation
3. Create release branch
4. Trigger build and publish

**Permissions Required:**
- `contents: write`
- `packages: write`

---

### trigger_release_new.yml

**Purpose:** New release process workflow.

**Similar to trigger_release.yml but with updated release process.**

---

### create_release_branch.yml

**Purpose:** Create a new release branch.

**Triggers:**
- Called by release workflows
- Manual trigger

**Process:**
1. Create branch from canary
2. Update version numbers
3. Generate changelog
4. Create release PR

---

### force_merge_canary_release_pr.yml

**Purpose:** Force merge release PR to canary.

**Triggers:**
- Manual workflow dispatch

**Use Case:** Emergency releases or when normal PR merge is blocked.

**Safety:** Requires explicit approval due to force merge.

---

## Maintenance & Updates

### update_fonts_data.yml

**Purpose:** Update Google Fonts metadata.

**Triggers:**
- Scheduled (likely weekly)
- Manual trigger

**Process:**
1. Fetch latest Google Fonts data
2. Update font metadata files
3. Create PR if changes detected

---

### update_react.yml

**Purpose:** Update React dependencies to latest canary.

**Triggers:**
- Scheduled (likely daily for canary)
- Manual trigger

**Process:**
1. Check for new React releases
2. Update package.json versions
3. Run tests
4. Create PR if successful

---

### upload-tests-manifest.yml

**Purpose:** Upload test manifests for tracking test coverage.

**Triggers:**
- After test runs complete

**Features:**
- Aggregates test results
- Uploads to artifact storage
- Tracks test coverage trends

---

### code_freeze.yml

**Purpose:** Automate code freeze process for releases.

**Triggers:**
- Manual trigger
- Scheduled (before major releases)

**Features:**
- Locks main branch
- Adds code freeze labels
- Notifies team

---

### popular.yml

**Purpose:** Track and notify about popular issues, PRs, and feature requests.

**Triggers:**
- Scheduled (every Monday at 10AM UTC)
- Manual trigger

**Process:**
1. Query top 15 most reacted issues (90 days)
2. Query top 15 most reacted PRs (90 days)
3. Query top 15 most reacted feature requests (90 days)
4. Send notification to Slack

**Environment Variables:**
- `GITHUB_TOKEN` - For API access
- `SLACK_TOKEN` - For notifications

---

## Utility Workflows

### cancel.yml

**Purpose:** Cancel duplicate workflow runs to save CI time.

**Triggers:**
- Workflow run started

**Features:**
- Detects duplicate runs
- Cancels outdated runs
- Keeps latest run only

---

### graphite_ci_optimizer.yml

**Purpose:** Optimize CI execution using Graphite.

**Features:**
- Intelligent test selection
- Parallel execution optimization
- CI cost reduction

---

### pull_request_stats.yml

**Purpose:** Collect and report PR statistics.

**Triggers:**
- Pull request events

**Metrics:**
- Build time
- Test duration
- Bundle size changes
- Performance impact

**Output:** Comment on PR with statistics

---

### retry_deploy_test.yml

**Purpose:** Retry failed deployment tests.

**Triggers:**
- Workflow failure
- Manual trigger

**Features:**
- Automatic retry on transient failures
- Exponential backoff
- Failure notification

---

### retry_test.yml

**Purpose:** Retry failed test runs.

**Triggers:**
- Test workflow failure
- Manual trigger

**Features:**
- Retries flaky tests
- Reports persistent failures
- Tracks test reliability

---

## Custom GitHub Actions

### Actions Overview

Located in `.github/actions/`, these are reusable actions:

1. **needs-triage** - Issue triaging automation
2. **next-integration-stat** - Integration test statistics
3. **next-repo-actions** - Repository management actions
4. **next-stats-action** - Performance statistics
5. **setup-rust** - Rust toolchain configuration
6. **upload-turboyet-data** - Performance data upload
7. **validate-docs-links** - Documentation link validation

---

## Workflow Best Practices

### Security
- All workflows use pinned action versions (e.g., `@v4`)
- Secrets stored in GitHub Secrets
- Limited permissions per workflow
- No hardcoded credentials

### Performance
- Dependency caching enabled
- Parallel job execution
- Conditional job execution
- Build artifact reuse

### Reliability
- Retry mechanisms for flaky tests
- Timeout configurations
- Error handling
- Status checks

### Maintainability
- Reusable workflows
- Clear naming conventions
- Inline documentation
- Consistent structure

---

## Environment Variables & Secrets

### Required Secrets
- `GITHUB_TOKEN` - Provided by GitHub Actions
- `VERCEL_API_TOKEN` - For documentation deployment
- `SLACK_TOKEN` - For notifications
- `NISSUER_WEBHOOK_URL` - For issue triaging
- `NISSUER_WEBHOOK_SECRET` - Webhook authentication

### Common Environment Variables
- `NODE_LTS_VERSION` - Node.js LTS version
- `NODE_MAINTENANCE_VERSION` - Maintenance version
- `NEXT_TEST_MODE` - Test mode (dev/start/deploy)
- `IS_TURBOPACK_TEST` - Enable Turbopack testing
- `IS_WEBPACK_TEST` - Enable Webpack testing

---

## Monitoring & Debugging

### Workflow Logs
- All workflows write detailed logs
- Logs retained for 90 days
- Accessible via GitHub Actions UI

### Notifications
- Slack notifications for important workflows
- GitHub notifications for failures
- Email notifications (configurable)

### Metrics
- Workflow duration tracking
- Success/failure rates
- Resource usage monitoring

---

## Contributing to Workflows

### Adding a New Workflow

1. Create `.yml` file in `.github/workflows/`
2. Follow naming convention: `<purpose>_<action>.yml`
3. Add comprehensive comments
4. Test on a fork first
5. Document in this file
6. Submit PR

### Modifying Existing Workflows

1. Understand current behavior
2. Test changes on fork
3. Update documentation
4. Consider backward compatibility
5. Review impact on other workflows

### Best Practices

- Use reusable workflows when possible
- Keep workflows focused on single purpose
- Add appropriate timeout values
- Use caching effectively
- Handle errors gracefully
- Document all parameters

---

## Troubleshooting

### Common Issues

**Workflow not triggering:**
- Check trigger conditions
- Verify branch names
- Check file path filters

**Tests failing:**
- Check Node.js version compatibility
- Verify dependencies installed
- Review recent changes

**Deployment failing:**
- Verify secrets are set
- Check API tokens validity
- Review deployment logs

**Performance issues:**
- Review caching configuration
- Check parallel execution
- Optimize test selection

---

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Next.js Contributing Guide](../contributing.md)
- [Workflow Examples](https://github.com/actions/starter-workflows)
