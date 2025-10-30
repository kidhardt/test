# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **zero-build, pure HTML/CSS/JavaScript static website** for ABC Translations, deployed via GitHub Pages. The project prioritizes mobile-first performance, accessibility, and internationalization while maintaining a clean developer experience without any build tools or frameworks.

## Architecture Philosophy

### Four Core Priorities (See docs/priorities.md)

1. **Mobile-First Performance & Universal Access** - Must load in under 3 seconds on 3G with a 2018 mid-range device
2. **HTML Excellence & Accessibility** - WCAG 2.2 AA+ compliant, semantic HTML5
3. **Internationalization from Day One** - Built for 13 languages (15 routes) with RTL support
4. **Clean Developer Experience** - Edit and refresh workflow, no compilation required

### Technical Stack

- **No frameworks** - Pure vanilla JavaScript, HTML5, CSS3
- **No build tools** - No webpack, Vite, or preprocessors
- **No package manager** - No npm/yarn dependencies to install
- **Progressive enhancement** - Core functionality works without JavaScript
- **Modern CSS** - CSS Grid, Flexbox, custom properties (CSS variables)

## File Structure

```
/
├── index.html          # Main landing page (production file)
├── index-bu.html       # Backup copy for safety
├── docs/
│   ├── decisions.md               # Technical decisions log with To Do list
│   ├── priorities.md              # Project priorities and achievements
│   └── revert-github-instructions.md  # Git version control guide
└── .claude/            # Claude Code settings (gitignored)
```

## Documentation (`/docs` folder)

### `decisions.md` - Technical Decisions Log
**Purpose:** Records all significant technical decisions with full justification, implementation details, and removed code.

**Structure:**
- **To Do section** - Deferred items from best practices reviews (check here first!)
- **Decision entries** - Chronological log of technical choices made

**When to update:**
- After making architectural decisions (vendor prefixes, semantic structure, etc.)
- When evaluating best practices and choosing to implement or defer
- When removing/refactoring significant code

**Example entry format:**
```markdown
## Decision: [Title]
**Date:** YYYY-MM-DD
**Context:** What prompted this
**Analysis:** Technical research and trade-offs
**Decision:** What we chose
**Justification:** Why, aligned with priorities
**Implementation:** What changed
**Impact:** Results and benefits
```

### `priorities.md` - Project Goals and Achievements
**Purpose:** Documents the four core priorities and tracks achievements against them.

**Contains:**
- Ranked priorities with success metrics
- Technical implementation guidelines for each priority
- **Successes section** - What's been achieved (update after major milestones)
- **Recent Improvements** - Latest changes (update with each significant improvement)

**When to update:**
- After completing features that advance priority goals
- After performance optimizations
- After accessibility improvements

### `revert-github-instructions.md` - Git Recovery Guide
**Purpose:** Step-by-step instructions for reverting to previous versions.

**Contains:**
- How to view commit history
- Multiple methods to revert changes
- Best practices for version control

**When to reference:** When you need to undo changes or restore previous versions.

## Development Workflow

### Making Changes

1. **Edit directly** - Open `index.html` in VS Code and edit
2. **Preview locally** - Open `index.html` in a browser or use Live Server extension
3. **Commit changes** - Use Git to commit (see commit message guidelines below)
4. **Push to deploy** - Changes automatically deploy to GitHub Pages on push to main

### No Build Steps Required

There is no `npm install`, `npm run build`, or compilation. Simply edit the HTML/CSS/JavaScript and refresh your browser.

## Git Workflow

### Deployment

- **Live Site**: https://kidhardt.github.io/test/
- **Deployment**: Automatic on push to `main` branch via GitHub Pages
- **Build Type**: Legacy (Jekyll-based GitHub Pages)

### Commit Message Format

Follow this format for all commits:

```
Brief description of changes

Optional longer description if needed.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

### Common Git Operations

```bash
# Check status
git status

# Add and commit changes
git add index.html
git commit -m "Your message here"

# Push to GitHub (auto-deploys)
git push

# View commit history
git log --oneline

# Revert to previous version
git checkout <commit-hash> index.html
git commit -m "Revert index.html to previous version"
git push
```

See `docs/revert-github-instructions.md` for detailed revert instructions.

## Code Standards

### HTML

- **All styles inline** - CSS is within `<style>` tags in the `<head>`
- **Semantic HTML5** - Proper use of landmarks, heading hierarchy
- **Progressive enhancement** - `<noscript>` fallbacks for critical functionality
- **Accessibility** - `:focus-visible` for keyboard users, proper ARIA when needed
- **Mobile-first** - Viewport meta tag, responsive design via CSS media queries

### CSS

- **CSS Custom Properties** - Use CSS variables defined in `:root` for theming
- **Mobile-first media queries** - Start with mobile, add `@media (min-width)` for larger screens
- **Modern layout** - CSS Grid and Flexbox (no floats or positioning hacks)
- **Text rendering optimization**:
  - `text-size-adjust: 100%` on `html` (prevents iOS auto-sizing)
  - `-webkit-font-smoothing: antialiased` on `body` (macOS/iOS smoothing)
  - `-moz-osx-font-smoothing: grayscale` on `body`

### JavaScript

- **Vanilla JS only** - No jQuery, React, or other frameworks
- **Progressive enhancement** - Site must work without JavaScript
- **Minimal usage** - Only for interactive features that cannot be CSS-only

## Internationalization (Future)

The architecture is designed for 13 languages with 15 routes:
- English, Spanish (Spain & Latin America), French, German, Portuguese (Brazil & Portugal), Italian, Dutch, Russian, Japanese, Chinese (Simplified & Traditional), Korean, Arabic, Farsi

**When implementing i18n:**
- Add `hreflang` links for all language variants
- Use logical CSS properties for RTL support (`inline-start` vs `left`)
- Organize localized assets by language code
- Use `.resx` files for UI strings

## Performance Targets

- **Time to Interactive**: < 3 seconds on 3G (mid-range 2018 device)
- **Payload**: Minimal - lean HTML, purged CSS, minimal JS
- **Font Loading**: Preconnect + preload strategy (already implemented)
- **Images**: Responsive images with multiple sizes (when added)

## Key Implementation Details

### Font Loading Strategy

Fonts are loaded with preconnect/preload for optimal performance:
```html
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preload" as="style" href="..." onload="...">
<noscript><link rel="stylesheet" href="..."></noscript>
```

### CSS Variables for Theming

All colors are defined in `:root` as CSS custom properties:
```css
:root {
  --color-bg1: rgb(8, 10, 15);
  --color-bg2: rgb(0, 17, 32);
  --color1: 18, 113, 255;
  /* ... */
}
```

Responsive circle sizes scale with viewport breakpoints (768px, 1024px, 1440px, 1920px).

### Focus Management

Accessibility-friendly focus indicators:
```css
/* Visible for keyboard users */
:focus-visible {
  outline: 2px solid #2563eb;
  outline-offset: 3px;
}

/* Hidden for mouse users */
:focus:not(:focus-visible) {
  outline: none;
}
```

## Making Changes

When modifying the site:

1. **Maintain zero-build philosophy** - No dependencies, no compilation
2. **Test without JavaScript** - Ensure core functionality works with JS disabled
3. **Check mobile-first** - Test on small viewports first
4. **Preserve accessibility** - Maintain keyboard navigation, screen reader compatibility
5. **Keep it lean** - Minimize payload, avoid unnecessary code

## Documentation

All achievements and priorities are documented in `docs/priorities.md`. Update this file when significant improvements are made to track progress against the four core priorities.

## Backup Strategy

`index-bu.html` serves as a safety backup. Update it when making major changes to `index.html`:

```bash
cp index.html index-bu.html
git add index-bu.html
git commit -m "Update backup copy"
git push
```
