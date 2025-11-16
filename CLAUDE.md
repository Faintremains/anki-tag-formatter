# CLAUDE.md - Anki Tag Formatter

## Project Overview

**Anki Tag Formatter** is a single-page web application designed to help users create properly formatted tags for Anki flashcards in medical specialties. The app provides a clean, dark-themed interface for selecting a medical domain and entering a topic name, which it formats into a hierarchical tag structure.

**Format Pattern**: `Domains::{Specialty}::{Topic_Name}`

**Example**: `Domains::Ophthalmology::Retinal_Detachment`

## Repository Structure

This is an intentionally minimal repository:

```
anki-tag-formatter/
├── .git/              # Git repository data
└── index.html         # Complete single-page application
```

### Key Characteristics

- **No build process**: Pure HTML/CSS/JavaScript - runs directly in browser
- **No dependencies**: Self-contained, no npm packages or external libraries
- **No configuration files**: No package.json, webpack, or other tooling
- **Single file architecture**: Everything lives in `index.html`

## Application Architecture

### File Structure (index.html)

The `index.html` file contains three main sections:

1. **HTML Structure** (lines 1-295): Semantic markup with form inputs and output display
2. **Embedded CSS** (lines 8-294): Complete styling with dark theme and responsive design
3. **Embedded JavaScript** (lines 333-393): Interactive functionality and event handlers

### Design System

**Color Palette**:
- Background: `#000000` (pure black)
- Container: `#1a1a1a` to `#0f0f0f` (gradient)
- Inputs: `#0a0a0a` background, `#2a2a2a` borders
- Text: `#ffffff` (white), `#a0a0a0` (labels)
- Accents: `#4a4a4a` (focus states)

**Typography**:
- Primary: System font stack (`-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto...`)
- Output section: `'Inter'` font family (lines 184)
- Monospace width: 750px max
- Responsive breakpoint: 640px

**Favicon**: SVG data URI with hashtag symbol (line 7)

## Core Functionality

### Medical Domains (Specialties)

Currently supported domains (lines 308-312):
1. Ophthalmology
2. Otorhinolaryngology
3. Rheumatology
4. Orthopedics
5. Emergency

### Tag Formatting Logic

**Function**: `formatTopic(text)` (lines 340-345)

```javascript
// Converts input text to Title_Case format
// Examples:
//   "retinal detachment" → "Retinal_Detachment"
//   "acute-otitis-media" → "Acute_Otitis_Media"
//   "COPD exacerbation" → "Copd_Exacerbation"
```

**Process**:
1. Splits on spaces and hyphens: `/[\s-]+/`
2. Capitalizes first letter of each word
3. Lowercases remaining letters
4. Joins with underscores

### User Interactions

1. **Select Domain**: Choose from dropdown (triggers `updateOutput`)
2. **Enter Topic**: Type in text field (triggers `updateOutput` on input)
3. **Copy**: Click "Copy to Clipboard" button or press Enter
4. **Clear**: Reset both fields and output

## Development Conventions

### Code Style

**HTML**:
- 4-space indentation
- Semantic elements preferred
- BEM-like class naming (e.g., `.form-group`, `.output-container`)

**CSS**:
- 4-space indentation
- Properties grouped logically (positioning, box model, visual, typography)
- Gradients for depth and visual interest
- Smooth transitions (0.2s ease) for interactive elements
- Mobile-first responsive design with `@media` queries

**JavaScript**:
- camelCase for variables and functions
- `const` for DOM references
- Arrow functions for event handlers and callbacks
- Template literals not used (prefer string concatenation for compatibility)

### Naming Conventions

**IDs**: camelCase (`specialty`, `topicInput`, `copyBtn`)
**Classes**: kebab-case (`.form-group`, `.copy-btn`, `.output-container`)
**Functions**: camelCase (`formatTopic`, `updateOutput`, `clearForm`)
**Variables**: Descriptive and clear (`specialtySelect`, not `sel`)

## Key Implementation Details

### State Management

No formal state management - state is derived from:
1. `specialtySelect.value` - selected domain
2. `topicInput.value` - entered topic text
3. `output.textContent` - computed output (read-only display)

### Event Handling

**Change Events** (line 369):
- `specialtySelect` on 'change' → `updateOutput()`

**Input Events** (line 370):
- `topicInput` on 'input' → `updateOutput()` (real-time feedback)

**Click Events** (lines 383, 392):
- `copyBtn` on 'click' → Copy to clipboard with visual feedback
- `clearBtn` on 'click' → Reset form

**Keyboard Events** (lines 372-381):
- Enter key in topic field → Copy to clipboard (convenience shortcut)

### Copy Feedback Animation

Visual confirmation when copying (lines 252-270):
1. Add `.copied` class to button
2. Show checkmark (✓) via CSS pseudo-element
3. Remove class after 600ms
4. Button text becomes transparent during animation

## Making Changes

### Adding New Domains

**Location**: Lines 308-312

```html
<select id="specialty">
    <option value=""></option>
    <option value="NewDomain">NewDomain</option>
    <!-- Add here -->
</select>
```

**Guidelines**:
- Keep alphabetical order (currently not followed - consider fixing)
- Use proper capitalization (medical specialty names)
- Value and display text should match
- First option remains empty (default state)

### Modifying Tag Format

**Current Format**: `Domains::{Specialty}::{Topic}`

**To Change** (lines 347-360):
```javascript
function updateOutput() {
    const specialty = specialtySelect.value;
    const topic = topicInput.value.trim();

    let result = 'Domains::';  // Modify prefix here

    if (specialty) {
        result += specialty + '::';  // Modify separator here
        if (topic) {
            result += formatTopic(topic);  // Modify formatting here
        }
    }

    output.textContent = result;
}
```

### Styling Updates

**Theme Colors**: Lines 9-294 (embedded `<style>`)
**Responsive Design**: Lines 272-293 (media query for mobile)
**Animations**: Look for `transition` properties throughout CSS

### Topic Formatting Changes

**Location**: Lines 340-345 (`formatTopic` function)

Current behavior:
- Splits on whitespace and hyphens
- Title case each word
- Joins with underscores

Alternative formats could include:
- PascalCase (no separators)
- kebab-case (hyphens)
- snake_case (underscores, no capitalization)
- UPPER_SNAKE_CASE

## Testing Considerations

Since this is a static page with no test framework:

### Manual Testing Checklist

**Functionality**:
- [ ] Select each domain - verify output format
- [ ] Enter various topic formats (spaces, hyphens, mixed case)
- [ ] Verify `formatTopic` handles edge cases (single word, empty, special chars)
- [ ] Test copy button - verify clipboard content
- [ ] Test Enter key shortcut for copying
- [ ] Test clear button - verify full reset
- [ ] Verify default state shows "Domains::"

**Visual/UI**:
- [ ] Test on mobile (< 640px width)
- [ ] Test on tablet and desktop
- [ ] Verify focus states on inputs
- [ ] Check hover states on buttons
- [ ] Verify copy button checkmark animation
- [ ] Test with long topic names (word-break behavior)

**Browser Compatibility**:
- [ ] Chrome/Edge (Chromium)
- [ ] Firefox
- [ ] Safari
- [ ] Mobile browsers (iOS Safari, Chrome Mobile)

**Edge Cases**:
- Empty specialty, empty topic
- Empty specialty, filled topic (should show "Domains::")
- Filled specialty, empty topic (should show "Domains::Specialty::")
- Special characters in topic names
- Very long topic names
- Rapid input changes (debouncing not implemented - verify no issues)

## Git Workflow

### Branch Naming Convention

All development branches follow the pattern:
```
claude/claude-md-{session-id}
```

Example: `claude/claude-md-mi1e857upp5i7auv-01RvxeMFigncTU8ni9dZyZMd`

### Recent History Patterns

Based on recent commits, this project follows:

1. **Feature branches**: Named with `claude/` prefix and descriptive suffix
2. **Focused commits**: Each commit addresses one specific change
3. **Pull requests**: Changes merged via PRs (see commits 16e4038, e537ace, a2c21b1)
4. **Descriptive messages**: Clear, actionable commit messages

**Example commits**:
- ✅ "Change specialty dropdown title from 'Specialty' to 'Domains'"
- ✅ "Change font family in output section"
- ✅ "Fix favicon by properly URL-encoding SVG data URI"

### Commit Message Guidelines

**Format**: `<Action> <description>`

**Good examples**:
- "Add Cardiology to specialty options"
- "Update tag format to include date prefix"
- "Fix topic formatting for hyphenated words"
- "Improve mobile layout for small screens"

**Actions**: Add, Update, Fix, Remove, Change, Improve, Refactor

### Development Process

1. **Create feature branch**: `git checkout -b claude/descriptive-name-{session-id}`
2. **Make focused changes**: Edit `index.html` for your specific feature
3. **Test thoroughly**: Use manual testing checklist
4. **Commit with clear message**: Follow commit message guidelines
5. **Push to remote**: `git push -u origin <branch-name>`
6. **Create pull request**: Merge to main branch

## Common Modifications

### Adding a New Specialty/Domain

```html
<!-- In <select id="specialty"> around line 306 -->
<option value="Cardiology">Cardiology</option>
```

**Test**: Select new option, verify output format

### Changing Label Text

**"Domains" label** (line 304):
```html
<label for="specialty">Domains</label>
```

**"Topic Name" label** (line 318):
```html
<label for="topic">Topic Name</label>
```

**Test**: Visual check, ensure consistency with output format

### Modifying Button Styles

**Copy button** (lines 219-233): White gradient, primary action
**Clear button** (lines 235-246): Dark gradient, secondary action

**Test**: Verify hover, focus, and active states work correctly

### Adjusting Layout

**Desktop layout** (lines 76-79): 2-column grid for form inputs
**Mobile layout** (lines 281-284): Single column for screens < 640px

**Test**: Resize browser, check responsive breakpoints

## Browser APIs Used

1. **Clipboard API** (lines 375, 385):
   ```javascript
   navigator.clipboard.writeText(text)
   ```
   - Modern API (may not work in older browsers)
   - Requires HTTPS in production (or localhost)
   - No fallback implemented

2. **DOM APIs**:
   - `getElementById` - Element selection
   - `addEventListener` - Event handling
   - `textContent` - Safe text manipulation (XSS-safe)
   - `classList.add/remove` - Class manipulation

## Security Considerations

### XSS Prevention

✅ **Safe**: Using `textContent` instead of `innerHTML` (line 360)
```javascript
output.textContent = result;  // Safe - no HTML injection possible
```

❌ **Unsafe** (never do this):
```javascript
output.innerHTML = result;  // Dangerous - could inject scripts
```

### Input Sanitization

**Current approach**: Limited character set through formatting
- `formatTopic()` only produces alphanumeric + underscores
- No validation on specialty (controlled via dropdown)
- No explicit sanitization on topic input

**Risks**: Low - output is only displayed in DOM and copied to clipboard
**Recommendation**: Current approach is sufficient for use case

## Performance Notes

- **No dependencies**: Zero network requests after initial load
- **Real-time updates**: Input event triggers on every keystroke
- **No debouncing**: Acceptable for this simple use case
- **Single DOM paint**: Changes to `output.textContent` only
- **Lightweight**: ~12KB total file size

## Accessibility

### Keyboard Navigation

✅ Tab through form fields
✅ Enter to submit/copy (line 372)
✅ Focus states visible (lines 97-99, 113-120)

### Screen Readers

⚠️ **Missing**: ARIA labels and announcements
⚠️ **Missing**: Live region for output updates
⚠️ **Missing**: Success announcement after copy

### Improvements to Consider

```html
<!-- Add ARIA labels -->
<div class="output" id="output" role="status" aria-live="polite">
    Domains::
</div>

<!-- Announce copy success -->
<div class="sr-only" role="status" aria-live="polite" id="copyStatus"></div>
```

## Future Enhancement Ideas

### Potential Features

1. **Additional Specialties**: Expand medical domains
2. **Custom Prefixes**: Allow user-defined tag prefix
3. **Tag History**: Remember recently created tags
4. **Export Options**: Download as CSV/JSON
5. **Keyboard Shortcuts**: Cmd/Ctrl+K to clear
6. **URL Parameters**: Pre-fill form from URL query string
7. **Dark/Light Mode Toggle**: Theme switcher
8. **Multiple Topics**: Batch tag creation
9. **Tag Validation**: Check against common patterns
10. **Analytics**: Track most-used specialties (privacy-conscious)

### Technical Improvements

1. **Service Worker**: Offline functionality
2. **Web Components**: Modularize for reusability
3. **TypeScript**: Type safety for larger features
4. **Build Process**: Minification and optimization
5. **Testing Framework**: Automated tests (Jest, Vitest)
6. **Accessibility Audit**: WCAG 2.1 AA compliance
7. **Error Handling**: Clipboard API fallback
8. **State Persistence**: LocalStorage for form state
9. **i18n**: Multi-language support
10. **CSS Variables**: Themeable color system

## Troubleshooting

### Copy Button Not Working

**Cause**: Clipboard API requires secure context (HTTPS or localhost)
**Solution**: Serve via HTTPS or use `localhost` for development

### Formatting Not Updating

**Cause**: Event listeners not attached (rare)
**Solution**: Check browser console for JavaScript errors

### Mobile Layout Issues

**Cause**: Viewport meta tag missing (currently present at line 5)
**Solution**: Verify `<meta name="viewport">` is in `<head>`

### Favicon Not Showing

**Cause**: SVG data URI encoding issues
**Solution**: Ensure SVG is properly URL-encoded (line 7)
**Note**: Fixed in commit b967c49

## Summary for AI Assistants

When working with this codebase:

1. **It's intentionally simple**: Don't over-engineer solutions
2. **Single file**: All changes go in `index.html`
3. **No build step**: Changes are immediately visible on refresh
4. **Test in browser**: Always verify changes visually
5. **Maintain style**: Match existing code style and conventions
6. **Git hygiene**: Use descriptive branch names and commit messages
7. **Focus on UX**: Smooth interactions, clear visual feedback
8. **Keep it fast**: No unnecessary dependencies or complexity
9. **Preserve accessibility**: Don't break keyboard navigation
10. **Document changes**: Update this file if adding major features

### Quick Reference

- **Main logic**: Lines 333-393 (JavaScript section)
- **Domains list**: Lines 308-312 (select options)
- **Format function**: Lines 340-345 (`formatTopic`)
- **Output builder**: Lines 347-360 (`updateOutput`)
- **Styling**: Lines 8-294 (CSS section)
- **Mobile responsive**: Lines 272-293 (media query)

### When to Update This File

- Major architectural changes
- New feature additions
- Changes to development workflow
- New conventions or patterns
- Breaking changes to functionality
- Updates to domain list or formatting logic

---

**Last Updated**: 2025-11-16
**Repository**: https://github.com/Faintremains/anki-tag-formatter
**License**: Not specified (consider adding)
