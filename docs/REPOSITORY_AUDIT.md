# Next.js Repository Audit

This document provides a comprehensive overview of the syed-reza98/next.js repository structure, workflows, and configuration.

**Last Updated:** October 17, 2025

---

## Repository Overview

The Next.js repository is a monorepo managed with pnpm workspaces, containing the Next.js framework, documentation, examples, and related tooling.

### Key Statistics
- **Workflows:** 36 GitHub Actions workflows
- **Custom Actions:** 8 custom GitHub Actions in `.github/actions`
- **Main Language:** JavaScript/TypeScript with Rust (for Turbopack)
- **Package Manager:** pnpm 9.6.0
- **Node Version:** >=20.9.0

---

## Directory Structure

```
next.js/
├── .github/                 # GitHub configuration and workflows
│   ├── actions/            # Custom GitHub Actions (8 actions)
│   ├── workflows/          # CI/CD workflows (36 workflows)
│   ├── ISSUE_TEMPLATE/     # Issue templates
│   ├── DISCUSSION_TEMPLATE/ # Discussion templates
│   └── comments/           # Reusable comment templates
├── apps/                   # Applications
│   └── docs/              # Documentation app (fumadocs-based)
├── packages/              # Next.js packages
├── examples/              # Example applications
├── test/                  # Test suites
├── turbopack/            # Turbopack bundler code
├── crates/               # Rust crates
├── scripts/              # Build and utility scripts
└── contributing/         # Contributing documentation
```

---

## Documentation Setup

### Current Documentation System

The repository uses **fumadocs** for documentation, located at `apps/docs/`:

- **Framework:** Next.js 15.5.3 with fumadocs
- **Content:** MDX files organized by sections (01-app, 02-pages, 03-architecture, 04-community)
- **Build Command:** `pnpm build-docs`
- **Dev Command:** `pnpm dev-docs`

### Documentation Sections

1. **01-app/** - App Router documentation
2. **02-pages/** - Pages Router documentation  
3. **03-architecture/** - Architecture guides
4. **04-community/** - Community resources

### Current Deployment

Documentation is currently deployed to **Vercel** via the `deploy_docs.yml` workflow when changes are made to `apps/docs/**`.

---

## GitHub Actions & Workflows

### Workflow Categories

#### 1. Build & Test Workflows (7)
- `build_and_deploy.yml` - Main build and deployment
- `build_and_test.yml` - Continuous integration testing
- `build_reusable.yml` - Reusable build workflow
- `integration_tests_reusable.yml` - Integration tests
- `test_examples.yml` - Example application tests
- `test_e2e_deploy_release.yml` - E2E deployment tests
- `test_e2e_project_reset_cron.yml` - Scheduled E2E tests

#### 2. Turbopack Workflows (5)
- `turbopack-benchmark.yml` - Performance benchmarks
- `turbopack-nextjs-build-integration-tests.yml` - Build integration tests
- `turbopack-nextjs-dev-integration-tests.yml` - Dev integration tests
- `turbopack-update-tests-manifest.yml` - Test manifest updates
- `test-turbopack-rust-bench-test.yml` - Rust benchmark tests

#### 3. Rspack Workflows (4)
- `rspack-nextjs-build-integration-tests.yml` - Build integration tests
- `rspack-nextjs-dev-integration-tests.yml` - Dev integration tests
- `rspack-update-tests-manifest.yml` - Test manifest updates
- `release-next-rspack.yml` - Rspack release workflow

#### 4. Documentation & Deployment (2)
- `deploy_docs.yml` - Deploy documentation to Vercel
- `setup-nextjs-build.yml` - Setup Next.js build environment

#### 5. Issue Management (4)
- `triage.yml` - Automated issue triaging
- `issue_lock.yml` - Lock closed issues
- `issue_stale.yml` - Mark stale issues
- `issue_wrong_template.yml` - Detect wrong templates

#### 6. Release Management (4)
- `trigger_release.yml` - Trigger new release
- `trigger_release_new.yml` - New release process
- `create_release_branch.yml` - Create release branches
- `force_merge_canary_release_pr.yml` - Merge release PRs

#### 7. Maintenance & Updates (5)
- `update_fonts_data.yml` - Update Google Fonts data
- `update_react.yml` - Update React dependencies
- `upload-tests-manifest.yml` - Upload test manifests
- `code_freeze.yml` - Code freeze automation
- `popular.yml` - Track popular issues/PRs

#### 8. Utility Workflows (5)
- `cancel.yml` - Cancel duplicate workflows
- `graphite_ci_optimizer.yml` - CI optimization
- `pull_request_stats.yml` - PR statistics
- `retry_deploy_test.yml` - Retry failed deployments
- `retry_test.yml` - Retry failed tests

---

## Custom GitHub Actions

The repository includes 8 custom actions in `.github/actions/`:

1. **needs-triage** - Automatic issue triaging
2. **next-integration-stat** - Integration test statistics
3. **next-repo-actions** - Repository automation (issues, PRs, feature requests)
4. **next-stats-action** - Performance statistics collection
5. **setup-rust** - Rust toolchain setup
6. **upload-turboyet-data** - Upload performance data
7. **validate-docs-links** - Validate documentation links
8. **next-repo-actions** - Multiple sub-actions for repo management

---

## Configuration Files

### Build & Development
- `package.json` - Root package configuration
- `pnpm-workspace.yaml` - Workspace configuration
- `turbo.json` - Turborepo configuration
- `tsconfig.json` - TypeScript configuration
- `jest.config.js` - Jest test configuration

### Code Quality
- `.prettierrc.json` - Prettier formatting rules
- `eslint.config.mjs` - ESLint configuration
- `.typos.toml` - Spell checking configuration
- `.alexrc` - Inclusive language checking

### CI/CD
- `.github/labeler.json` - Automatic PR labeling
- `.github/CODEOWNERS` - Code ownership rules
- `vercel.json` - Vercel deployment configuration

### Rust/Cargo
- `Cargo.toml` - Rust workspace configuration
- `rust-toolchain.toml` - Rust toolchain specification
- `.rustfmt.toml` - Rust formatting rules

---

## Key Technologies

### Frontend
- Next.js 15.5.3
- React 19
- TypeScript 5.9.2

### Build Tools
- Turbopack (Rust-based bundler)
- Webpack 5.98.0
- Rspack 1.5.0
- Turborepo 2.5.5

### Testing
- Jest 29.7.0
- Playwright 1.48.0
- Testing Library

### Documentation
- fumadocs (MDX-based documentation framework)

---

## Best Practices Observed

### ✅ Strengths

1. **Comprehensive Testing** - Multiple test workflows for different scenarios
2. **Automated Workflows** - Extensive automation for CI/CD, triaging, and releases
3. **Code Quality** - Multiple linters and formatters configured
4. **Monorepo Structure** - Well-organized with pnpm workspaces
5. **Custom Actions** - Reusable GitHub Actions for common tasks
6. **Documentation** - Dedicated documentation app with modern tooling

### 📋 Areas for Enhancement

1. **GitHub Pages** - No GitHub Pages deployment configured (currently Vercel-only)
2. **Workflow Documentation** - Workflows could benefit from more inline documentation
3. **Performance Monitoring** - Could expand performance tracking visibility
4. **Security Scanning** - Could add more security scanning workflows

---

## Documentation Gaps Identified

1. **Workflow Documentation** - Need comprehensive guide explaining each workflow
2. **GitHub Pages Setup** - Missing GitHub Pages deployment option
3. **Architecture Diagrams** - Could benefit from visual architecture documentation
4. **API Documentation** - API reference could be more prominent
5. **Performance Benchmarks** - Benchmark results could be published publicly

---

## Recommended Next Steps

1. ✅ Create comprehensive workflow documentation
2. ✅ Add GitHub Pages deployment workflow
3. ✅ Document GitHub Pages setup process
4. 📝 Consider adding architecture diagrams
5. 📝 Expand API documentation
6. 📝 Publish performance benchmarks

---

## References

- [Contributing Guide](../contributing.md)
- [Code of Conduct](../CODE_OF_CONDUCT.md)
- [License](../license.md)
- [Next.js Documentation](https://nextjs.org/docs)
