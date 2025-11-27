# Front-End Accessibility Guidelines

## 1. General Guidelines

- Ensure all interactive elements are keyboard-accessible
- Use semantic HTML (`<button>`, `<a>`, `<header>`, `<main>`, `<nav>`, `<footer>`, `<article>`)
- Provide meaningful labels for form controls and icons
- Avoid empty `aria-label` on focusable elements
- Follow WCAG 2.2 principles: Perceivable, Operable, Understandable, Robust
- Maintain a logical heading hierarchy (`<h1>` → `<h2>` → `<h3>`, no skipping levels)
- Ensure minimum color contrast ratios: 4.5:1 for normal text, 3:1 for large text (18pt+)
- Support browser zoom up to 200% without loss of functionality
- Provide alternative text for all meaningful images (`alt` attribute)
- Use `lang` attribute on `<html>` tag and for any language changes within content
- Ensure touch targets are at least 44×44 pixels (WCAG 2.2 Level AA)
- Avoid using color alone to convey information

---

## 2. Inputs & Forms

- Use `<label>` for inputs; `<span>` for decorative text
- Always associate labels with inputs using `for` attribute or by wrapping
- Group related inputs with `<fieldset>` and `<legend>`
- Use `autocomplete` attributes for common fields (name, email, address, etc.)
- Provide clear, descriptive error messages that explain how to fix the issue
- Include visible focus indicators on all form controls
- Don't rely solely on placeholders—they're not replacements for labels

### ARIA Attributes for Forms

- `aria-required="true"` (or use HTML5 `required` attribute)
- `aria-invalid="true"` when validation fails
- `aria-describedby` links to error/help text element IDs
- `aria-errormessage` (WCAG 2.2) to reference error message IDs
- Use `aria-describedby` to associate persistent error messages with inputs
- Use `aria-live` regions for dynamic validation feedback that appears/updates during interaction

### Custom Form Controls

- For custom controls, use appropriate ARIA roles: `role="checkbox"`, `role="radio"`, `role="combobox"`, etc.
- Ensure all custom controls are keyboard-accessible with standard interactions

---

## 3. Live Regions

### ARIA Live Attributes

- `aria-live="polite"` → updates announced when user is idle
- `aria-live="assertive"` → urgent updates, interrupts reading
- `aria-live="off"` → disables live region announcements entirely (default behavior)
- `aria-atomic="true"` → announce entire region
- `aria-atomic="false"` → announce only changes
- `aria-relevant`: `additions`, `removals`, `text`, `all`

### Implicit Live Region Roles

- `role="status"` → non-urgent status updates (implies `aria-live="polite"`)
- `role="alert"` → important, time-sensitive messages (implies `aria-live="assertive"`)
- Other roles: `log`, `progressbar`, `marquee`, `timer`

### Best Practices

- Live regions must exist in the DOM before content updates (create container on page load)
- Avoid overusing `aria-live="assertive"`—it's disruptive
- Test live regions with actual screen readers, as behavior varies between implementations

---

## 4. Icons & Media

### Icons

- **Interactive icons** → provide `aria-label` or accessible text
- **Decorative icons** → `aria-hidden="true"` and empty `alt=""`
- **Icon buttons** → ensure button has accessible text via `aria-label`, visually-hidden text, or `<title>` element (for SVG)
- **Icon-only buttons** → should have visible tooltip on hover/focus for sighted keyboard users

### Images

- Provide text alternatives for all meaningful images using `alt` attribute
- Decorative images → `alt=""` (empty, not missing)
- Complex images (charts, diagrams) → use `alt` or `aria-describedby` for detailed descriptions
- Decorative background images → no accessibility annotation needed (but ensure text contrast)

### Video & Audio

- Provide **captions** for video (for deaf users)
- Provide **audio descriptions** for video (for blind users)
- Provide **transcripts** for audio content
- Include **keyboard controls** for all media players
- Don't autoplay media with sound; provide controls to pause/stop
- Ensure media players have visible focus states

### Time-based Content

- Use `role="timer"` for countdown/countup displays that auto-update
- Combine with `aria-live="off"` (or `polite`) to control announcement frequency

---

## 5. Focus & Keyboard Navigation

### Focus Management

- Focus must be visible—never use `outline: none` without providing an alternative focus indicator
- Ensure focus order follows logical reading order (matches visual layout)
- All interactive elements must be reachable via `Tab` key
- Avoid keyboard traps (users should always be able to navigate away)
- Modals must trap focus inside and return focus on close
- For single-page apps, manage focus when routes change (move focus to new heading/content)

### Tabindex Usage

- `tabindex="0"` → makes custom interactive elements focusable (natural tab order)
- `tabindex="-1"` → allows programmatic focus only (not in tab order)
- **Never use positive `tabindex` values** (e.g., `tabindex="1"`)—it breaks natural tab order

### Keyboard Navigation Patterns by Component

#### Standard Form Controls

| Component                 | Keys                                  | Behavior                                          |
| ------------------------- | ------------------------------------- | ------------------------------------------------- |
| **Button**                | `Enter` or `Space`                    | Activates the button                              |
|                           | `Tab`                                 | Moves focus to next element                       |
| **Link**                  | `Enter`                               | Follows the link                                  |
|                           | `Space`                               | No action (scrolls page)                          |
| **Checkbox** (native)     | `Space`                               | Toggles checked state                             |
|                           | `Tab`                                 | Moves to next element                             |
| **Radio Button** (native) | `Arrow Up/Down` or `Arrow Left/Right` | Moves selection to previous/next radio in group   |
|                           | `Tab`                                 | Moves focus to radio group (then to next element) |
|                           | `Space`                               | Selects focused radio (if none selected)          |
| **Text Input**            | `Tab`                                 | Moves focus to next element                       |
|                           | All typing keys                       | Inputs text                                       |
|                           | `Ctrl + A`                            | Selects all text                                  |
| **Textarea**              | `Tab`                                 | Moves focus to next element (unless modified)     |
|                           | All typing keys                       | Inputs text including `Enter` for new lines       |

#### Dropdown & Selection Components

| Component                   | Keys                    | Behavior                                       |
| --------------------------- | ----------------------- | ---------------------------------------------- |
| **Select (native)**         | `Arrow Up/Down`         | Moves through options (opens on some browsers) |
|                             | `Enter` or `Space`      | Opens dropdown menu                            |
|                             | `Home` / `End`          | Jumps to first/last option                     |
|                             | `Type-ahead`            | Jumps to option starting with typed character  |
|                             | `Tab`                   | Closes dropdown and moves to next element      |
|                             | `Esc`                   | Closes dropdown without changing selection     |
| **Combobox (custom)**       | `Arrow Down`            | Opens dropdown, moves to next option           |
|                             | `Arrow Up`              | Opens dropdown, moves to previous option       |
|                             | `Enter`                 | Selects highlighted option and closes dropdown |
|                             | `Esc`                   | Closes dropdown without selecting              |
|                             | `Home` / `End`          | Moves to first/last option                     |
|                             | `Type-ahead`            | Filters or jumps to matching options           |
|                             | `Tab`                   | Closes dropdown and moves to next element      |
| **Listbox (single-select)** | `Arrow Up/Down`         | Moves selection                                |
|                             | `Home` / `End`          | Moves to first/last item                       |
|                             | `Type-ahead`            | Jumps to item starting with typed character    |
|                             | `Tab`                   | Moves to next element                          |
| **Listbox (multi-select)**  | `Arrow Up/Down`         | Moves focus (not selection)                    |
|                             | `Space`                 | Toggles selection of focused item              |
|                             | `Shift + Arrow Up/Down` | Extends selection                              |
|                             | `Ctrl + A`              | Selects all items                              |
|                             | `Tab`                   | Moves to next element                          |

#### File Upload

| Component               | Keys                    | Behavior                                     |
| ----------------------- | ----------------------- | -------------------------------------------- |
| **File Input (native)** | `Tab`                   | Moves focus to file input button             |
|                         | `Enter` or `Space`      | Opens file browser dialog                    |
| **Custom File Upload**  | `Tab`                   | Moves focus to upload trigger button         |
|                         | `Enter` or `Space`      | Opens file browser or triggers upload action |
|                         | `Delete` or `Backspace` | Removes selected file (if showing file list) |

#### Toggle & Switch Components

| Component           | Keys                                              | Behavior                               |
| ------------------- | ------------------------------------------------- | -------------------------------------- |
| **Toggle Button**   | `Space` or `Enter`                                | Toggles pressed state (`aria-pressed`) |
|                     | `Tab`                                             | Moves to next element                  |
| **Switch (custom)** | `Space`                                           | Toggles on/off state                   |
|                     | `Tab`                                             | Moves to next element                  |
|                     | **Note:** Should behave like checkbox, not button |

#### Date & Time Pickers

| Component                | Keys                    | Behavior                                     |
| ------------------------ | ----------------------- | -------------------------------------------- |
| **Date Input (native)**  | `Tab`                   | Moves focus to date input                    |
|                          | `Arrow Up/Down`         | Increments/decrements focused segment        |
|                          | `Arrow Left/Right`      | Moves between date segments (day/month/year) |
|                          | `Enter` or `Space`      | Opens calendar popup (browser-dependent)     |
| **Date Picker (custom)** | `Tab`                   | Moves to date input field                    |
|                          | `Arrow Down` or `Space` | Opens calendar dialog                        |
|                          | **In Calendar:**        |                                              |
|                          | `Arrow keys`            | Navigates between dates                      |
|                          | `Home` / `End`          | First/last day of week                       |
|                          | `Page Up/Down`          | Previous/next month                          |
|                          | `Shift + Page Up/Down`  | Previous/next year                           |
|                          | `Enter` or `Space`      | Selects focused date                         |
|                          | `Esc`                   | Closes calendar without selecting            |

#### Sliders & Range Inputs

| Component        | Keys              | Behavior                             |
| ---------------- | ----------------- | ------------------------------------ |
| **Range Slider** | `Arrow Left/Down` | Decreases value by one step          |
|                  | `Arrow Right/Up`  | Increases value by one step          |
|                  | `Home`            | Sets to minimum value                |
|                  | `End`             | Sets to maximum value                |
|                  | `Page Up`         | Increases by larger step (e.g., 10%) |
|                  | `Page Down`       | Decreases by larger step (e.g., 10%) |
|                  | `Tab`             | Moves to next element                |

#### Menu & Navigation Components

| Component             | Keys                               | Behavior                                    |
| --------------------- | ---------------------------------- | ------------------------------------------- |
| **Menu Button**       | `Enter` or `Space` or `Arrow Down` | Opens menu, focus moves to first item       |
|                       | `Arrow Up`                         | Opens menu, focus moves to last item        |
| **Menu (vertical)**   | `Arrow Down`                       | Moves to next menu item                     |
|                       | `Arrow Up`                         | Moves to previous menu item                 |
|                       | `Home` / `End`                     | Moves to first/last item                    |
|                       | `Enter` or `Space`                 | Activates menu item                         |
|                       | `Esc`                              | Closes menu, returns focus to button        |
|                       | `Tab`                              | Closes menu, moves to next element          |
|                       | `Type-ahead`                       | Moves to item starting with typed character |
| **Menu (horizontal)** | `Arrow Right`                      | Moves to next menu item                     |
|                       | `Arrow Left`                       | Moves to previous menu item                 |
|                       | `Arrow Down`                       | Opens submenu (if exists)                   |
|                       | `Home` / `End`                     | Moves to first/last item                    |
|                       | `Enter` or `Space`                 | Activates menu item                         |
|                       | `Esc`                              | Closes menu                                 |
| **Menubar**           | `Arrow Right/Left`                 | Moves between top-level items               |
|                       | `Arrow Down`                       | Opens submenu                               |
|                       | `Enter`                            | Activates item or opens submenu             |
|                       | `Esc`                              | Closes submenu, focus to parent             |

#### Tabs

| Component                           | Keys                  | Behavior                                        |
| ----------------------------------- | --------------------- | ----------------------------------------------- |
| **Tab List**                        | `Tab`                 | Moves focus into tab list (to selected tab)     |
|                                     | `Arrow Left`          | Moves to previous tab, activates it (automatic) |
|                                     | `Arrow Right`         | Moves to next tab, activates it (automatic)     |
|                                     | `Home`                | Moves to first tab                              |
|                                     | `End`                 | Moves to last tab                               |
|                                     | `Tab` (from tab list) | Moves focus to active tab panel                 |
| **Alternative (manual activation)** | `Arrow Left/Right`    | Moves focus only (doesn't activate)             |
|                                     | `Enter` or `Space`    | Activates focused tab                           |

#### Accordion

| Component            | Keys                    | Behavior                                           |
| -------------------- | ----------------------- | -------------------------------------------------- |
| **Accordion Header** | `Enter` or `Space`      | Toggles panel open/closed                          |
|                      | `Tab`                   | Moves to next accordion header or out of accordion |
|                      | `Arrow Down` (optional) | Moves focus to next header                         |
|                      | `Arrow Up` (optional)   | Moves focus to previous header                     |
|                      | `Home` (optional)       | Moves to first header                              |
|                      | `End` (optional)        | Moves to last header                               |

#### Modal Dialog

| Component | Keys              | Behavior                                            |
| --------- | ----------------- | --------------------------------------------------- |
| **Modal** | `Tab`             | Moves focus forward within modal (cycles at end)    |
|           | `Shift + Tab`     | Moves focus backward within modal (cycles at start) |
|           | `Esc`             | Closes modal (returns focus to trigger)             |
|           | **Focus trap**    | Focus must stay within modal until closed           |
|           | **Initial focus** | Move to first focusable element or close button     |

#### Tooltip

| Component           | Keys                                              | Behavior                                     |
| ------------------- | ------------------------------------------------- | -------------------------------------------- |
| **Tooltip Trigger** | `Hover`                                           | Shows tooltip (with delay)                   |
|                     | `Focus`                                           | Shows tooltip immediately                    |
|                     | `Esc`                                             | Dismisses tooltip (returns focus to trigger) |
|                     | `Tab`                                             | Closes tooltip, moves to next element        |
|                     | **Note:** Tooltip content should not be focusable |

#### Carousel / Slideshow

| Component    | Keys               | Behavior                                            |
| ------------ | ------------------ | --------------------------------------------------- |
| **Carousel** | `Tab`              | Moves through carousel controls (prev/next buttons) |
|              | `Arrow Left`       | Previous slide (when carousel focused)              |
|              | `Arrow Right`      | Next slide (when carousel focused)                  |
|              | `Home`             | First slide                                         |
|              | `End`              | Last slide                                          |
|              | `Space` or `Enter` | Pause/play auto-rotation                            |

#### Tree View

| Component | Keys               | Behavior                                     |
| --------- | ------------------ | -------------------------------------------- |
| **Tree**  | `Arrow Down`       | Moves to next visible node                   |
|           | `Arrow Up`         | Moves to previous visible node               |
|           | `Arrow Right`      | Expands closed node, or moves to first child |
|           | `Arrow Left`       | Collapses open node, or moves to parent      |
|           | `Home`             | Moves to first node                          |
|           | `End`              | Moves to last visible node                   |
|           | `Enter` or `Space` | Activates/selects node                       |
|           | `Type-ahead`       | Moves to node starting with typed character  |
|           | `*` (asterisk)     | Expands all siblings at same level           |

#### Data Grid / Table

| Component | Keys           | Behavior                                                |
| --------- | -------------- | ------------------------------------------------------- |
| **Grid**  | `Arrow keys`   | Moves focus between cells                               |
|           | `Home`         | Moves to first cell in row                              |
|           | `End`          | Moves to last cell in row                               |
|           | `Ctrl + Home`  | Moves to first cell in grid                             |
|           | `Ctrl + End`   | Moves to last cell in grid                              |
|           | `Page Up/Down` | Scrolls grid up/down by viewport                        |
|           | `Enter`        | Activates cell (edit mode or action)                    |
|           | `Tab`          | Moves to next interactive element in grid or exits grid |

#### Toolbar

| Component   | Keys                 | Behavior                                         |
| ----------- | -------------------- | ------------------------------------------------ |
| **Toolbar** | `Tab`                | Moves focus into toolbar (to first enabled item) |
|             | `Arrow Left/Right`   | Moves between toolbar items                      |
|             | `Home` / `End`       | Moves to first/last item                         |
|             | `Enter` or `Space`   | Activates focused item                           |
|             | `Tab` (from toolbar) | Moves out of toolbar to next element             |

### Best Practices for Keyboard Navigation

#### General Principles

1. **Follow Platform Conventions**

   - Use standard keyboard patterns users expect
   - Test on multiple platforms (Windows, Mac, mobile)

2. **Provide Multiple Ways**

   - Support both `Enter` and `Space` for buttons when possible
   - Offer both arrow keys and `Tab` where appropriate

3. **Visual Feedback**

   - Always show clear focus indicators
   - Indicate which keys are available (tooltips, labels)

4. **Documentation**

   - Consider adding keyboard shortcut help (press `?` to show)
   - Include instructions for complex widgets

5. **Avoid Conflicts**

   - Don't override browser shortcuts (like `Ctrl+T`, `F5`)
   - Be careful with single-key shortcuts (can interfere with screen readers)

6. **Focus Management**
   - When removing focused element, move focus intentionally
   - Return focus after closing modals/dialogs
   - Move focus to meaningful content after route changes

#### Common Mistakes to Avoid

❌ **Don't:**

- Trap focus without `Esc` key to exit
- Use `Enter` for checkbox activation (use `Space`)
- Make arrow keys scroll the page when in a widget
- Forget to implement `Home`/`End` in lists and menus
- Override expected behavior without good reason
- Use hover-only interactions without keyboard equivalents

✅ **Do:**

- Test with keyboard only before shipping
- Follow ARIA Authoring Practices Guide patterns
- Provide visible focus indicators
- Document custom keyboard interactions
- Consider screen reader users (focus movement is announced)
- Test with actual assistive technology users

---

## 6. Links

- Links with `target="_blank"` should use `rel="noopener"` for security
- Provide visual/textual indication that link opens in new window (icon + screen reader text)
- Link text should be descriptive—avoid "click here" or "read more" without context
- Use `aria-label` or visually-hidden text to add context to generic link text
- Distinguish links from regular text using more than color (underline, bold, icons)
- Ensure links have sufficient color contrast (3:1 minimum for UI components)

---

## 7. ARIA Best Practices

### The Five Rules of ARIA

1. **Don't use ARIA if native HTML works**

   - Use `<button>` instead of `<div role="button">`
   - Use `<input type="checkbox">` instead of `<div role="checkbox">`

2. **Don't change native semantics unless absolutely necessary**

   - ❌ Bad: `<h2 role="tab">` (confuses heading and tab semantics)

3. **All interactive ARIA controls must be keyboard-accessible**

4. **Don't use `role="presentation"` or `aria-hidden="true"` on focusable elements**

5. **All interactive elements must have an accessible name**

### Common ARIA Roles

- **Widgets:** `button`, `link`, `checkbox`, `radio`, `textbox`, `slider`, `progressbar`
- **Landmarks:** `navigation`, `search`, `main`, `complementary`, `contentinfo`, `banner`
- **Dialogs:** `dialog`, `alertdialog`
- **Menus:** `menu`, `menubar`, `menuitem`, `menuitemcheckbox`, `menuitemradio`
- **Tabs:** `tab`, `tabpanel`, `tablist`
- **Regions:** `region` (requires `aria-label`)

### ARIA States & Properties

- `aria-expanded` → for collapsible content (true/false)
- `aria-pressed` → for toggle buttons (true/false/mixed)
- `aria-selected` → for selectable items like tabs (true/false)
- `aria-checked` → for checkboxes/radio buttons when not using native inputs (true/false/mixed)
- `aria-current` → indicates current item in navigation (page, step, location, date, time, true, false)
- `aria-hidden="true"` → hides content from assistive technology (but remains visible)
- `aria-labelledby` → references element(s) that label the current element
- `aria-modal="true"` → indicates a modal dialog

---

## 8. Testing Best Practices

### Manual Testing

- Test with keyboard only (unplug mouse)
- Test with actual screen readers (NVDA, JAWS, VoiceOver, TalkBack)
- Test at 200% browser zoom
- Test with browser extensions disabled (ensure native HTML semantics work)
- Test in high contrast mode (Windows High Contrast, dark mode)
- Test all interactive states (hover, focus, active, disabled)

### Automated Testing

- Use multiple tools (axe, WAVE, Lighthouse)
- Remember: automated tools catch only ~30-40% of issues

### User Testing

- Include users with disabilities in usability testing
- Test with diverse assistive technologies
- Gather feedback on real-world usage patterns

---

## 9. Developer Tools for Accessibility

### 9.1 VSCode Extensions

- **axe Accessibility Linter** – automatically detect accessibility issues in code
- **ESLint Plugin JSX-A11y** – lint ARIA and JSX accessibility rules
- **Color Highlight / Contrast Checker** – ensures sufficient color contrast
- **Webhint** – accessibility, performance, and security hints
- **htmlhint** – HTML validation including accessibility checks

### 9.2 Browser Tools

- **Chrome DevTools Lighthouse** – automated accessibility audit
- **axe DevTools** (Chrome & Edge extension) – live accessibility testing
- **WAVE** (Web Accessibility Evaluation Tool) – visual accessibility testing
- **Accessibility Insights for Web** – comprehensive scanning and guidance
- **Firefox Accessibility Inspector** – built-in accessibility tree viewer
- **Chrome's Accessibility pane** (DevTools) – inspect accessibility properties
- **Stark** – contrast checker and color blindness simulator

### 9.3 Screen Readers

| Platform | Screen Reader            |
| -------- | ------------------------ |
| Windows  | NVDA (free), JAWS (paid) |
| macOS    | VoiceOver (built-in)     |
| iOS      | VoiceOver (built-in)     |
| Android  | TalkBack (built-in)      |
| Linux    | Orca (free)              |

---

## 10. References

### Official Standards & Guidelines

- [WCAG 2.2 Guidelines](https://www.w3.org/WAI/WCAG22/quickref/) – W3C Web Content Accessibility Guidelines
- [WAI-ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/) – patterns for accessible interactive widgets
- [MDN Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility) – comprehensive technical documentation

### Community Resources

- [WebAIM](https://webaim.org/) – articles, training, and accessibility resources
- [The A11Y Project](https://www.a11yproject.com/) – community-driven accessibility checklist
- [Inclusive Components](https://inclusive-components.design/) – accessible component patterns by Heydon Pickering

### Specific Topics

- [Smashing Magazine – Accessible Form Validation](https://www.smashingmagazine.com/2023/02/guide-accessible-form-validation/)
- [MDN – ARIA Live Regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions)
