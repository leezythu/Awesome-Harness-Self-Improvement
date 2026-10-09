# Star History Design

## Goal

Add an automatically updating GitHub star trend chart to both English and Chinese READMEs.

## Design

- Add a table-of-contents entry in each README.
- Add a final `Star History` / `Star 趋势` section after the citation.
- Use the token-free `star-history.dera.page` dynamic SVG.
- Link the chart to the matching interactive Star History page.

## Verification

- Confirm both SVG and interactive URLs target `leezythu/Awesome-Harness-Self-Improvement`.
- Confirm the Markdown renders identically in both README variants.
