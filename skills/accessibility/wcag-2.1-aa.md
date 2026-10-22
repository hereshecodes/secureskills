# WCAG 2.1 Level A and AA Criteria Map

All 50 Level A and AA success criteria in [WCAG 2.1](https://www.w3.org/TR/WCAG21/) (30 A, 20 AA), what each means in plain words, and where SecureSkills covers it. This is a reference file, not a skill. Use it as the starting point for a VPAT or Accessibility Conformance Report.

**Key**
- **Rule N**: rule in [accessibility/SKILL.md](SKILL.md). **QR**: that skill's Quick Reference table.
- **Pass N**: pass in [accessibility-testing/SKILL.md](../accessibility-testing/SKILL.md). **Pass 1** is automated (axe); the rest are manual.
- **Not covered**: outside what a coding skill can enforce, with the reason.

Standards: ADA Title II (state and local government, including public schools and universities) requires WCAG 2.1 AA. Section 508 points to WCAG 2.0 AA; every 2.0 A/AA criterion is in this table, and the 2.1 additions are marked *(2.1)*.

## 1. Perceivable

| # | Criterion | Level | In plain words | Covered by |
|---|-----------|-------|----------------|------------|
| 1.1.1 | Non-text Content | A | Images, icons and charts have a text alternative; decorative ones are hidden (`alt=""`). | Rule 7, Pass 1, Pass 3 |
| 1.2.1 | Audio-only and Video-only (Prerecorded) | A | Audio-only has a transcript; silent video has a transcript or audio track. | Pass 5. Not a code rule: media content is produced, not coded |
| 1.2.2 | Captions (Prerecorded) | A | Prerecorded video with sound has captions. | Pass 5. Not a code rule: captions are content (ship a `<track kind="captions">`) |
| 1.2.3 | Audio Description or Media Alternative (Prerecorded) | A | Video has audio description or a full text alternative. | Pass 5. Not a code rule: content production |
| 1.2.4 | Captions (Live) | AA | Live video with sound has live captions. | Not covered: needs a captioning service; no live media in scope of these skills |
| 1.2.5 | Audio Description (Prerecorded) | AA | Prerecorded video has audio description. | Pass 5. Not a code rule: content production |
| 1.3.1 | Info and Relationships | A | Structure shown visually (headings, lists, tables, labels, groups) is in the markup too. | Rule 4, Rule 5, Rule 7, Pass 1, Pass 3, Pass 5 (PDF tags) |
| 1.3.2 | Meaningful Sequence | A | DOM and reading order match the visual meaning. | Pass 2 (focus order), Pass 3, Pass 5 (PDF reading order) |
| 1.3.3 | Sensory Characteristics | A | Instructions don't rely only on shape, size, position, color or sound. | Pass 5 |
| 1.3.4 | Orientation *(2.1)* | AA | Works in portrait and landscape; no orientation lock. | QR (never lock orientation), Pass 4 |
| 1.3.5 | Identify Input Purpose *(2.1)* | AA | Personal-data inputs use `autocomplete` tokens. | Rule 5, QR, Pass 1 |
| 1.4.1 | Use of Color | A | Color is never the only way to convey meaning. | Rule 5, Pass 1 (links in text), Pass 4 |
| 1.4.2 | Audio Control | A | Audio that plays over 3 seconds can be paused or muted. | Pass 5 |
| 1.4.3 | Contrast (Minimum) | AA | Text 4.5:1, large text 3:1. | Rule 6, Pass 1, Pass 4 |
| 1.4.4 | Resize Text | AA | Text scales to 200% with nothing lost. | QR (fluid layout, `rem`), Pass 4 |
| 1.4.5 | Images of Text | AA | Use real text, not pictures of text (logos excepted). | Pass 5 |
| 1.4.10 | Reflow *(2.1)* | AA | At 320 CSS px wide (400% zoom), no two-way scrolling except data tables, maps, etc. | QR (fluid layout), Pass 4 |
| 1.4.11 | Non-text Contrast *(2.1)* | AA | Input borders, checkboxes, focus rings and meaningful icons 3:1. | Rule 6, Pass 4 |
| 1.4.12 | Text Spacing *(2.1)* | AA | User-set line, letter, word and paragraph spacing doesn't break the layout. | QR (no fixed heights), Pass 4 |
| 1.4.13 | Content on Hover or Focus *(2.1)* | AA | Tooltips and popovers are dismissible (Esc), hoverable and persistent. | Rule 1, Rule 3, Pass 2 |

## 2. Operable

| # | Criterion | Level | In plain words | Covered by |
|---|-----------|-------|----------------|------------|
| 2.1.1 | Keyboard | A | Everything works with a keyboard alone. | Rule 1, Rule 7, Pass 2 |
| 2.1.2 | No Keyboard Trap | A | Focus can always move away; modal dialogs trap focus on purpose and release it on close. | Rule 2, Pass 2 |
| 2.1.4 | Character Key Shortcuts *(2.1)* | A | Single-key shortcuts can be turned off, remapped, or only work on focus. | Pass 2 |
| 2.2.1 | Timing Adjustable | A | Warn before a time limit and let users extend it. | Rule 8, Pass 4 |
| 2.2.2 | Pause, Stop, Hide | A | Moving, blinking, scrolling or auto-updating content can be paused. | Rule 8 |
| 2.3.1 | Three Flashes or Below Threshold | A | Nothing flashes more than 3 times a second. | Pass 5 |
| 2.4.1 | Bypass Blocks | A | Skip link or landmarks to jump past repeated content. | Rule 7, Pass 1, Pass 2 |
| 2.4.2 | Page Titled | A | Each page (and PDF) has a descriptive title. | Rule 7, QR (PDFs), Pass 1, Pass 3, Pass 5 |
| 2.4.3 | Focus Order | A | Focus moves in a logical order; dialogs return focus to the trigger. | Rule 2, Pass 2 |
| 2.4.4 | Link Purpose (In Context) | A | Link text makes sense alone or with its sentence. No "click here". | Rule 7, Pass 3 |
| 2.4.5 | Multiple Ways | AA | More than one way to find a page (nav, search, site map). | Pass 5 |
| 2.4.6 | Headings and Labels | AA | Headings and labels describe their topic or purpose. | Rule 5, Rule 7, Pass 3 |
| 2.4.7 | Focus Visible | AA | The keyboard focus indicator is always visible. | Rule 6, Pass 2 |
| 2.5.1 | Pointer Gestures *(2.1)* | A | Multi-point or path gestures have a single-pointer alternative. | Pass 5 |
| 2.5.2 | Pointer Cancellation *(2.1)* | A | Actions fire on release, so a press can be aborted. | Pass 5 (native `<button>` does this; Rule 7) |
| 2.5.3 | Label in Name *(2.1)* | A | The accessible name contains the visible label text. | QR, Pass 1, Pass 3 |
| 2.5.4 | Motion Actuation *(2.1)* | A | Shake or tilt actions have a UI alternative and can be turned off. | Pass 5 |

## 3. Understandable

| # | Criterion | Level | In plain words | Covered by |
|---|-----------|-------|----------------|------------|
| 3.1.1 | Language of Page | A | `<html lang>` (and PDF language) is set. | Rule 7, Pass 1, Pass 5 |
| 3.1.2 | Language of Parts | AA | A passage in another language has its own `lang`. | Pass 5 |
| 3.2.1 | On Focus | A | Focusing something never changes context. | QR, Pass 2 |
| 3.2.2 | On Input | A | Changing a value doesn't navigate or submit unless the user was told. | QR, Pass 2 |
| 3.2.3 | Consistent Navigation | AA | Repeated navigation stays in the same order across pages. | QR, Pass 3 |
| 3.2.4 | Consistent Identification | AA | The same function has the same name and icon everywhere. | QR, Pass 3 |
| 3.3.1 | Error Identification | A | Errors name the field and describe the problem in text. | Rule 5, Pass 3 |
| 3.3.2 | Labels or Instructions | A | Inputs have labels and any needed format instructions. | Rule 5, QR, Pass 1, Pass 3 |
| 3.3.3 | Error Suggestion | AA | Errors suggest how to fix the input when known. | Rule 5 |
| 3.3.4 | Error Prevention (Legal, Financial, Data) | AA | Submissions with legal, financial or data consequences can be reviewed, checked, or undone. | Rule 5 (confirm or Undo) |

## 4. Robust

| # | Criterion | Level | In plain words | Covered by |
|---|-----------|-------|----------------|------------|
| 4.1.1 | Parsing | A | Markup has no duplicate IDs or broken nesting. A 2023 W3C errata treats this as always met for HTML; it is removed in WCAG 2.2. | Met by default for HTML (see errata). axe no longer checks it by default. |
| 4.1.2 | Name, Role, Value | A | Every control exposes its name, role and state (`aria-disabled`, `aria-sort`, `aria-selected`, `aria-invalid`). | Rule 1, Rule 2, Rule 4, Rule 5, Rule 7, Pass 1, Pass 3 |
| 4.1.3 | Status Messages *(2.1)* | AA | Status changes (saved, results count, errors) are announced without moving focus. | Rule 4, Rule 5, Pass 3 |

## Not covered by a code rule

| Criterion | Why |
|-----------|-----|
| 1.2.1, 1.2.2, 1.2.3, 1.2.5 | Captions, transcripts and audio description are media content. The testing skill checks they exist; producing them is outside a coding skill. |
| 1.2.4 | Live captions need a captioning service or human captioner. If you add live video, add one. |

Everything else has at least one rule or test. A real conformance claim still needs the manual passes run on the actual product.
