# Course Deployment Guide

## How to Build the Website
1.  Open your terminal in this folder.
2.  Run the command: `quarto render`
3.  A folder named `_site` will be created. This contains your HTML website.

## Option A: Publish to Netlify (Easiest)
1.  Go to [Netlify Drop](https://app.netlify.com/drop).
2.  Drag and drop the `_site` folder onto the page.
3.  Your course is now live! You can change the URL in "Site Settings".

## Option B: Publish to GitHub Pages
1.  Create a new repository on GitHub.
2.  Push this entire project code to the repository.
3.  Go to Settings > Pages.
4.  Select "GitHub Actions" as the source (Quarto has a built-in action) OR configure it to serve from the `/docs` folder if you change the output directory in `_quarto.yml`.