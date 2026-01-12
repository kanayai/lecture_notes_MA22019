# Text Analysis Project Setup

## Prerequisites

- [R](https://www.r-project.org/) with `reticulate` package
- [uv](https://github.com/astral-sh/uv) for Python environment management
- [Quarto](https://quarto.org/)

---

## Python Environment Setup

### 1. Create Virtual Environment

```bash
cd /path/to/text_analysis
uv venv
```

This creates a `.venv` folder with an isolated Python 3.12 installation.

### 2. Install Python Packages

```bash
uv pip install nltk gensim spacy
```

### 3. Install spaCy Language Model

Since `uv` doesn't include `pip`, install the model directly from the release URL:

```bash
uv pip install en_core_web_sm@https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.8.0/en_core_web_sm-3.8.0-py3-none-any.whl
```

### 4. Download NLTK Resources

Download tokenizers, stopwords, and lexicons into the project's `.venv/nltk_data`:

```bash
.venv/bin/python -m nltk.downloader -d .venv/nltk_data punkt_tab stopwords wordnet vader_lexicon
```

---

## R/Quarto Configuration

### `.Rprofile`

Create a `.Rprofile` file in the project root so R and Quarto can find the Python environment:

```r
Sys.setenv(RETICULATE_PYTHON = "/absolute/path/to/text_analysis/.venv/bin/python")
Sys.setenv(NLTK_DATA = "/absolute/path/to/text_analysis/.venv/nltk_data")
```

> **Note:** Use absolute paths. Relative paths fail because Quarto may render from different directories.

After creating/editing `.Rprofile`, restart your R session.

### Verify Configuration

In R, run:

```r
library(reticulate)
py_config()
```

You should see the Python path pointing to `.venv/bin/python`.

---

## Sharing Data Between Quarto Documents

Each `.qmd` file renders independently—variables don't persist. Use RDS files to share objects.

### In the source module (e.g., `m1-foundations.qmd`):

```r
saveRDS(tidy_books, here::here("data", "tidy_books.rds"))
```

### In downstream modules (e.g., `m2-preprocessing.qmd`):

```r
tidy_books <- readRDS(here::here("data", "tidy_books.rds"))
```

`here::here()` constructs paths relative to the project root regardless of the working directory.

---

## Project Structure

```
text_analysis/
├── .Rprofile                 # R config: Python path, NLTK_DATA
├── .venv/                    # Python virtual environment
│   ├── bin/python
│   └── nltk_data/            # NLTK resources
│       ├── tokenizers/
│       ├── corpora/
│       └── sentiment/
├── data/
│   └── tidy_books.rds        # Shared data between modules
├── modules/
│   ├── m1-foundations.qmd    # Creates & saves tidy_books
│   ├── m2-preprocessing.qmd  # Loads tidy_books
│   ├── m3-topic-modeling.qmd
│   └── m4-inference.qmd
├── solutions/
├── _quarto.yml
└── index.qmd
```

---

## Rendering

```bash
quarto render
```

Or render a single module:

```bash
quarto render modules/m1-foundations.qmd
```

---

## Troubleshooting

| Error | Solution |
|-------|----------|
| `Python specified in RETICULATE_PYTHON does not exist` | Use absolute path in `.Rprofile` |
| `Resource punkt_tab not found` | Run `nltk.downloader` with `-d .venv/nltk_data` |
| `ModuleNotFoundError: No module named 'gensim'` | Run `uv pip install gensim` |
| `object 'tidy_books' not found` | Add `readRDS(here::here("data", "tidy_books.rds"))` to the module |
| `No module named pip` (when downloading spaCy model) | Install model via `uv pip install` with direct URL |


##  Deployment Guide 



### How to Build the Website
1.  Open your terminal in this folder.
2.  Run the command: `quarto render`
3.  A folder named `_site` will be created. This contains your HTML website.

#### Option A: Publish to Netlify (Easiest)
1.  Go to [Netlify Drop](https://app.netlify.com/drop).
2.  Drag and drop the `_site` folder onto the page.
3.  Your course is now live! You can change the URL in "Site Settings".

#### Option B: Publish to GitHub Pages
1.  Create a new repository on GitHub.
2.  Push this entire project code to the repository.
3.  Go to Settings > Pages.
4.  Select "GitHub Actions" as the source (Quarto has a built-in action) OR configure it to serve from the `/docs` folder if you change the output directory in `_quarto.yml`.
