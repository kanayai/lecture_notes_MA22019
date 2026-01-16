# Text Analysis Project Setup

## Prerequisites

- [R](https://www.r-project.org/)
- [Quarto](https://quarto.org/)
- **Recommended Packages:** `tidyverse`, `tidytext`, `topicmodels`, `textstem`, `gutenbergr`, `ggraph`

---

## Project Structure

```
text_analysis/
├── data/
│   └── tidy_books.rds        # Shared data between modules
├── modules/
│   ├── m1-foundations.qmd    # Creates & saves tidy_books
│   ├── m2-preprocessing.qmd  # Loads tidy_books
│   ├── m3-topic-modeling.qmd
│   ├── m4-inference.qmd
├── solutions/
│   └── solutions.qmd
├── _quarto.yml
└── index.qmd
```

## Setup

1. Open the project in RStudio or VS Code.
2. Ensure you have the required packages installed:

```r
install.packages(c("tidyverse", "tidytext", "topicmodels", "textstem", "gutenbergr", "ggraph", "here"))
```



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
| `object 'tidy_books' not found` | Add `readRDS(here::here("data", "tidy_books.rds"))` to the module |
| `there is no package called 'igraph'` | Run `install.packages("igraph")` |
| `Cannot create a graph object because the edge data frame contains NAs` | Ensure you `drop_na()` before `graph_from_data_frame()` |


## Deployment Guide 



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
