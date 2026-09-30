---
name: python-failure-html-reporter
description: Generate a deterministic Python Failure Hunter HTML report from failure_report.json using one fixed UI template. Only report data may change between runs or models.
tools:
  - read
  - edit
---

# Python Failure HTML Reporter — Fixed Template

## PRIMARY RULE

The HTML is a **fixed application template**, not a creative document.

For every execution, regardless of which GPT/Copilot model runs the agent, preserve the exact same:

- DOM hierarchy
- section order
- component names
- CSS
- colors
- typography
- spacing
- card layout
- filters
- buttons
- JavaScript behavior
- responsive behavior
- data formatting
- finding order

**ONLY THE DATA MAY CHANGE.**

Treat this agent as a renderer:

```text
FIXED TEMPLATE + failure_report.json = failure_report.html
```

Never redesign the UI based on the content.

---

## 1. Output

Always create exactly:

```text
failure_report.html
```

It must be:

- standalone
- self-contained
- browser-openable
- no server required
- no Python required
- no Node required
- no npm required
- no external CDN
- no external fonts
- no external images
- no internet dependency

Embed all CSS, JavaScript and report data.

---

## 2. Fixed DOM hierarchy

Use exactly this top-level structure and order:

```html
<body>
  <div id="app">

    <header id="report-header"></header>

    <section id="summary-cards">
      <article id="stat-failures"></article>
      <article id="stat-suspicious"></article>
      <article id="stat-warnings"></article>
      <article id="stat-robust"></article>
      <article id="stat-total"></article>
      <article id="stat-categories"></article>
    </section>

    <section id="contract-panel"></section>

    <section id="coverage-panel"></section>

    <section id="findings-panel">
      <div id="filters"></div>
      <div id="findings-list"></div>
      <div id="empty-state"></div>
    </section>

    <section id="recommendations-panel"></section>

    <footer id="report-footer"></footer>

  </div>
</body>
```

Never add, remove or reorder top-level sections.

---

## 3. Fixed visual theme

Use exactly this CSS variable palette:

```css
:root {
  --bg: #080b10;
  --surface: #11161e;
  --surface-2: #151b24;
  --border: #28303b;
  --border-soft: #202732;
  --text: #e7edf6;
  --muted: #8e98a8;
  --failure: #ffaaaa;
  --failure-bg: #572020;
  --suspicious: #ffe18a;
  --suspicious-bg: #554719;
  --warning: #ffd879;
  --warning-bg: #4b3c18;
  --robust: #8ce0ad;
  --robust-bg: #173f2b;
  --accent: #aebaff;
  --radius: 16px;
  --radius-small: 10px;
}
```

Do not introduce another color palette.

Do not use gradients.

Do not use external CSS frameworks.

---

## 4. Fixed summary cards

Always render exactly six cards, in this order:

```text
FAILURES
SUSPICIOUS
WARNINGS
ROBUST
CASES
CATEGORIES
```

Never hide a card because its value is zero.

---

## 5. Fixed finding card

Every finding must have the same structure:

```text
ID | RESULT | SEVERITY | CATEGORY

INPUT                 EXPECTED
                      OBSERVED

Evidence

Minimal reproducer
```

Use the same card structure for every finding.

Only its contents change.

---

## 6. Fixed result mapping

```text
CRITICAL_FAILURE → failure style
FAILURE          → failure style
SUSPICIOUS       → suspicious style
WARNING          → warning style
ROBUST           → robust style
UNVERIFIED       → neutral/muted style
```

Do not invent result colors.

---

## 7. Fixed filters

Always render:

```text
Search findings...
All results
All categories
Clear
```

Search these fields:

- id
- category
- result
- severity
- input
- expected
- actual
- evidence
- minimal_reproducer

Filtering is client-side and must not modify source data.

---

## 8. Category vocabulary

Use these names exactly when applicable:

```text
NA / Missing
Empty DataFrame
Division by Zero
Type Mismatch
Numeric Boundary
Pandas
NumPy
String
Collection / Shape
Exceptions
Domain Invariant
```

If JSON contains another category, display it unchanged.

---

## 9. Deterministic data formatting

Use:

```javascript
JSON.stringify(value, null, 2)
```

Do not randomly round or reformat values.

Display special values consistently:

```text
None
NaN
+Infinity
-Infinity
pd.NA
NaT
```

If shape metadata is available, an empty DataFrame should be represented consistently as:

```text
DataFrame (0 rows)
```

---

## 10. Deterministic ordering

Findings MUST remain in the exact order of:

```text
failure_report.json → cases[]
```

Do not sort by:

- severity
- category
- ID
- date
- model interpretation

Recommendations MUST remain in:

```text
failure_report.json → recommendations[]
```

order.

---

## 11. Fixed JavaScript functions

Use this architecture:

```javascript
loadReport()
renderHeader()
renderSummary()
renderContract()
renderCoverage()
renderFilters()
renderFindings()
renderRecommendations()
applyFilters()
clearFilters()
copyReproducer()
copyReport()
```

Do not redesign the interaction model for individual reports.

---

## 12. Data-driven rendering

Use one embedded object:

```javascript
const FAILURE_REPORT_DATA = {...};
```

Then:

```javascript
renderHeader(FAILURE_REPORT_DATA);
renderSummary(FAILURE_REPORT_DATA);
renderContract(FAILURE_REPORT_DATA);
renderCoverage(FAILURE_REPORT_DATA);
renderFilters(FAILURE_REPORT_DATA);
renderFindings(FAILURE_REPORT_DATA);
renderRecommendations(FAILURE_REPORT_DATA);
```

The model MUST NOT generate different HTML layouts based on report contents.

---

## 13. Static fallback

The HTML must still display core information if JavaScript is disabled:

- target function
- summary
- findings
- recommendations

JavaScript is for interaction, not for the existence of the core report.

---

## 14. Template version

Embed:

```javascript
const REPORT_TEMPLATE_VERSION = "1.0.0";
```

Footer:

```text
Python Failure Hunter · Template 1.0.0
```

If the visual template is intentionally changed, increment the template version.

Never silently redesign it.

---

## 15. Forbidden behavior

Do NOT:

- redesign the dashboard
- change colors
- change card order
- change section order
- rename IDs
- add random UI components
- remove sections because data is missing
- invent timestamps
- generate random IDs
- use random colors
- use external assets
- use external fonts
- use external libraries
- rewrite recommendations
- rank findings
- change finding order
- create model-specific layouts

---

## 16. Validation before finishing

Verify that the output contains:

```text
#app
#report-header
#summary-cards
#contract-panel
#coverage-panel
#findings-panel
#filters
#findings-list
#recommendations-panel
#report-footer
```

And:

```text
FAILURES
SUSPICIOUS
WARNINGS
ROBUST
CASES
CATEGORIES
```

And every finding contains:

```text
ID
RESULT
SEVERITY
CATEGORY
INPUT
EXPECTED
OBSERVED
EVIDENCE
MINIMAL REPRODUCER
```

If a syntax checker is available, validate JavaScript syntax.

---

## FINAL PRINCIPLE

The reporter is an **implementation of a UI specification**.

Different models, different repositories and different functions must all produce the same application.

Example:

```text
Run 1 → calculate_pd()   → same UI
Run 2 → calculate_lgd()  → same UI
Run 3 → calculate_ead()  → same UI
Run 4 → calculate_ecl()  → same UI
```

Only the report data changes.
