# Statistics 224 Documentation - MkDocs Site

This is a professional documentation website for your Statistics 224 course, built with MkDocs Material.

## What's Included

- **Complete Stats Doc** converted to organized Markdown
- **Professional theme** with search, navigation, code highlighting
- **Mobile-responsive** design
- **Direct integration** with your interactive modules
- **Easy updates** via Markdown files

## Structure

```
mkdocs_site/
├── mkdocs.yml          # Site configuration
├── docs/               # All content pages
│   ├── index.md        # Homepage
│   ├── quick-start/    # Getting started guides
│   ├── which-test/     # Decision tree and scenarios
│   ├── effect-sizes/   # Effect size reference
│   ├── modules/        # Interactive modules info
│   ├── parametric/     # T-tests, ANOVA, Regression
│   ├── non-parametric/ # Non-parametric tests
│   ├── chi-square/     # Chi-square tests
│   └── resources/      # Datasets, workflows, archives
└── README.md           # This file
```

## Deployment Options

### Option 1: GitHub Pages (Recommended)

**Step 1: Create Repository**
```bash
cd mkdocs_site
git init
git add .
git commit -m "Initial MkDocs site"
```

**Step 2: Create GitHub Repo**
- Go to github.com
- Create new repository: "stats-doc"
- Push your code:

```bash
git remote add origin https://github.com/jkoxford-a11y/stats-doc.git
git branch -M main
git push -u origin main
```

**Step 3: Deploy**
```bash
mkdocs gh-deploy
```

Your site will be live at: `https://jkoxford-a11y.github.io/stats-doc/`

### Option 2: Local Preview

**Install MkDocs:**
```bash
pip install mkdocs-material
```

**Run locally:**
```bash
cd mkdocs_site
mkdocs serve
```

Visit: `http://localhost:8000`

## Updating Content

### Easy Method (GitHub Web)
1. Go to your repo on GitHub
2. Navigate to any `.md` file in `docs/`
3. Click "Edit this file" (pencil icon)
4. Make changes in browser
5. Commit changes
6. Site updates automatically!

### Advanced Method (Local)
1. Edit `.md` files in `docs/` folder
2. Preview with `mkdocs serve`
3. Commit and push to GitHub
4. Run `mkdocs gh-deploy`

## Adding New Pages

1. Create new `.md` file in appropriate `docs/` subdirectory
2. Add entry to `mkdocs.yml` under `nav:` section
3. Deploy changes

## Customization

### Change Colors
Edit `mkdocs.yml`:
```yaml
theme:
  palette:
    primary: indigo  # Change this
    accent: indigo   # And this
```

Colors: red, pink, purple, indigo, blue, cyan, teal, green, lime, yellow, amber, orange, deep-orange

### Change Site Name/URL
Edit top of `mkdocs.yml`:
```yaml
site_name: Your Course Name
site_url: https://your-url.github.io/stats-doc/
```

## Features Included

✓ Search functionality
✓ Dark/light mode toggle
✓ Mobile-responsive
✓ Code syntax highlighting
✓ Copy code buttons
✓ Navigation tabs
✓ MathJax support (for equations)
✓ Admonitions (info boxes)
✓ Mermaid diagrams

## Support

- [MkDocs Documentation](https://www.mkdocs.org/)
- [Material Theme Guide](https://squidfunk.github.io/mkdocs-material/)

## Next Steps

1. Review content in `docs/` folder
2. Test locally with `mkdocs serve`
3. Deploy to GitHub Pages
4. Share URL with students!

Your documentation website is ready to go! 🚀
