# FairWeb AI: make the web accessible to everyone

A browser-based tool that helps developers find accessibility problems in a web page,
understand them in plain language, and fix them.

## Run it

No install, no build step. Unzip and open `index.html` in Chrome, Edge, Firefox or Safari.

To host it for free (for your hackathon submission), drag the folder into Netlify Drop,
or push it to a GitHub repo and turn on GitHub Pages.

## What it does

| Feature | Where |
| --- | --- |
| Check text contrast (real computed colors, inherited backgrounds, transparency) | Page checker |
| Find images with missing or useless alt text | Page checker |
| Detect form fields without labels, placeholder-only fields, ungrouped radios | Page checker |
| Spot keyboard problems: clickable divs, removed focus outlines, bad tabindex, unnamed links and buttons, autoplay | Page checker |
| Page structure: language, title, zoom blocking, headings, landmarks, tables, frames, duplicate ids | Page checker |
| Beginner-friendly explanation, who is affected, fix, copy-ready code, and "what a screen reader says" before and after | Every problem card |
| Report download (HTML or Markdown), copy summary, print or save as PDF | Bottom of results |
| Contrast lab with WCAG AA/AAA table and an automatic "closest passing color" | Contrast lab |
| The site itself: skip link, visible focus ring, high-contrast mode, larger text, reduce motion | Display options |

## How it works

1. Your HTML is rendered in a hidden, sandboxed iframe with scripts disabled. Nothing is uploaded.
2. Because the page really renders, the checker can read computed styles for contrast and text size.
3. A rule engine (`js/checks.js`) looks at the DOM and produces grouped issues.
4. Each rule carries a plain-language explanation, a fix and a code example (the `RULES` object).
5. `js/report.js` turns the result into a downloadable report.

If the browser blocks the iframe's document (rare), it falls back to parsing without layout,
and contrast checks are skipped with a note.

## Files

```
index.html        page structure
css/styles.css    design, themes (default and high contrast), print styles
js/checks.js      the checking engine and plain-language rule text
js/report.js      HTML / Markdown / summary report builders
js/app.js         UI wiring, results rendering, contrast lab
js/sample.js      the deliberately broken sample page
```

## Add a new rule

1. Add an entry to `RULES` in `js/checks.js` (severity, category, title, plain, why, fix, code).
2. Add a check inside `run()` that calls `add('your-rule-id', element)`.

## Honest limits

- The checks and explanations are rule-based. Nothing here calls an AI model.
  An easy next step is a small backend that sends each issue to an LLM for a custom explanation
  or an automatic code fix.
- Automated tools find some accessibility barriers, not all. Pair this with a keyboard test
  and a screen reader.
- The fonts load from Google Fonts (Atkinson Hyperlegible). Offline, the site falls back to system fonts.
- A pasted page is checked as-is. External CSS files and images are not fetched, so link or inline
  your CSS if you want the contrast check to see it.

## Roadmap

- Scan a live URL through a small proxy
- Browser extension
- CI check (fail a build when critical issues appear)
- LLM-written explanations and one-click fix suggestions
- More rules: color-only meaning, target size, text spacing, ARIA misuse
