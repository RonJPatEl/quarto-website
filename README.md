# Updating the website

Open `quarto_personalwebsite.Rproj` in RStudio. Run the commands below in the
**Terminal** tab (not the R Console), from this project folder.

## Refresh locally, then deploy through GitHub

```sh
quarto render
git add _site index.qmd publications.qmd _quarto.yml
git commit -m "Update website publications and h-index"
git push origin main
```

Rendering fetches the latest public Zotero publication record and Google Scholar
h-index. The generated website is in `_site`, which is deliberately tracked in
Git. Pushing the updated build to `main` lets Netlify deploy it to
<https://ronjpat-el-website.netlify.app/>.

Check that rendering succeeds before committing. Use `git status` to review the
changes; include any other source files you intentionally edited. If Git says
there is nothing to commit, that usually means the rendered content has not
changed.

Netlify should use the GitHub repository `RonJPatEl/quarto-website`, production
branch `main`, publish directory `_site`, and no build command, since R runs
locally. You do not need a separate Netlify login for each Git push.

## Review before publishing

```sh
quarto render
quarto preview --no-render
```

Check the Home and Publications pages. Stop the preview with Ctrl+C, then commit
and push the build using the Git commands above.

## Alternative: publish directly with Quarto

The destination is also saved in `_publish.yml`, so you can upload an already
reviewed build directly:

```sh
quarto publish netlify --no-render
```

This route requires authorizing Quarto with the Netlify account that owns the
**Ron Pat-El** team (`ronjpatel`, linked to your GitHub account). Only use
`--no-render` after a successful render. A direct upload does not update GitHub;
a later GitHub deployment will use the files committed there.

Rendering alone never updates the live site. You must push the build to GitHub
or publish it directly to Netlify.

## Data and dependencies

- Publications come from Zotero user `6167652`, **My Publications**. Update that
  list and sync Zotero before rebuilding. The page fetches every result page.
- The h-index comes from Google Scholar profile `sAAmFv8AAAAJ`.
- Internet access is required. If either service fails, fix the error and
  successfully rebuild before publishing.
- Publication execution is deliberately not frozen or cached: changes in Zotero
  must be picked up even when `publications.qmd` has not changed.

If R reports a missing package, install the dependencies in the **R Console**:

```r
install.packages(c("knitr", "rmarkdown", "curl", "jsonlite", "stringr", "dplyr", "scholar"))
```

Official documentation: [Quarto publishing to Netlify](https://quarto.org/docs/publishing/netlify.html).
