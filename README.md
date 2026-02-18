# Search Highlight Demo

Demo site for [quarto-dev/quarto-cli#14047](https://github.com/quarto-dev/quarto-cli/issues/14047) — search highlights cleared by layout-settling events before users could see them.

## The Bug

When navigating to a Quarto page with `?q=term` in the URL (e.g., from clicking a search result), the matching text should be highlighted with `<mark>` elements. However, layout-settling events (`quarto-hrChanged` and `quarto-sectionChanged`) fired within ~24ms of page load and triggered code that cleared those highlights before they were visible.

## The Fix

[PR #14049](https://github.com/quarto-dev/quarto-cli/pull/14049) delays the registration of highlight-clearing event listeners by 1000ms, allowing initial layout events to settle before the listeners become active.

## Live Demo

**Site:** <https://cderv.github.io/quarto-search-highlight-demo/>

Test search highlighting by navigating directly to these URLs:

- [pandas.html?q=data](https://cderv.github.io/quarto-search-highlight-demo/pandas.html?q=data)
- [visualization.html?q=chart](https://cderv.github.io/quarto-search-highlight-demo/visualization.html?q=chart)
- [modeling.html?q=regression](https://cderv.github.io/quarto-search-highlight-demo/modeling.html?q=regression)

Or use the search box in the navbar — click a result and the destination page should show highlighted matches.
