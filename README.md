# Search Highlight Demo

Demo site for [quarto-dev/quarto-cli#14047](https://github.com/quarto-dev/quarto-cli/issues/14047) — search highlights cleared by layout-settling events before users could see them.

## The Bug

When navigating to a Quarto page with `?q=term` in the URL (e.g., from clicking a search result), the matching text should be highlighted with `<mark>` elements. However, layout-settling events (`quarto-hrChanged` and `quarto-sectionChanged`) fired within ~24ms of page load and triggered code that cleared those highlights before they were visible.

## The Fix

[PR #14049](https://github.com/quarto-dev/quarto-cli/pull/14049) delays the registration of highlight-clearing event listeners by 1000ms, allowing initial layout events to settle before the listeners become active.

## Search & Tab Activation

[PR #14053](https://github.com/quarto-dev/quarto-cli/pull/14053) automatically activates tabs containing matching content. When a search result points to content inside an inactive Bootstrap tab, the tab is activated so the highlighted match is visible. This works for ungrouped tabs, grouped tabs (overriding localStorage preferences), and nested tabsets.

## Live Demo

**Site:** <https://cderv.github.io/quarto-search-highlight-demo/>

### Search highlights

- [pandas.html?q=data](https://cderv.github.io/quarto-search-highlight-demo/pandas.html?q=data)
- [visualization.html?q=chart](https://cderv.github.io/quarto-search-highlight-demo/visualization.html?q=chart)
- [modeling.html?q=regression](https://cderv.github.io/quarto-search-highlight-demo/modeling.html?q=regression)

### Tab activation

- [Inactive tab activation](https://cderv.github.io/quarto-search-highlight-demo/tabsets.html?q=beta-unique-search-term) — activates Tab Beta
- [Nested tabs](https://cderv.github.io/quarto-search-highlight-demo/tabsets.html?q=nested-inner-only-term) — activates both outer and inner tabs
- [Grouped tab override](https://cderv.github.io/quarto-search-highlight-demo/tabsets.html?q=python-only-content) — Python activates despite R preference

Or use the search box in the navbar — click a result and the destination page should show highlighted matches.
