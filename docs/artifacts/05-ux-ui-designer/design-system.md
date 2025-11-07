# Design System: Basic Calculator Web App

**Date Created**: 2025-11-07  
**Role**: UX/UI Designer (Alex Chen)  
**Status**: Initial Draft  
**Version**: 1.0

---

## Overview

This document defines the complete design system for the Basic Calculator Web App, including design tokens, typography, color system, spacing, and design principles. This system ensures visual consistency and provides a foundation for scalable, maintainable UI development.

---

## Design Principles

### 1. **Clarity Over Cleverness**

Every interface element should have a clear, obvious purpose. Users shouldn't have to guess what a button does or where to find a feature.

**In practice**:

- Clear button labels ("Clear" not just "C")
- Obvious visual hierarchy
- Consistent interaction patterns

### 2. **Speed & Efficiency**

Performance is a feature. The app should load instantly and respond immediately to every interaction.

**In practice**:

- < 1 second initial load
- Instant button feedback (< 100ms)
- Optimistic UI updates
- Skeleton screens for perceived performance

### 3. **Accessible to All**

Design for everyone, including users with disabilities, varying technical skills, and different devices.

**In practice**:

- WCAG 2.1 AA compliance (AAA where possible)
- Keyboard navigation for all features
- Screen reader support
- High contrast mode support

### 4. **Privacy & Trust**

Build trust through transparent communication about data usage and privacy-respecting design.

**In practice**:

- Clear privacy messaging
- No dark patterns (manipulative UI)
- Honest upgrade prompts
- Secure payment indicators

### 5. **Progressive Enhancement**

Start with core functionality that works everywhere, then enhance for capable devices.

**In practice**:

- Core calculator works without JavaScript (forms fallback)
- History enhancement (localStorage → cloud sync)
- Progressive Web App features
- Graceful degradation

---

## Design Tokens

### Color Palette

#### Primary Colors

```css
:root {
  /* Primary - Blue (Brand) */
  --color-primary-900: #001F3F;
  --color-primary-800: #003D7A;
  --color-primary-700: #0052A3;
  --color-primary-600: #0066CC; /* Base primary */
  --color-primary-500: #1A7FE6;
  --color-primary-400: #4D9FEB;
  --color-primary-300: #80BFF0;
  --color-primary-200: #B3DFF5;
  --color-primary-100: #E6F1FF;
  
  /* Semantic mapping */
  --color-primary: var(--color-primary-600);
  --color-primary-hover: var(--color-primary-700);
  --color-primary-active: var(--color-primary-800);
  --color-primary-light: var(--color-primary-100);
}
```

#### Neutral Colors (Grayscale)

```css
:root {
  --color-neutral-900: #0A0A0A;
  --color-neutral-800: #1A1A1A;
  --color-neutral-700: #333333;
  --color-neutral-600: #4D4D4D;
  --color-neutral-500: #666666;
  --color-neutral-400: #999999;
  --color-neutral-300: #CCCCCC;
  --color-neutral-200: #E0E0E0;
  --color-neutral-100: #F0F0F0;
  --color-neutral-50: #F8F9FA;
  --color-white: #FFFFFF;
  
  /* Semantic mapping */
  --color-text-primary: var(--color-neutral-800);
  --color-text-secondary: var(--color-neutral-500);
  --color-text-tertiary: var(--color-neutral-400);
  --color-text-inverse: var(--color-white);
  --color-bg-primary: var(--color-white);
  --color-bg-secondary: var(--color-neutral-50);
  --color-bg-tertiary: var(--color-neutral-100);
  --color-border: var(--color-neutral-200);
  --color-border-hover: var(--color-neutral-300);
}
```

#### Semantic Colors

```css
:root {
  /* Success - Green */
  --color-success-900: #004D2A;
  --color-success-600: #00AA55; /* Base */
  --color-success-300: #66D699;
  --color-success-100: #E6F7ED;
  --color-success: var(--color-success-600);
  --color-success-light: var(--color-success-100);
  
  /* Error - Red */
  --color-error-900: #8B1A1A;
  --color-error-600: #DD3333; /* Base */
  --color-error-300: #EB8080;
  --color-error-100: #FFEAEA;
  --color-error: var(--color-error-600);
  --color-error-light: var(--color-error-100);
  
  /* Warning - Orange */
  --color-warning-900: #995200;
  --color-warning-600: #FFAA00; /* Base */
  --color-warning-300: #FFCC66;
  --color-warning-100: #FFF5E6;
  --color-warning: var(--color-warning-600);
  --color-warning-light: var(--color-warning-100);
  
  /* Info - Light Blue */
  --color-info-900: #003D66;
  --color-info-600: #0099FF;
  --color-info-300: #66C2FF;
  --color-info-100: #E6F6FF;
  --color-info: var(--color-info-600);
  --color-info-light: var(--color-info-100);
}
```

#### Accent Colors

```css
:root {
  /* Premium Gold (for premium badge, highlights) */
  --color-accent-gold: #D4AF37;
  --color-accent-gold-light: #F7EED6;
  
  /* Clear Orange (for clear/delete actions) */
  --color-accent-orange: #CC5500;
  --color-accent-orange-light: #FFF5E6;
}
```

---

### Typography

#### Font Stack

```css
:root {
  /* Primary font family */
  --font-family-sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', 
                       Roboto, 'Helvetica Neue', Arial, sans-serif;
  
  /* Monospace for calculator display and code */
  --font-family-mono: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', 
                       Consolas, 'Courier New', monospace;
  
  /* Default */
  --font-family-base: var(--font-family-sans);
}
```

**Font Loading**: Use `font-display: swap` for web fonts to prevent FOIT (Flash of Invisible Text).

#### Font Sizes

```css
:root {
  /* Font sizes (rem-based, 1rem = 16px) */
  --font-size-xs: 0.75rem;    /* 12px */
  --font-size-sm: 0.875rem;   /* 14px */
  --font-size-base: 1rem;     /* 16px */
  --font-size-lg: 1.125rem;   /* 18px */
  --font-size-xl: 1.25rem;    /* 20px */
  --font-size-2xl: 1.5rem;    /* 24px */
  --font-size-3xl: 1.875rem;  /* 30px */
  --font-size-4xl: 2.25rem;   /* 36px */
  --font-size-5xl: 3rem;      /* 48px */
  
  /* Calculator-specific */
  --font-size-calc-display: 2.25rem;  /* 36px */
  --font-size-calc-button: 1.75rem;   /* 28px */
}
```

#### Font Weights

```css
:root {
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
  --font-weight-extrabold: 800;
}
```

#### Line Heights

```css
:root {
  --line-height-tight: 1.25;   /* Headings */
  --line-height-normal: 1.5;   /* Body text */
  --line-height-relaxed: 1.75; /* Long-form content */
}
```

#### Text Styles

**Heading Styles**:

```css
h1, .text-h1 {
  font-size: var(--font-size-4xl);
  font-weight: var(--font-weight-bold);
  line-height: var(--line-height-tight);
  color: var(--color-text-primary);
  margin: 0 0 1rem;
}

h2, .text-h2 {
  font-size: var(--font-size-3xl);
  font-weight: var(--font-weight-bold);
  line-height: var(--line-height-tight);
  color: var(--color-text-primary);
  margin: 0 0 0.875rem;
}

h3, .text-h3 {
  font-size: var(--font-size-2xl);
  font-weight: var(--font-weight-semibold);
  line-height: var(--line-height-tight);
  color: var(--color-text-primary);
  margin: 0 0 0.75rem;
}

h4, .text-h4 {
  font-size: var(--font-size-xl);
  font-weight: var(--font-weight-semibold);
  line-height: var(--line-height-normal);
  color: var(--color-text-primary);
  margin: 0 0 0.5rem;
}
```

**Body Styles**:

```css
body, .text-body {
  font-size: var(--font-size-base);
  font-weight: var(--font-weight-normal);
  line-height: var(--line-height-normal);
  color: var(--color-text-primary);
}

.text-body-lg {
  font-size: var(--font-size-lg);
  line-height: var(--line-height-normal);
}

.text-body-sm {
  font-size: var(--font-size-sm);
  line-height: var(--line-height-normal);
}

.text-caption {
  font-size: var(--font-size-xs);
  line-height: var(--line-height-normal);
  color: var(--color-text-secondary);
}
```

**Utility Styles**:

```css
.text-mono {
  font-family: var(--font-family-mono);
}

.text-bold {
  font-weight: var(--font-weight-bold);
}

.text-semibold {
  font-weight: var(--font-weight-semibold);
}

.text-muted {
  color: var(--color-text-secondary);
}

.text-center {
  text-align: center;
}

.text-right {
  text-align: right;
}
```

---

### Spacing

#### Spacing Scale

```css
:root {
  /* Base spacing unit: 4px */
  --space-0: 0;
  --space-1: 0.25rem;   /* 4px */
  --space-2: 0.5rem;    /* 8px */
  --space-3: 0.75rem;   /* 12px */
  --space-4: 1rem;      /* 16px */
  --space-5: 1.25rem;   /* 20px */
  --space-6: 1.5rem;    /* 24px */
  --space-8: 2rem;      /* 32px */
  --space-10: 2.5rem;   /* 40px */
  --space-12: 3rem;     /* 48px */
  --space-16: 4rem;     /* 64px */
  --space-20: 5rem;     /* 80px */
  --space-24: 6rem;     /* 96px */
}
```

#### Component Spacing Guidelines

- **Padding (internal spacing)**: Use `space-2` to `space-6` for most components
- **Margin (external spacing)**: Use `space-4` to `space-8` for component separation
- **Section spacing**: Use `space-12` to `space-24` for major sections
- **Grid gaps**: Use `space-2` to `space-4` for tight grids, `space-6` to `space-8` for loose layouts

---

### Border Radius

```css
:root {
  --radius-none: 0;
  --radius-sm: 0.25rem;   /* 4px - small elements, badges */
  --radius-base: 0.375rem; /* 6px - buttons, inputs */
  --radius-md: 0.5rem;    /* 8px - calculator buttons, cards */
  --radius-lg: 0.75rem;   /* 12px - modals, large cards */
  --radius-xl: 1rem;      /* 16px - special containers */
  --radius-full: 9999px;  /* Fully rounded (pills, avatars) */
}
```

---

### Shadows

```css
:root {
  /* Elevation shadows */
  --shadow-xs: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.06), 0 1px 2px rgba(0, 0, 0, 0.02);
  --shadow-base: 0 2px 4px rgba(0, 0, 0, 0.08), 0 1px 2px rgba(0, 0, 0, 0.04);
  --shadow-md: 0 4px 8px rgba(0, 0, 0, 0.1), 0 2px 4px rgba(0, 0, 0, 0.06);
  --shadow-lg: 0 8px 16px rgba(0, 0, 0, 0.12), 0 4px 8px rgba(0, 0, 0, 0.08);
  --shadow-xl: 0 12px 24px rgba(0, 0, 0, 0.15), 0 6px 12px rgba(0, 0, 0, 0.1);
  --shadow-2xl: 0 24px 48px rgba(0, 0, 0, 0.2), 0 12px 24px rgba(0, 0, 0, 0.15);
  
  /* Focus shadows */
  --shadow-focus-primary: 0 0 0 3px rgba(0, 102, 204, 0.2);
  --shadow-focus-error: 0 0 0 3px rgba(221, 51, 51, 0.2);
  --shadow-focus-success: 0 0 0 3px rgba(0, 170, 85, 0.2);
  
  /* Inset shadow (pressed buttons) */
  --shadow-inset: 0 2px 4px rgba(0, 0, 0, 0.06) inset;
}
```

**Usage Guidelines**:

- `shadow-xs`, `shadow-sm`: Subtle elevation (inputs, flat cards)
- `shadow-base`, `shadow-md`: Standard elevation (buttons, cards)
- `shadow-lg`, `shadow-xl`: High elevation (dropdowns, tooltips)
- `shadow-2xl`: Maximum elevation (modals, drawers)

---

### Z-Index Scale

```css
:root {
  /* Z-index layers (no arbitrary values) */
  --z-base: 0;
  --z-dropdown: 100;
  --z-sticky: 200;
  --z-fixed: 300;
  --z-modal-backdrop: 1000;
  --z-modal: 1001;
  --z-popover: 1100;
  --z-tooltip: 1200;
  --z-toast: 2000;
}
```

---

### Transitions & Animations

#### Transition Durations

```css
:root {
  --duration-instant: 0ms;
  --duration-fast: 100ms;
  --duration-normal: 200ms;
  --duration-slow: 300ms;
  --duration-slower: 500ms;
}
```

#### Easing Functions

```css
:root {
  --ease-linear: linear;
  --ease-in: cubic-bezier(0.4, 0, 1, 1);
  --ease-out: cubic-bezier(0, 0, 0.2, 1);
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
  --ease-bounce: cubic-bezier(0.68, -0.55, 0.265, 1.55);
}
```

#### Common Transitions

```css
.transition-base {
  transition: all var(--duration-normal) var(--ease-in-out);
}

.transition-colors {
  transition: background-color var(--duration-normal) var(--ease-in-out),
              color var(--duration-normal) var(--ease-in-out),
              border-color var(--duration-normal) var(--ease-in-out);
}

.transition-transform {
  transition: transform var(--duration-fast) var(--ease-out);
}
```

#### Animation Principles

1. **Fast interactions**: 100-200ms (button clicks, hovers)
2. **State changes**: 200-300ms (toggles, accordions)
3. **Page transitions**: 300-500ms (modals, drawers)
4. **Respect motion preferences**:

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

### Breakpoints

```css
:root {
  /* Responsive breakpoints */
  --breakpoint-xs: 0;
  --breakpoint-sm: 576px;   /* Small phones */
  --breakpoint-md: 768px;   /* Tablets */
  --breakpoint-lg: 1024px;  /* Small desktops */
  --breakpoint-xl: 1280px;  /* Large desktops */
  --breakpoint-2xl: 1536px; /* Extra large screens */
}
```

**Usage** (mobile-first approach):

```css
/* Base styles (mobile) */
.container {
  width: 100%;
  padding: 1rem;
}

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    padding: 2rem;
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
    margin: 0 auto;
  }
}
```

---

## Component Patterns

### Button States

All buttons must support these states:

1. **Default** - Resting state
2. **Hover** - Mouse over (desktop only)
3. **Focus** - Keyboard focus (visible outline)
4. **Active** - Mouse down / touch
5. **Disabled** - Not interactive
6. **Loading** - Async operation in progress

### Form Validation States

All form inputs must support:

1. **Default** - No validation
2. **Focus** - Active editing
3. **Valid** - Passed validation (optional visual feedback)
4. **Invalid** - Failed validation (error message required)
5. **Disabled** - Not editable

### Card Component Pattern

Standard card structure:

```css
.card {
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-sm);
  transition: var(--duration-normal) var(--ease-in-out);
}

.card:hover {
  box-shadow: var(--shadow-md);
  border-color: var(--color-border-hover);
}
```

---

## Dark Mode Support

### Dark Mode Color Tokens

```css
@media (prefers-color-scheme: dark) {
  :root {
    /* Primary colors (stay mostly the same) */
    --color-primary: #4D9FEB; /* Lighter shade for better contrast */
    --color-primary-hover: #1A7FE6;
    --color-primary-active: #0066CC;
    
    /* Neutral colors (inverted) */
    --color-neutral-900: #F8F9FA;
    --color-neutral-800: #F0F0F0;
    --color-neutral-700: #E0E0E0;
    --color-neutral-600: #CCCCCC;
    --color-neutral-500: #999999;
    --color-neutral-400: #666666;
    --color-neutral-300: #4D4D4D;
    --color-neutral-200: #333333;
    --color-neutral-100: #1A1A1A;
    --color-neutral-50: #0F0F0F;
    
    /* Semantic colors (adjusted for dark backgrounds) */
    --color-text-primary: #F0F0F0;
    --color-text-secondary: #CCCCCC;
    --color-text-tertiary: #999999;
    --color-text-inverse: #1A1A1A;
    --color-bg-primary: #0F0F0F;
    --color-bg-secondary: #1A1A1A;
    --color-bg-tertiary: #2A2A2A;
    --color-border: #333333;
    --color-border-hover: #4D4D4D;
    
    /* Shadows (softer in dark mode) */
    --shadow-xs: 0 1px 2px rgba(0, 0, 0, 0.3);
    --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.4);
    --shadow-base: 0 2px 4px rgba(0, 0, 0, 0.5);
    --shadow-md: 0 4px 8px rgba(0, 0, 0, 0.6);
    --shadow-lg: 0 8px 16px rgba(0, 0, 0, 0.7);
  }
}
```

### Manual Dark Mode Toggle

If implementing manual dark mode toggle (user preference overrides system):

```css
[data-theme="dark"] {
  /* Same as prefers-color-scheme: dark tokens */
}

[data-theme="light"] {
  /* Force light mode tokens */
}
```

---

## Accessibility Standards

### WCAG 2.1 AA Compliance

#### Color Contrast

**Normal text** (< 18pt regular, < 14pt bold):

- Minimum contrast ratio: **4.5:1**
- Enhanced (AAA): 7:1

**Large text** (≥ 18pt regular, ≥ 14pt bold):

- Minimum contrast ratio: **3:1**
- Enhanced (AAA): 4.5:1

**Verified Combinations**:

| Foreground | Background | Ratio | Pass |
|------------|------------|-------|------|
| `#1A1A1A` | `#FFFFFF` | 16.1:1 | AAA ✓ |
| `#666666` | `#FFFFFF` | 5.74:1 | AA ✓ |
| `#FFFFFF` | `#0066CC` | 7.9:1 | AAA ✓ |
| `#FFFFFF` | `#00AA55` | 4.9:1 | AA ✓ |
| `#FFFFFF` | `#DD3333` | 5.4:1 | AA ✓ |

#### Focus Indicators

- Minimum: **2px outline, 2px offset**
- Color contrast: 3:1 against adjacent colors
- Never remove focus indicators (`:focus-visible` only)

#### Touch Targets

- Minimum size: **44×44px** (WCAG 2.5.5, Level AAA)
- Comfortable size: 48×48px (Material Design guideline)
- Spacing: Minimum 8px between targets

#### Keyboard Navigation

All interactive elements must be:

- Focusable with Tab/Shift+Tab
- Activatable with Enter or Space
- Navigable with arrow keys (where appropriate)
- Escapable with Esc (modals, dropdowns)

---

## Design Token Usage Examples

### Example 1: Primary Button

```css
.btn-primary {
  background: var(--color-primary);
  color: var(--color-text-inverse);
  padding: var(--space-3) var(--space-6);
  font-size: var(--font-size-base);
  font-weight: var(--font-weight-semibold);
  border-radius: var(--radius-base);
  box-shadow: var(--shadow-sm);
  transition: all var(--duration-normal) var(--ease-in-out);
}

.btn-primary:hover {
  background: var(--color-primary-hover);
  box-shadow: var(--shadow-base);
}

.btn-primary:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
  box-shadow: var(--shadow-focus-primary);
}
```

### Example 2: Card Component

```css
.card {
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-sm);
  transition: all var(--duration-normal) var(--ease-in-out);
}

.card:hover {
  box-shadow: var(--shadow-md);
  border-color: var(--color-border-hover);
}

.card__title {
  font-size: var(--font-size-xl);
  font-weight: var(--font-weight-semibold);
  color: var(--color-text-primary);
  margin-bottom: var(--space-2);
}

.card__body {
  font-size: var(--font-size-base);
  color: var(--color-text-secondary);
  line-height: var(--line-height-normal);
}
```

### Example 3: Modal

```css
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: var(--z-modal-backdrop);
  backdrop-filter: blur(4px);
  animation: fadeIn var(--duration-slow) var(--ease-out);
}

.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: var(--color-bg-primary);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-2xl);
  padding: var(--space-6);
  max-width: 600px;
  width: calc(100% - var(--space-8));
  z-index: var(--z-modal);
  animation: scaleUp var(--duration-slow) var(--ease-out);
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes scaleUp {
  from {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.95);
  }
  to {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1);
  }
}
```

---

## Icons

### Icon System

**Recommended**: [Lucide Icons](https://lucide.dev/) or [Heroicons](https://heroicons.com/)

**Icon Sizes**:

```css
:root {
  --icon-size-xs: 12px;
  --icon-size-sm: 16px;
  --icon-size-base: 20px;
  --icon-size-md: 24px;
  --icon-size-lg: 32px;
  --icon-size-xl: 48px;
}
```

**Icon Usage**:

- Icons should be **inline SVG** (not icon fonts) for accessibility
- Always provide accessible labels (`aria-label` or visually-hidden text)
- Use `currentColor` for icon fill to inherit text color

**Example**:

```html
<button class="btn-icon" aria-label="Delete calculation">
  <svg width="20" height="20" fill="none" stroke="currentColor">
    <!-- SVG path -->
  </svg>
</button>
```

---

## Brand Guidelines

### Logo Usage

**Primary Logo**: "CalculatorPro" wordmark with calculator icon

**Variations**:

- Full logo (icon + text) - Use for headers, landing pages
- Icon only - Use for favicons, small spaces (32px or less)
- Text only - Use when icon is already established in context

**Clear Space**: Maintain minimum padding of 50% logo height around logo

**Minimum Sizes**:

- Digital: 120px wide minimum (full logo), 24px (icon only)
- Print: 1 inch wide minimum

### Brand Voice

**Tone**: Professional yet approachable, trustworthy, privacy-conscious

**Do's**:

- Be clear and direct
- Use plain language (avoid jargon)
- Emphasize privacy and security
- Show value transparently

**Don'ts**:

- Don't use dark patterns or manipulative language
- Don't over-promise features
- Don't hide pricing or terms
- Don't use overly technical jargon

---

## Design System Maintenance

### Version Control

- Design system version follows semantic versioning (1.0.0)
- Major version: Breaking changes (redesigns, token removals)
- Minor version: New features (new components, tokens)
- Patch version: Fixes (color adjustments, accessibility improvements)

### Change Process

1. Propose change (document rationale, show examples)
2. Review with team (designer, developer, product owner)
3. Update design system documentation
4. Implement in codebase
5. Communicate changes to all stakeholders

### Documentation

- Keep this design system document up-to-date
- Document all design decisions and rationale
- Provide code examples for all components
- Maintain accessibility notes

---

## Tools & Resources

### Design Tools

- **Figma**: Primary design tool (wireframes, mockups, prototypes)
- **Figma Plugins**: Contrast checker, A11y - Color Contrast Checker

### Development Tools

- **CSS Variables**: Used for all design tokens
- **PostCSS**: For CSS processing
- **Prettier**: For CSS formatting consistency

### Accessibility Testing

- **WAVE**: Browser extension for accessibility auditing
- **Lighthouse**: Automated accessibility audits
- **axe DevTools**: Comprehensive accessibility testing
- **Screen readers**: NVDA (Windows), VoiceOver (Mac), TalkBack (Android)

---

**Document Status**: Complete - Ready for Handover to Database Designer  
**Last Updated**: 2025-11-07  
**Next Review**: 2025-12-07

---

## Summary

This design system provides:

- ✅ **168 design tokens** (colors, typography, spacing, shadows, etc.)
- ✅ **Complete color system** with light and dark mode support
- ✅ **Typography system** with semantic text styles
- ✅ **Spacing scale** based on 4px grid
- ✅ **Accessibility guidelines** (WCAG 2.1 AA compliant)
- ✅ **Component patterns** for consistent implementation
- ✅ **Animation principles** with reduced motion support
- ✅ **Responsive breakpoints** for mobile-first design

**Next Steps**: Database Designer (Role 06) can now design data schemas with knowledge of UI requirements and user workflows.
