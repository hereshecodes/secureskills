---
name: accessibility-testing
description: Use when testing, reviewing, or releasing UI for accessibility, or before claiming WCAG 2.1 AA, ADA Title II, or Section 508 conformance
---

## Accessibility Testing (WCAG 2.1 AA)

Automated tools find only some WCAG failures. A conformance claim needs every pass below.

> Related: accessibility, security-context

### Pass 1: Automated Checks in CI
Run axe on every page AND every state: open dialogs, error states, expanded menus, filtered grids.
```javascript
// WRONG: one scan of the landing page, results ignored
await new AxeBuilder({ page }).analyze();
// RIGHT: WCAG 2.1 A and AA rules, every key state, test fails on any violation
import AxeBuilder from '@axe-core/playwright';
test('gradebook: error state has no violations', async ({ page }) => {
  await page.goto('/gradebook'); await page.getByRole('button', { name: 'Save' }).click();
  const { violations } = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa', 'wcag21a', 'wcag21aa']).analyze();
  expect(violations).toEqual([]);
});
```
```bash
npx lighthouse http://localhost:4200 --only-categories=accessibility --view   # spot check, not a gate
```
.NET: `Deque.AxeCore.Playwright` gives the same check in xUnit or NUnit.

### Pass 2: Keyboard Only (unplug the mouse)
Tab, Shift+Tab, Enter, Space, Esc, arrow keys. On every page check:
- [ ] A skip link is first, and it works
- [ ] Every control is reachable, in a logical order, with a focus ring you can always see
- [ ] Disabled actions can still be focused and their reason is read or shown
- [ ] Dialogs, drawers, menus: focus moves in, cannot escape, Esc closes, focus returns to the trigger
- [ ] Tooltips open on focus, close on Esc, and you can move the pointer onto them
- [ ] No keyboard trap anywhere (editors, embeds, date pickers)
- [ ] Focus or a `select` change never navigates, submits, or opens a window on its own
- [ ] Single-key shortcuts can be turned off or remapped

### Pass 3: Screen Reader
NVDA + Firefox or Chrome (Windows). VoiceOver + Safari (macOS, Cmd+F5).
- [ ] Page title and `lang` are right; headings list (NVDA: Insert+F7) reads as an outline
- [ ] Every input reads its label, required state, and error message
- [ ] Icon buttons read a name that includes any visible text
- [ ] Grids read column headers, sort direction, selection, and the new row count after filtering
- [ ] Toasts, saves, and errors are announced without moving focus
- [ ] Images read useful alt text; decorative images are skipped

### Pass 4: Zoom, Reflow, and Contrast
```text
WRONG: test at 100% only, eyeball the colors
RIGHT:
- Browser zoom 200%: nothing clipped, overlapping, or hidden
- Window 1280px wide at 400% (= 320 CSS px): one scroll direction, except data tables
- Text spacing bookmarklet (line-height 1.5, letter 0.12em, word 0.16em): nothing cut off
- Rotate a phone or tablet: works in portrait and landscape
- Contrast checker (WebAIM, DevTools): text 4.5:1 (large 3:1); borders, icons, focus rings 3:1
- Session timeout: a warning appears with time to extend
```

### Pass 5: Content, Documents, Media, Email
- [ ] Drag, swipe, pinch, and shake actions have a button alternative; clicks fire on release, not press
- [ ] Instructions don't rely on shape, color, sound, or position alone ("the round button on the right")
- [ ] No images of text; a passage in another language has its own `lang`
- [ ] Pages can be found two ways (navigation plus search or a site map)
- [ ] Generated PDFs pass PAC 2024 or Acrobat "Accessibility > Full Check": tagged, title, language, reading order
- [ ] Video has captions; prerecorded video has audio description or a full transcript
- [ ] HTML emails: `lang`, alt text, real headings, 4.5:1 text, readable at 200%
- [ ] Nothing flashes more than 3 times per second; auto-playing audio can be stopped

### What Automated Tools Can't Catch
| Needs a human | Why |
|----|-------|
| Alt text is accurate, link text makes sense | Tools only see that text exists |
| Focus order is logical; focus returns after dialogs | Tools don't drive the UI |
| Disabled reasons and errors are reachable and clear | Requires judgement and interaction |
| Tooltips are hoverable, dismissible, persistent | Needs real pointer and keyboard use |
| Reflow at 320px, text spacing, orientation | Needs a resized viewport and a human eye |
| Live announcements are timely, not noisy | Only a screen reader shows this |
| Consistent navigation and naming across pages | Spans multiple pages |
| Captions and transcripts are correct | Content, not code |
