# HTML Elements - Accessibility Properties Reference

## Document Structure Elements

| Element     | Accessibility Properties              | Notes                                                                                                            |
| ----------- | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `<html>`    | `lang`                                | **Required.** Specifies document language (e.g., `lang="en"`). Helps screen readers pronounce content correctly. |
| `<title>`   | Text content                          | **Required.** Page title announced by screen readers. Should be unique and descriptive.                          |
| `<main>`    | `id`, `aria-label`, `aria-labelledby` | Landmark for main content. Should be unique on page. Use `role="main"` for older browsers.                       |
| `<header>`  | `aria-label`, `aria-labelledby`       | Landmark for header content. Multiple allowed if labeled distinctly.                                             |
| `<footer>`  | `aria-label`, `aria-labelledby`       | Landmark for footer content. Multiple allowed if labeled distinctly.                                             |
| `<nav>`     | `aria-label`, `aria-labelledby`       | **Important landmark.** Label multiple navs (e.g., "Primary", "Footer").                                         |
| `<aside>`   | `aria-label`, `aria-labelledby`       | Landmark for complementary content. Label if multiple exist.                                                     |
| `<section>` | `aria-label`, `aria-labelledby`       | Becomes landmark only when labeled. Use for distinct page regions.                                               |
| `<article>` | `aria-label`, `aria-labelledby`       | Self-contained content. May contain headings and own structure.                                                  |

---

## Headings & Text

| Element         | Accessibility Properties         | Notes                                                                                                        |
| --------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `<h1>` - `<h6>` | Text content, `id`, `aria-label` | **Critical for navigation.** Maintain logical hierarchy. Don't skip levels. One `<h1>` per page recommended. |
| `<p>`           | `lang`, `id`, `aria-label`       | Standard text. Use `lang` if paragraph differs from document language.                                       |
| `<blockquote>`  | `cite`, `lang`                   | Quote attribution via `cite` attribute (URL) or nested `<cite>` element.                                     |
| `<cite>`        | Text content                     | Reference to creative work. Announced as citation by some screen readers.                                    |
| `<abbr>`        | `title`                          | **Important.** Full term in `title` attribute (e.g., `<abbr title="HyperText Markup Language">HTML</abbr>`). |
| `<time>`        | `datetime`                       | Machine-readable date/time. Helps assistive tech understand temporal content.                                |
| `<code>`        | `lang`                           | Inline code. May need `lang` for non-English code.                                                           |
| `<pre>`         | `aria-label`, `tabindex="0"`     | Preformatted text. May need scrolling, so consider keyboard access.                                          |

---

## Lists

| Element | Accessibility Properties                             | Notes                                                           |
| ------- | ---------------------------------------------------- | --------------------------------------------------------------- |
| `<ul>`  | `aria-label`, `aria-labelledby`                      | Unordered list. Screen readers announce list and item count.    |
| `<ol>`  | `aria-label`, `aria-labelledby`, `start`, `reversed` | Ordered list. Numbering information conveyed to screen readers. |
| `<li>`  | `value`                                              | List item. Screen readers announce position in list.            |
| `<dl>`  | `aria-label`, `aria-labelledby`                      | Description list. Groups `<dt>`/`<dd>` pairs.                   |
| `<dt>`  | Text content                                         | Term in description list. Associated with following `<dd>`.     |
| `<dd>`  | Text content                                         | Description in list. Connected to preceding `<dt>`.             |

---

## Links & Navigation

| Element | Accessibility Properties                                                                                                      | Notes                                                                                                                                                                                                                                                                           |
| ------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<a>`   | `href`, `aria-label`, `aria-labelledby`, `aria-describedby`, `aria-current`, `title`, `hreflang`, `download`, `target`, `rel` | **Critical element.** `href` makes it focusable/keyboard-accessible. Link text should be descriptive. Use `aria-label` to override text. `aria-current="page"` for current page. `target="_blank"` needs `rel="noopener"` + warning. `title` provides additional info on hover. |

---

## Buttons & Interactive Elements

| Element     | Accessibility Properties                                                                                                                   | Notes                                                                                                                                                                                                             |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<button>`  | `type`, `disabled`, `aria-label`, `aria-labelledby`, `aria-describedby`, `aria-pressed`, `aria-expanded`, `aria-controls`, `aria-haspopup` | **Preferred for interactions.** `type="button"` (default action), `type="submit"` (form), `type="reset"`. `disabled` removes from tab order. `aria-pressed` for toggles. `aria-expanded` for collapsible content. |
| `<details>` | `open`                                                                                                                                     | Native disclosure widget. Keyboard-accessible by default. `open` attribute controls visibility.                                                                                                                   |
| `<summary>` | Text content, `aria-label`                                                                                                                 | Clickable heading for `<details>`. First child becomes toggle button.                                                                                                                                             |
| `<dialog>`  | `open`, `aria-label`, `aria-labelledby`, `aria-describedby`, `aria-modal`                                                                  | Native modal/dialog. Use `.showModal()` for proper focus trap. `aria-modal="true"` recommended.                                                                                                                   |

---

## Forms & Inputs

| Element      | Accessibility Properties                                                                                                                                                                                                                                                     | Notes                                                                                                                                                                                                                                                                                                                                                               |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<form>`     | `aria-label`, `aria-labelledby`, `name`, `novalidate`                                                                                                                                                                                                                        | Form container. Label if multiple forms on page.                                                                                                                                                                                                                                                                                                                    |
| `<label>`    | `for`                                                                                                                                                                                                                                                                        | **Critical.** Associates text with input via `id`. Or wrap input inside label.                                                                                                                                                                                                                                                                                      |
| `<input>`    | `type`, `id`, `name`, `required`, `aria-required`, `disabled`, `readonly`, `aria-invalid`, `aria-describedby`, `aria-errormessage`, `placeholder`, `autocomplete`, `aria-label`, `aria-labelledby`, `checked`, `value`, `min`, `max`, `step`, `pattern`, `inputmode`, `list` | **Most complex element.** `type` determines behavior. Always need associated `<label>`. `required`/`aria-required` for mandatory fields. `aria-invalid="true"` on validation errors. `aria-describedby` links to help/error text. `autocomplete` critical for autofill. `placeholder` is NOT a label replacement. `disabled` removes from tab order and submission. |
| `<textarea>` | `id`, `name`, `required`, `aria-required`, `disabled`, `readonly`, `aria-invalid`, `aria-describedby`, `aria-errormessage`, `placeholder`, `autocomplete`, `aria-label`, `aria-labelledby`, `rows`, `cols`, `maxlength`, `minlength`                                         | Multiline text input. Same accessibility properties as `<input>`.                                                                                                                                                                                                                                                                                                   |
| `<select>`   | `id`, `name`, `required`, `aria-required`, `disabled`, `aria-invalid`, `aria-describedby`, `aria-errormessage`, `aria-label`, `aria-labelledby`, `multiple`, `size`, `autocomplete`                                                                                          | Dropdown/listbox. Needs `<label>`. `multiple` allows multi-select.                                                                                                                                                                                                                                                                                                  |
| `<option>`   | `value`, `selected`, `disabled`, `label`                                                                                                                                                                                                                                     | Option in `<select>`. Text content or `label` attribute provides display text.                                                                                                                                                                                                                                                                                      |
| `<optgroup>` | `label`, `disabled`                                                                                                                                                                                                                                                          | Groups options. `label` provides group name (announced by screen readers).                                                                                                                                                                                                                                                                                          |
| `<fieldset>` | `disabled`, `aria-describedby`                                                                                                                                                                                                                                               | **Important for grouping.** Groups related inputs (radio buttons, checkboxes). `disabled` affects all nested inputs.                                                                                                                                                                                                                                                |
| `<legend>`   | Text content                                                                                                                                                                                                                                                                 | **Critical.** First child of `<fieldset>`. Provides group name/question.                                                                                                                                                                                                                                                                                            |
| `<datalist>` | `id`                                                                                                                                                                                                                                                                         | Provides autocomplete suggestions for `<input>`. Linked via `list` attribute.                                                                                                                                                                                                                                                                                       |
| `<output>`   | `for`, `aria-live`, `aria-atomic`, `name`                                                                                                                                                                                                                                    | Displays calculation result. `for` references input IDs used in calculation. Acts as live region.                                                                                                                                                                                                                                                                   |
| `<progress>` | `value`, `max`, `aria-label`, `aria-labelledby`, `aria-describedby`, `aria-valuenow`, `aria-valuemin`, `aria-valuemax`                                                                                                                                                       | Progress indicator. Screen readers announce percentage. Indeterminate if no `value`.                                                                                                                                                                                                                                                                                |
| `<meter>`    | `value`, `min`, `max`, `low`, `high`, `optimum`, `aria-label`, `aria-labelledby`                                                                                                                                                                                             | Gauge/measurement. Thresholds (`low`, `high`, `optimum`) provide semantic meaning.                                                                                                                                                                                                                                                                                  |

---

## Tables

| Element      | Accessibility Properties                                    | Notes                                                                                                                                                                                                                     |
| ------------ | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<table>`    | `aria-label`, `aria-labelledby`, `aria-describedby`, `role` | **Data tables need structure.** Use `<caption>` or `aria-label` for title. Avoid for layout (use CSS instead).                                                                                                            |
| `<caption>`  | Text content                                                | **Recommended.** Table title/summary. First child of `<table>`. Always announced by screen readers.                                                                                                                       |
| `<thead>`    | None specific                                               | Groups header rows. Helps screen readers distinguish headers.                                                                                                                                                             |
| `<tbody>`    | None specific                                               | Groups body rows. Helps screen readers understand table structure.                                                                                                                                                        |
| `<tfoot>`    | None specific                                               | Groups footer rows (totals, summaries).                                                                                                                                                                                   |
| `<tr>`       | None specific                                               | Table row. Context provided by position and nested `<th>`/`<td>`.                                                                                                                                                         |
| `<th>`       | `scope`, `headers`, `abbr`, `colspan`, `rowspan`            | **Critical for data tables.** `scope="col"` (column header), `scope="row"` (row header), `scope="colgroup"`, `scope="rowgroup"`. `headers` links to `<th>` IDs for complex tables. `abbr` provides shortened header text. |
| `<td>`       | `headers`, `colspan`, `rowspan`                             | Table data cell. `headers` attribute references `<th>` IDs for complex tables.                                                                                                                                            |
| `<colgroup>` | `span`                                                      | Groups columns for styling. Limited accessibility impact.                                                                                                                                                                 |
| `<col>`      | `span`                                                      | Defines column properties. Limited accessibility impact.                                                                                                                                                                  |

---

## Media Elements

| Element     | Accessibility Properties                                                                       | Notes                                                                                                                                                                                              |
| ----------- | ---------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<img>`     | `alt`, `aria-label`, `aria-labelledby`, `aria-describedby`, `role`, `loading`, `longdesc`      | **`alt` is critical.** Describe image content/function. Decorative images: `alt=""` (not missing). Complex images: use `aria-describedby` or `longdesc`. `role="img"` for SVGs.                    |
| `<svg>`     | `role="img"`, `aria-label`, `aria-labelledby`, `aria-describedby`, `focusable`                 | Use `role="img"` for accessibility. `<title>` (first child) provides name. `<desc>` provides description. `focusable="false"` prevents tab focus in IE.                                            |
| `<canvas>`  | `aria-label`, `aria-describedby`, `role`                                                       | **Inherently inaccessible.** Provide fallback content inside `<canvas>` tags. Describe visual content via `aria-label` or text alternative.                                                        |
| `<audio>`   | `controls`, `autoplay`, `loop`, `muted`, `aria-label`, `aria-describedby`, `preload`           | **`controls` essential for accessibility.** Provide transcript. Avoid `autoplay`. Caption audio if contains speech.                                                                                |
| `<video>`   | `controls`, `autoplay`, `loop`, `muted`, `poster`, `aria-label`, `aria-describedby`, `preload` | **`controls` essential.** Provide captions (`<track kind="captions">`) and audio description. Avoid `autoplay` with sound.                                                                         |
| `<track>`   | `kind`, `src`, `srclang`, `label`, `default`                                                   | **Critical for video accessibility.** `kind="subtitles"` (translations), `kind="captions"` (sound descriptions), `kind="descriptions"` (audio descriptions), `kind="chapters"`, `kind="metadata"`. |
| `<source>`  | `src`, `type`, `media`                                                                         | Provides alternative media formats. No direct accessibility impact.                                                                                                                                |
| `<picture>` | None specific                                                                                  | Responsive image container. Accessibility depends on `<img>` child.                                                                                                                                |
| `<map>`     | `name`, `aria-label`                                                                           | Image map. Coordinate with `<area>` elements.                                                                                                                                                      |
| `<area>`    | `alt`, `href`, `shape`, `coords`, `aria-label`, `aria-describedby`                             | **`alt` required.** Describes clickable region of image map. Keyboard-accessible when `href` present.                                                                                              |

---

## Embedded Content

| Element    | Accessibility Properties                                                        | Notes                                                                                               |
| ---------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| `<iframe>` | `title`, `aria-label`, `aria-labelledby`, `aria-describedby`, `sandbox`, `name` | **`title` required.** Describes iframe content/purpose. Screen readers announce iframe and title.   |
| `<embed>`  | `type`, `aria-label`, `aria-describedby`                                        | Embeds external content. Provide alternative content or description. Limited accessibility support. |
| `<object>` | `type`, `data`, `aria-label`, `aria-describedby`                                | Embeds external resource. Provide fallback content between tags.                                    |

---

## Scripting & Dynamic Content

| Element      | Accessibility Properties | Notes                                                                                     |
| ------------ | ------------------------ | ----------------------------------------------------------------------------------------- |
| `<script>`   | `type`, `async`, `defer` | No direct accessibility properties. Affects page behavior. Use `<noscript>` for fallback. |
| `<noscript>` | Text content             | Fallback for users without JavaScript. Ensure core functionality accessible.              |
| `<template>` | None specific            | Inert content template. No accessibility impact until cloned into document.               |
| `<slot>`     | `name`                   | Web component slot. Accessibility depends on slotted content.                             |

---

## Deprecated/Rare Elements (Use with Caution)

| Element                 | Accessibility Properties  | Notes                                                                           |
| ----------------------- | ------------------------- | ------------------------------------------------------------------------------- |
| `<marquee>`             | `aria-live`, `aria-label` | **Deprecated.** Avoid. Can cause motion sickness. If used, acts as live region. |
| `<blink>`               | None                      | **Deprecated.** Avoid. Inaccessible and distracting.                            |
| `<frame>`, `<frameset>` | `title`, `name`           | **Deprecated.** Avoid. Poor accessibility. Use modern layout techniques.        |

---

## Universal Accessibility Attributes (Apply to Most Elements)

| Attribute          | Applies To    | Description                                                                                                        |
| ------------------ | ------------- | ------------------------------------------------------------------------------------------------------------------ |
| `id`               | All elements  | Unique identifier. Used for labels, descriptions, and ARIA references.                                             |
| `class`            | All elements  | CSS classes. No direct accessibility impact, but affects presentation.                                             |
| `lang`             | All elements  | Specifies content language. Overrides document language. Critical for screen readers.                              |
| `dir`              | All elements  | Text direction: `ltr` (left-to-right), `rtl` (right-to-left). Important for multilingual content.                  |
| `title`            | Most elements | Tooltip text. Avoid as primary label (not consistently accessible). Use `aria-label` instead.                      |
| `tabindex`         | All elements  | Controls keyboard focus order. `0` = natural order, `-1` = programmatic only, `>0` = bad practice.                 |
| `hidden`           | All elements  | Completely removes from accessibility tree and display. Use for truly hidden content.                              |
| `aria-hidden`      | All elements  | Removes from accessibility tree only (still visible). Use for decorative content. **Never on focusable elements.** |
| `role`             | All elements  | Overrides native semantics. Use sparingly. Prefer native HTML.                                                     |
| `aria-label`       | Most elements | Provides accessible name. Overrides visible text. Not supported on `<div>`, `<span>` without roles.                |
| `aria-labelledby`  | Most elements | References element(s) providing accessible name. Can reference multiple IDs (space-separated).                     |
| `aria-describedby` | Most elements | References element(s) providing additional description. Announced after name and role.                             |
| `aria-live`        | All elements  | Creates live region: `off`, `polite`, `assertive`. For dynamic content updates.                                    |
| `aria-atomic`      | Live regions  | If `true`, announces entire region. If `false`, only changed content.                                              |
| `aria-relevant`    | Live regions  | What changes to announce: `additions`, `removals`, `text`, `all`.                                                  |

---

## Key Accessibility Principles by Element Category

### Forms

- Every input needs an associated `<label>` (via `for` or wrapping)
- Use `aria-invalid` and `aria-describedby` for error states
- `required` attribute makes field mandatory
- `disabled` fields are not submitted and removed from tab order
- `autocomplete` attribute improves user experience significantly

### Interactive Elements

- `<button>` is preferred over `<div>` with `role="button"`
- All interactive elements must be keyboard-accessible
- Provide clear focus indicators
- Use semantic HTML before adding ARIA roles

### Media

- Images require `alt` text (empty for decorative)
- Videos need captions and controls
- Audio needs transcripts
- Don't autoplay media with sound

### Structure

- Use semantic landmarks (`<nav>`, `<main>`, `<aside>`)
- Maintain heading hierarchy
- One `<main>` per page
- Label landmarks when multiples exist

### Tables

- Use `<th>` with `scope` for headers
- Provide `<caption>` for table title
- Use `<thead>`, `<tbody>`, `<tfoot>` for structure
- Avoid tables for layout

---

## Testing Checklist

1. ✅ All images have appropriate `alt` text
2. ✅ All form inputs have associated labels
3. ✅ All interactive elements are keyboard-accessible
4. ✅ Heading hierarchy is logical (no skipped levels)
5. ✅ Page has meaningful `<title>`
6. ✅ Document `lang` attribute is set
7. ✅ Color contrast meets WCAG standards
8. ✅ Focus indicators are visible
9. ✅ Tables use proper header structure
10. ✅ Videos have captions and controls
11. ✅ ARIA attributes are used correctly (not overriding native semantics)
12. ✅ Tested with keyboard only
13. ✅ Tested with screen reader
