# GitHub Pages Deployment Guide

This guide provides step-by-step instructions for enabling and deploying the Next.js documentation to GitHub Pages.

**Last Updated:** October 17, 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Setup Instructions](#setup-instructions)
4. [Configuration Files](#configuration-files)
5. [Deployment Process](#deployment-process)
6. [Troubleshooting](#troubleshooting)
7. [Best Practices](#best-practices)

---

## Overview

GitHub Pages provides free static site hosting directly from your GitHub repository. This setup enables automatic deployment of the Next.js documentation whenever changes are pushed to the repository.

### Benefits

- ✅ **Free Hosting** - No cost for public repositories
- ✅ **Automatic Deployment** - CI/CD via GitHub Actions
- ✅ **Custom Domain Support** - Use your own domain
- ✅ **HTTPS by Default** - Secure by default
- ✅ **Version Control** - Full history of deployments
- ✅ **Easy Rollback** - Revert to previous versions easily

### Current Setup

- **Documentation Framework:** fumadocs (Next.js-based)
- **Location:** `apps/docs/`
- **Build Command:** `pnpm run build-docs`
- **Output Directory:** `apps/docs/.next` or `apps/docs/out`

---

## Prerequisites

### Repository Requirements

- ✅ Repository must be public (or GitHub Pro/Enterprise for private repos)
- ✅ Admin access to repository settings
- ✅ GitHub Actions enabled
- ✅ Documentation builds successfully

### Local Testing

Before enabling GitHub Pages, test the build locally:

```bash
# Navigate to docs directory
cd apps/docs

# Install dependencies
pnpm install

# Build documentation
pnpm run build-docs

# Test the production build
pnpm start
```

---

## Setup Instructions

### Step 1: Enable GitHub Pages

1. **Navigate to Repository Settings**
   - Go to `https://github.com/syed-reza98/next.js/settings`
   - Click on **Pages** in the left sidebar

2. **Configure Source**
   - Under "Build and deployment"
   - Source: Select **GitHub Actions**
   - This enables deployment via workflow instead of branch

3. **Save Configuration**
   - GitHub will automatically recognize the workflow
   - First deployment will trigger on next commit

### Step 2: Update Next.js Configuration

The documentation needs to be configured for static export. Update `apps/docs/next.config.ts`:

```typescript
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'export',  // Enable static export
  
  // If deploying to a subdirectory (e.g., https://user.github.io/next.js)
  // Uncomment and configure:
  // basePath: '/next.js',
  // assetPrefix: '/next.js',
  
  images: {
    unoptimized: true,  // Required for static export
  },
  
  // Preserve existing configuration
  ...require('./source.config.ts').default,
};

module.exports = nextConfig;
```

### Step 3: Add .nojekyll File

Create `.nojekyll` in the repository root to prevent GitHub from processing the site with Jekyll:

```bash
# From repository root
touch .nojekyll
git add .nojekyll
git commit -m "Add .nojekyll for GitHub Pages"
```

### Step 4: Verify Workflow Configuration

The workflow file is already created at `.github/workflows/deploy-github-pages.yml`. Review and adjust if needed:

- **Trigger Branches:** Currently set to `canary` and `main`
- **Build Path:** Configured for `apps/docs`
- **Node Version:** Set to Node.js 20

### Step 5: Test Deployment

1. **Commit and Push Changes**
   ```bash
   git add .
   git commit -m "Setup GitHub Pages deployment"
   git push origin canary
   ```

2. **Monitor Workflow**
   - Go to `https://github.com/syed-reza98/next.js/actions`
   - Watch the "Deploy Documentation to GitHub Pages" workflow
   - Check for any errors in build or deployment

3. **Access Your Site**
   - Once deployed, site will be available at:
   - `https://syed-reza98.github.io/next.js/` (if repo-based)
   - Or custom domain if configured

---

## Configuration Files

### 1. GitHub Actions Workflow

**Location:** `.github/workflows/deploy-github-pages.yml`

**Key Sections:**
```yaml
# Triggers
on:
  push:
    branches: [canary, main]
    paths: ['apps/docs/**', 'docs/**']
  workflow_dispatch:

# Jobs
jobs:
  build:   # Build documentation
  deploy:  # Deploy to GitHub Pages
```

### 2. Next.js Configuration

**Location:** `apps/docs/next.config.ts`

**Required Changes:**
```typescript
{
  output: 'export',           // Static export
  images: { unoptimized: true },  // No image optimization
  // basePath: '/next.js',    // If using subdirectory
}
```

### 3. .nojekyll File

**Location:** `/.nojekyll` (repository root)

**Purpose:** Prevents Jekyll processing, allows files starting with underscore

### 4. CNAME File (Optional)

**Location:** `/apps/docs/public/CNAME` (if using custom domain)

**Content:**
```
docs.yoursite.com
```

---

## Deployment Process

### Automatic Deployment

The workflow automatically deploys when:

1. ✅ Commits pushed to `canary` or `main` branch
2. ✅ Changes affect `apps/docs/**` or `docs/**`
3. ✅ Manual trigger from Actions tab

### Deployment Steps

1. **Checkout Code** - Repository cloned
2. **Setup Environment** - Node.js and pnpm installed
3. **Cache Dependencies** - pnpm store cached
4. **Install Dependencies** - Project dependencies installed
5. **Build Documentation** - Next.js build executed
6. **Upload Artifact** - Build output uploaded
7. **Deploy Pages** - Artifact deployed to GitHub Pages
8. **Comment URL** - Deployment URL posted (on PRs)

### Deployment Time

- **First Deployment:** 5-10 minutes
- **Subsequent Deployments:** 3-5 minutes
- **Cache Hit:** 2-3 minutes

---

## Troubleshooting

### Common Issues

#### 1. Build Fails

**Symptom:** Workflow fails during build step

**Solutions:**
```bash
# Test build locally
cd apps/docs
pnpm install
pnpm run build-docs

# Check for errors
# Fix any TypeScript or build errors
# Ensure all dependencies are installed
```

#### 2. 404 Errors on Pages

**Symptom:** Homepage loads but other pages show 404

**Solutions:**
- Check `basePath` configuration in `next.config.ts`
- Ensure `output: 'export'` is set
- Verify routing matches expected structure
- Check for client-side only features (not supported in static export)

#### 3. Images Not Loading

**Symptom:** Images show broken links

**Solutions:**
- Set `images: { unoptimized: true }` in `next.config.ts`
- Use relative paths for images
- Store images in `public/` directory
- Update `basePath` if using subdirectory

#### 4. Styles Not Applied

**Symptom:** Page loads but styling is missing

**Solutions:**
- Check CSS imports
- Verify Tailwind configuration
- Ensure CSS files in `public/` directory
- Check `assetPrefix` configuration

#### 5. Workflow Permissions Error

**Symptom:** "Permission denied" during deployment

**Solutions:**
1. Go to Repository Settings → Actions → General
2. Scroll to "Workflow permissions"
3. Select "Read and write permissions"
4. Check "Allow GitHub Actions to create and approve pull requests"
5. Save changes

#### 6. Pages Not Enabled

**Symptom:** No deployment URL generated

**Solutions:**
1. Go to Repository Settings → Pages
2. Under "Build and deployment"
3. Source: Select "GitHub Actions"
4. Save and retry workflow

---

## Best Practices

### Performance Optimization

1. **Image Optimization**
   ```typescript
   // Use Next.js Image component with static export
   import Image from 'next/image'
   
   <Image 
     src="/image.png" 
     width={500} 
     height={300}
     unoptimized
   />
   ```

2. **Code Splitting**
   - Next.js automatically code splits
   - Use dynamic imports for large components
   - Keep bundle sizes small

3. **Caching Strategy**
   - Leverage browser caching
   - Use immutable asset URLs
   - Configure cache headers

### Security

1. **Content Security Policy**
   ```typescript
   // In next.config.ts
   async headers() {
     return [{
       source: '/(.*)',
       headers: [{
         key: 'Content-Security-Policy',
         value: "default-src 'self';"
       }]
     }]
   }
   ```

2. **Dependency Updates**
   - Regularly update dependencies
   - Run security audits: `pnpm audit`
   - Use Dependabot for automated updates

### Monitoring

1. **Deployment Status**
   - Monitor GitHub Actions for failures
   - Set up notifications for deployment status
   - Check deployment logs regularly

2. **Site Health**
   - Test all links after deployment
   - Verify mobile responsiveness
   - Check browser compatibility

3. **Analytics (Optional)**
   - Add Google Analytics
   - Track page views
   - Monitor user behavior

---

## Custom Domain Setup

### Step 1: Add CNAME File

Create `apps/docs/public/CNAME`:
```
docs.yoursite.com
```

### Step 2: Configure DNS

Add DNS records at your domain provider:

**Option A: Custom subdomain**
```
Type: CNAME
Name: docs
Value: syed-reza98.github.io
```

**Option B: Apex domain**
```
Type: A
Name: @
Value: 185.199.108.153
Value: 185.199.109.153
Value: 185.199.110.153
Value: 185.199.111.153
```

### Step 3: Configure in GitHub

1. Go to Repository Settings → Pages
2. Under "Custom domain"
3. Enter your domain: `docs.yoursite.com`
4. Save and wait for DNS check
5. Enable "Enforce HTTPS"

### Step 4: Update Configuration

Update `apps/docs/next.config.ts`:
```typescript
const nextConfig = {
  output: 'export',
  // Remove basePath for custom domain
  images: { unoptimized: true },
};
```

---

## Maintenance

### Regular Tasks

- [ ] **Weekly:** Check deployment status
- [ ] **Monthly:** Update dependencies
- [ ] **Quarterly:** Review and optimize build
- [ ] **Yearly:** Audit and cleanup unused pages

### Version Control

- Tag releases: `git tag -a v1.0.0 -m "Release 1.0.0"`
- Keep changelog updated
- Document breaking changes
- Maintain migration guides

### Backup Strategy

- GitHub automatically retains deployment history
- Consider additional backups of built assets
- Document restore procedures
- Test recovery process

---

## Advanced Configuration

### Environment Variables

Set secrets in GitHub Settings → Secrets and variables → Actions:

```yaml
# In workflow file
env:
  NEXT_PUBLIC_API_URL: ${{ secrets.API_URL }}
```

### Multiple Environments

Deploy to staging and production:

```yaml
# Add to workflow
environment:
  name: ${{ github.ref == 'refs/heads/main' && 'production' || 'staging' }}
```

### Preview Deployments

For pull request previews, consider:
- Vercel for automatic PR previews
- Netlify Deploy Previews
- AWS S3 + CloudFront

---

## Resources

### Documentation
- [GitHub Pages Docs](https://docs.github.com/en/pages)
- [Next.js Static Export](https://nextjs.org/docs/app/building-your-application/deploying/static-exports)
- [GitHub Actions](https://docs.github.com/en/actions)

### Tools
- [fumadocs Documentation](https://fumadocs.vercel.app/)
- [Next.js Documentation](https://nextjs.org/docs)
- [GitHub Pages Health Check](https://github.com/pages-health-check)

### Support
- [GitHub Community](https://github.community/)
- [Next.js Discussions](https://github.com/vercel/next.js/discussions)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/github-pages)

---

## Checklist for GitHub Pages Deployment

Use this checklist to ensure successful deployment:

### Pre-Deployment
- [ ] Documentation builds successfully locally
- [ ] Repository is public or has appropriate access
- [ ] GitHub Actions is enabled
- [ ] Admin access to repository

### Configuration
- [ ] `.github/workflows/deploy-github-pages.yml` created
- [ ] `next.config.ts` updated with `output: 'export'`
- [ ] `.nojekyll` file added to repository root
- [ ] Images configured with `unoptimized: true`
- [ ] Base path configured (if needed)

### GitHub Settings
- [ ] Pages enabled in repository settings
- [ ] Source set to "GitHub Actions"
- [ ] Workflow permissions set to "Read and write"
- [ ] Custom domain configured (if applicable)
- [ ] HTTPS enforced

### Testing
- [ ] Workflow runs successfully
- [ ] Site accessible at GitHub Pages URL
- [ ] All pages load correctly
- [ ] Images display properly
- [ ] Links work as expected
- [ ] Mobile responsive
- [ ] Browser compatibility verified

### Post-Deployment
- [ ] Monitor deployment status
- [ ] Set up notifications
- [ ] Document custom configuration
- [ ] Share deployment URL with team
- [ ] Update README with deployment info

---

## Next Steps

After successful GitHub Pages deployment:

1. ✅ Monitor first deployment
2. ✅ Test all documentation pages
3. ✅ Set up custom domain (optional)
4. ✅ Configure analytics (optional)
5. ✅ Add deployment badge to README
6. ✅ Document any custom configuration
7. ✅ Share deployment URL with stakeholders

---

## Support

For issues or questions:

1. Check [Troubleshooting](#troubleshooting) section
2. Review [GitHub Pages documentation](https://docs.github.com/en/pages)
3. Open an issue in the repository
4. Contact repository maintainers

---

**Happy Deploying! 🚀**
