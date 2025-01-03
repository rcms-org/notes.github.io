# My Project on GitHub Pages Deployment

This repository hosts a project that is served via **GitHub Pages**. Below is a description of the workflow and steps involved in deploying the site.

## Table of Contents

1. GitHub Pages Setup
2. [Custom Domain Setup](#custom-domain-setup)
3. [DNS Settings for Custom Domain](#dns-settings-for-custom-domain)
4. [Automated Deployment with GitHub Actions](#automated-deployment-with-github-actions)
5. [Verify the Website](#verify-the-website)

## GitHub Pages Setup

### Steps to Enable GitHub Pages

1. Go to the **Settings** tab in your GitHub repository.
2. Scroll down to the **GitHub Pages** section.
3. Select the branch (usually `main`) as the source for the GitHub Pages site.
4. GitHub will automatically generate a URL: `https://your-username.github.io/your-repository-name/`.

## Custom Domain Setup

If you want to use a **custom domain** (e.g., `www.yourdomain.com`), you need to:

1. Create a `CNAME` file in the root of the repository.
2. Add your custom domain to the `CNAME` file (e.g., `www.yourdomain.com`).
3. Update your **DNS settings** to point to GitHub Pages.

### DNS Settings for Custom Domain

1. Log in to your **GoDaddy** account (or another domain provider).
2. For **A Records**:
   - Add the following four IPs:

     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```

3. For **CNAME Record**:
   - Add a CNAME record for the `sub-domain-name` subdomain pointing to valuve `your-username.github.io` .

4. Wait for DNS propagation.
-----------
## Automated Static Deployment with GitHub Actions

To automatically deploy your website when changes are pushed to your repository, create a `.github/workflows/deploy.yml` file with the following content:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Set up Node.js (if needed)
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Build site
        run: npm run build

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          personal_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./build
