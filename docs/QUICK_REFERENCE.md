# Quick Reference - GitHub Pages Deployment

**One-page reference for deploying Next.js documentation to GitHub Pages**

---

## 🚀 5-Minute Setup

### Step 1: Enable GitHub Pages (2 min)
```
1. Go to: https://github.com/syed-reza98/next.js/settings/pages
2. Under "Build and deployment"
3. Source: Select "GitHub Actions"
4. Click "Save"
```

### Step 2: Configure Permissions (1 min)
```
1. Go to: https://github.com/syed-reza98/next.js/settings/actions
2. Click "General" in left sidebar
3. Scroll to "Workflow permissions"
4. Select "Read and write permissions"
5. Check "Allow GitHub Actions to create and approve pull requests"
6. Click "Save"
```

### Step 3: Deploy (2 min)
```
1. Merge this PR (copilot/audit-repository-and-workflows)
2. Go to: https://github.com/syed-reza98/next.js/actions
3. Watch "Deploy Documentation to GitHub Pages" workflow
4. Wait for completion (~3-5 minutes)
5. Access site at: https://syed-reza98.github.io/next.js/
```

---

## 📁 Files Created

| File | Purpose |
|------|---------|
| `.github/workflows/deploy-github-pages.yml` | Deployment workflow |
| `.nojekyll` | GitHub Pages config |
| `docs/SUMMARY.md` | Executive summary |
| `docs/REPOSITORY_AUDIT.md` | Repository analysis |
| `docs/WORKFLOWS.md` | Workflow docs |
| `docs/GITHUB_PAGES_SETUP.md` | Setup guide |
| `docs/DEPLOYMENT_CHECKLIST.md` | Verification checklist |
| `docs/README.md` | Documentation index |

---

## 🔍 Workflow Triggers

The deployment workflow runs when:

- ✅ Push to `canary` or `main` branch
- ✅ Changes to `apps/docs/**` or `docs/**`
- ✅ Manual trigger from Actions tab

---

## 🛠️ Commands

### Local Development
```bash
cd apps/docs
pnpm install
pnpm dev-docs       # Start dev server
pnpm build-docs     # Build for production
pnpm start          # Start production server
```

### Testing Build
```bash
cd apps/docs
pnpm install
pnpm build-docs     # Should complete without errors
```

---

## 🔗 Important Links

| Resource | URL |
|----------|-----|
| **Repository Settings** | https://github.com/syed-reza98/next.js/settings |
| **GitHub Pages** | https://github.com/syed-reza98/next.js/settings/pages |
| **Actions** | https://github.com/syed-reza98/next.js/actions |
| **Workflows** | https://github.com/syed-reza98/next.js/tree/canary/.github/workflows |
| **Documentation** | https://github.com/syed-reza98/next.js/tree/canary/docs |

---

## 📖 Documentation Quick Links

- [Full Setup Guide](./GITHUB_PAGES_SETUP.md) - Detailed instructions
- [Repository Audit](./REPOSITORY_AUDIT.md) - Structure overview
- [Workflows](./WORKFLOWS.md) - All 36 workflows explained
- [Deployment Checklist](./DEPLOYMENT_CHECKLIST.md) - Verification steps
- [Summary](./SUMMARY.md) - Project overview

---

## 🐛 Common Issues

### Build Fails
```bash
# Test locally first
cd apps/docs
pnpm install
pnpm build-docs

# Check error messages
# Fix any TypeScript/build errors
```

### 404 Errors
```
Check:
- basePath in next.config.ts
- output: 'export' is set (if needed)
- Clear browser cache
```

### Permissions Error
```
Go to: Settings → Actions → General
Enable: "Read and write permissions"
```

---

## ✅ Pre-Flight Checklist

Before deployment:
- [ ] GitHub Pages enabled in settings
- [ ] Workflow permissions configured
- [ ] Build succeeds locally
- [ ] All tests pass
- [ ] Documentation reviewed

---

## 📊 What Gets Deployed

```
apps/docs/
├── Build output from Next.js
├── Static HTML pages
├── Optimized assets
└── Generated from MDX content
```

---

## 🎯 Expected Results

After successful deployment:

- ✅ Site accessible at GitHub Pages URL
- ✅ All pages load correctly
- ✅ Navigation works
- ✅ Images display
- ✅ Styles applied
- ✅ Mobile responsive

---

## 🔄 Workflow Process

```
1. Commit pushed to canary/main
2. Workflow triggered
3. Environment setup (Node.js, pnpm)
4. Dependencies installed
5. Documentation built
6. Artifact uploaded
7. Deployed to GitHub Pages
8. URL available in workflow output
```

**Total Time:** ~3-5 minutes

---

## 💡 Tips

- **First deployment** takes longer (no cache)
- **Subsequent deploys** are faster (cached dependencies)
- **Manual trigger** available in Actions tab
- **Monitor in real-time** via Actions page
- **Check logs** for detailed error messages

---

## 📞 Support

**Issues?** Check:
1. [Troubleshooting section](./GITHUB_PAGES_SETUP.md#troubleshooting)
2. Workflow logs in Actions
3. [GitHub Pages documentation](https://docs.github.com/en/pages)
4. Open issue if needed

---

## 🎉 Success Indicators

✅ Workflow badge shows green  
✅ Deployment URL accessible  
✅ All pages load correctly  
✅ No console errors  
✅ Mobile responsive  

---

## 📝 Quick Notes

- **Node Version:** 20
- **Package Manager:** pnpm 9.6.0
- **Framework:** Next.js 15.5.3 + fumadocs
- **Build Time:** 2-3 minutes
- **Deploy Time:** 1-2 minutes

---

## 🔐 Security

- HTTPS enabled by default
- No sensitive data in public files
- Workflow permissions configured correctly
- Secrets managed via GitHub Secrets

---

**Need more details?** See full documentation in [GITHUB_PAGES_SETUP.md](./GITHUB_PAGES_SETUP.md)

**Ready to deploy?** Follow the 5-Minute Setup above ⬆️
