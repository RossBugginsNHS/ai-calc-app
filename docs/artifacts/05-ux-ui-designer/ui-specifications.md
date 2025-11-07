# UI Specifications: Basic Calculator Web App

**Date Created**: 2025-11-07  
**Role**: UX/UI Designer (Alex Chen)  
**Status**: Initial Draft  
**Version**: 1.0

---

## Overview

This document provides detailed visual and interaction specifications for all UI components in the Basic Calculator Web App. These specs ensure consistent implementation across the application.

### Component Index

1. [Buttons](#buttons)
2. [Display Screen](#display-screen)
3. [History Item Card](#history-item-card)
4. [Input Fields](#input-fields)
5. [Modals](#modals)
6. [Navigation](#navigation)
7. [Toasts & Notifications](#toasts--notifications)
8. [Loading Indicators](#loading-indicators)
9. [Empty States](#empty-states)

---

## Design Tokens Reference

> **Note**: Complete design token values are defined in `design-system.md`

**Quick Reference**:

- Primary color: `#0066CC` (Blue)
- Success color: `#00AA55` (Green)
- Error color: `#DD3333` (Red)
- Warning color: `#FFAA00` (Orange)
- Base font: `Inter, -apple-system, system-ui, sans-serif`
- Body text: `16px / 1.5`
- Border radius: `6px` (standard), `8px` (large), `12px` (cards)

---

## Buttons

### C-BTN-001: Primary Button

**Component ID**: C-BTN-001  
**Category**: Actions  
**Usage**: Primary user actions (calculate, submit, confirm, subscribe)

#### Visual Specifications

**Default State**:

```css
.btn-primary {
  /* Layout */
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 120px;
  height: 44px;
  padding: 12px 24px;
  
  /* Typography */
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  font-weight: 600;
  line-height: 1;
  letter-spacing: 0.01em;
  text-align: center;
  
  /* Colors */
  background: #0066CC;
  color: #FFFFFF;
  border: none;
  
  /* Effects */
  border-radius: 6px;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
  cursor: pointer;
  transition: all 200ms ease;
}
```

**Hover State**:

```css
.btn-primary:hover {
  background: #0052A3;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transform: translateY(-1px);
}
```

**Active/Pressed State**:

```css
.btn-primary:active {
  background: #003D7A;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
  transform: translateY(0);
}
```

**Focus State** (Keyboard Navigation):

```css
.btn-primary:focus-visible {
  outline: 2px solid #0066CC;
  outline-offset: 2px;
  box-shadow: 0 0 0 4px rgba(0, 102, 204, 0.2);
}
```

**Disabled State**:

```css
.btn-primary:disabled {
  background: #CCCCCC;
  color: #666666;
  cursor: not-allowed;
  opacity: 0.6;
  box-shadow: none;
  transform: none;
}
```

#### Size Variants

**Small** (36px height):

```css
.btn-primary.btn-sm {
  height: 36px;
  padding: 8px 16px;
  font-size: 14px;
  min-width: 100px;
}
```

**Large** (52px height):

```css
.btn-primary.btn-lg {
  height: 52px;
  padding: 16px 32px;
  font-size: 18px;
  min-width: 140px;
}
```

**Full-width**:

```css
.btn-primary.btn-block {
  width: 100%;
  min-width: unset;
}
```

#### Accessibility

- **ARIA role**: `button`
- **Keyboard**: Tab (focus), Enter/Space (activate)
- **Screen reader**: Button text must be descriptive and action-oriented
- **Min contrast**: 4.5:1 (WCAG AA) — White text on `#0066CC` = 7.9:1 ✓
- **Touch target**: 44×44px minimum (met)
- **Focus indicator**: 2px outline, 2px offset, high contrast

#### Usage Guidelines

**Do**:

- Use for primary action on a page (e.g., "Calculate", "Subscribe", "Save")
- Ensure button text is action-oriented ("Save Changes", not "OK")
- Use only one primary button per section to establish hierarchy
- Place primary button on the right in multi-button layouts (LTR)

**Don't**:

- Don't use more than one primary button in a single view
- Don't use for navigation (use links or secondary buttons)
- Don't use vague labels ("Click here", "Submit")
- Don't make buttons too wide (max 400px recommended)

#### Examples

```html
<!-- Standard primary button -->
<button class="btn-primary">Subscribe Now</button>

<!-- Primary button with icon -->
<button class="btn-primary">
  <svg class="icon-left">...</svg>
  Start Free Trial
</button>

<!-- Large full-width primary button -->
<button class="btn-primary btn-lg btn-block">
  Complete Purchase
</button>

<!-- Disabled state -->
<button class="btn-primary" disabled>
  Processing...
</button>
```

---

### C-BTN-002: Secondary Button

**Component ID**: C-BTN-002  
**Category**: Actions  
**Usage**: Secondary actions (cancel, dismiss, alternative option)

#### Visual Specifications

**Default State**:

```css
.btn-secondary {
  /* Layout */
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 120px;
  height: 44px;
  padding: 12px 24px;
  
  /* Typography */
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  font-weight: 600;
  line-height: 1;
  
  /* Colors */
  background: #FFFFFF;
  color: #0066CC;
  border: 2px solid #0066CC;
  
  /* Effects */
  border-radius: 6px;
  cursor: pointer;
  transition: all 200ms ease;
}
```

**Hover State**:

```css
.btn-secondary:hover {
  background: #F0F7FF;
  border-color: #0052A3;
  color: #0052A3;
}
```

**Active State**:

```css
.btn-secondary:active {
  background: #E6F1FF;
  border-color: #003D7A;
  color: #003D7A;
}
```

---

### C-BTN-003: Calculator Number Button

**Component ID**: C-BTN-003  
**Category**: Calculator Input  
**Usage**: Number buttons (0-9) on calculator keypad

#### Visual Specifications

**Default State**:

```css
.btn-calculator-number {
  /* Layout */
  display: flex;
  align-items: center;
  justify-content: center;
  width: 80px;
  height: 80px;
  
  /* Typography */
  font-family: 'Inter', sans-serif;
  font-size: 28px;
  font-weight: 500;
  
  /* Colors */
  background: #FFFFFF;
  color: #1A1A1A;
  border: 1px solid #E0E0E0;
  
  /* Effects */
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.06);
  cursor: pointer;
  transition: all 100ms ease;
  user-select: none;
}
```

**Hover State**:

```css
.btn-calculator-number:hover {
  background: #F5F5F5;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.08);
}
```

**Active State** (Clicked/Tapped):

```css
.btn-calculator-number:active {
  background: #E8E8E8;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05) inset;
  transform: scale(0.97);
}
```

**Focus State**:

```css
.btn-calculator-number:focus-visible {
  outline: 2px solid #0066CC;
  outline-offset: 2px;
  z-index: 1;
}
```

#### Responsive Sizes

**Desktop** (1024px+): `80px × 80px`  
**Tablet** (768px - 1023px): `70px × 70px`  
**Mobile** (< 768px): `64px × 64px`

#### Touch Feedback

- **Haptic**: Light impact on tap (mobile devices)
- **Visual**: Scale down to 97% on tap, instant feedback
- **Audio**: Optional click sound (user preference)

#### Accessibility

- **Touch target**: 64-80px (exceeds 44px minimum)
- **Keyboard**: 0-9 keys trigger respective buttons
- **Screen reader**: Announces number value ("Button, 5")
- **High contrast mode**: Ensure visible borders

---

### C-BTN-004: Calculator Operation Button

**Component ID**: C-BTN-004  
**Category**: Calculator Input  
**Usage**: Operation buttons (+, −, ×, ÷, =)

#### Visual Specifications

**Default State**:

```css
.btn-calculator-operation {
  /* Layout */
  display: flex;
  align-items: center;
  justify-content: center;
  width: 80px;
  height: 80px;
  
  /* Typography */
  font-family: 'Inter', sans-serif;
  font-size: 28px;
  font-weight: 600;
  
  /* Colors */
  background: #0066CC;
  color: #FFFFFF;
  border: none;
  
  /* Effects */
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 102, 204, 0.3);
  cursor: pointer;
  transition: all 100ms ease;
  user-select: none;
}
```

**Hover State**:

```css
.btn-calculator-operation:hover {
  background: #0052A3;
  box-shadow: 0 2px 4px rgba(0, 102, 204, 0.4);
}
```

**Active State**:

```css
.btn-calculator-operation:active {
  background: #003D7A;
  box-shadow: 0 1px 2px rgba(0, 102, 204, 0.2) inset;
  transform: scale(0.97);
}
```

**Selected State** (Current Operation):

```css
.btn-calculator-operation.is-selected {
  background: #003D7A;
  box-shadow: 0 0 0 2px #FFAA00;
  outline: 2px solid #FFAA00;
  outline-offset: 2px;
}
```

#### Accessibility

- **Keyboard shortcuts**: 
  - `+` for addition
  - `-` for subtraction
  - `*` for multiplication
  - `/` for division
  - `Enter` or `=` for equals
- **Screen reader**: Announces operation ("Button, multiply")
- **Contrast**: White on `#0066CC` = 7.9:1 (AAA compliant)

---

### C-BTN-005: Clear/Function Button

**Component ID**: C-BTN-005  
**Category**: Calculator Input  
**Usage**: Clear (C), Backspace (⌫), and other utility functions

#### Visual Specifications

**Default State**:

```css
.btn-calculator-clear {
  /* Layout */
  display: flex;
  align-items: center;
  justify-content: center;
  width: 80px;
  height: 80px;
  
  /* Typography */
  font-family: 'Inter', sans-serif;
  font-size: 20px;
  font-weight: 600;
  
  /* Colors */
  background: #FFF5E6;
  color: #CC5500;
  border: 1px solid #FFD699;
  
  /* Effects */
  border-radius: 8px;
  box-shadow: 0 1px 2px rgba(204, 85, 0, 0.1);
  cursor: pointer;
  transition: all 100ms ease;
  user-select: none;
}
```

**Hover State**:

```css
.btn-calculator-clear:hover {
  background: #FFEBCC;
  border-color: #FFBB66;
}
```

**Active State**:

```css
.btn-calculator-clear:active {
  background: #FFE0B2;
  transform: scale(0.97);
}
```

#### Accessibility

- **Keyboard shortcuts**: 
  - `Escape` for Clear (C)
  - `Backspace` for delete last character (⌫)
- **Screen reader**: "Button, Clear" or "Button, Backspace"

---

### C-BTN-006: Icon Button

**Component ID**: C-BTN-006  
**Category**: Actions  
**Usage**: Icon-only buttons (copy, delete, settings, close)

#### Visual Specifications

**Default State**:

```css
.btn-icon {
  /* Layout */
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  padding: 0;
  
  /* Colors */
  background: transparent;
  color: #666666;
  border: none;
  
  /* Effects */
  border-radius: 6px;
  cursor: pointer;
  transition: all 150ms ease;
}

.btn-icon svg {
  width: 20px;
  height: 20px;
}
```

**Hover State**:

```css
.btn-icon:hover {
  background: #F0F0F0;
  color: #1A1A1A;
}
```

**Active State**:

```css
.btn-icon:active {
  background: #E0E0E0;
  color: #000000;
}
```

#### Variants

**Danger** (Delete):

```css
.btn-icon.btn-icon-danger:hover {
  background: #FFEAEA;
  color: #DD3333;
}
```

**Success** (Confirm):

```css
.btn-icon.btn-icon-success:hover {
  background: #E6F7ED;
  color: #00AA55;
}
```

#### Accessibility

- **ARIA label required**: Since no visible text, must have descriptive label
- **Example**: `<button class="btn-icon" aria-label="Delete calculation">...</button>`
- **Touch target**: 36px minimum for small icons, 44px for primary actions
- **Screen reader**: Reads the ARIA label ("Delete calculation button")

---

## Display Screen

### C-DISP-001: Calculator Display

**Component ID**: C-DISP-001  
**Category**: Output  
**Usage**: Shows current calculation, expression, and result

#### Visual Specifications

```css
.calculator-display {
  /* Layout */
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: flex-end;
  width: 100%;
  height: 100px;
  padding: 16px 20px;
  
  /* Colors */
  background: #F8F9FA;
  border: 1px solid #E0E0E0;
  border-radius: 8px 8px 0 0;
  
  /* Typography */
  font-family: 'SF Mono', 'Monaco', 'Consolas', monospace;
  text-align: right;
}

.calculator-display__expression {
  font-size: 16px;
  color: #666666;
  margin-bottom: 8px;
  min-height: 20px;
}

.calculator-display__result {
  font-size: 36px;
  font-weight: 600;
  color: #1A1A1A;
  line-height: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  max-width: 100%;
}
```

#### States

**Empty State**:

```css
.calculator-display__result--empty {
  color: #CCCCCC;
}
/* Shows "0" */
```

**Error State**:

```css
.calculator-display__result--error {
  color: #DD3333;
  font-size: 18px;
  white-space: normal;
  text-align: right;
}
/* Shows "Cannot divide by zero" or similar */
```

**Calculating State** (Animation):

```css
.calculator-display__result--calculating::after {
  content: '...';
  animation: ellipsis 1.5s infinite;
}

@keyframes ellipsis {
  0% { content: ''; }
  33% { content: '.'; }
  66% { content: '..'; }
  100% { content: '...'; }
}
```

#### Responsive Behavior

**Desktop**: Result `36px`, Expression `16px`  
**Tablet**: Result `32px`, Expression `14px`  
**Mobile**: Result `28px`, Expression `13px`

#### Accessibility

- **ARIA live region**: Announces result changes to screen readers
- **Example**: `<div class="calculator-display" role="region" aria-live="polite" aria-atomic="true">`
- **Screen reader**: "Calculation result: 42"

---

## History Item Card

### C-HIST-ITEM-001: History Item

**Component ID**: C-HIST-ITEM-001  
**Category**: Content Display  
**Usage**: Individual calculation in history list

#### Visual Specifications

```css
.history-item {
  /* Layout */
  display: flex;
  flex-direction: column;
  padding: 12px 16px;
  gap: 8px;
  
  /* Colors */
  background: #FFFFFF;
  border: 1px solid #E0E0E0;
  border-radius: 8px;
  
  /* Effects */
  transition: all 200ms ease;
  cursor: pointer;
}

.history-item:hover {
  background: #F8F9FA;
  border-color: #CCCCCC;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}

.history-item__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.history-item__expression {
  font-family: 'SF Mono', monospace;
  font-size: 16px;
  font-weight: 600;
  color: #1A1A1A;
}

.history-item__actions {
  display: flex;
  gap: 4px;
  opacity: 0;
  transition: opacity 200ms ease;
}

.history-item:hover .history-item__actions {
  opacity: 1;
}

.history-item__note {
  font-size: 14px;
  color: #666666;
  margin-top: 4px;
}

.history-item__timestamp {
  font-size: 12px;
  color: #999999;
  margin-top: 4px;
}
```

#### States

**Selected State** (Clicked):

```css
.history-item.is-selected {
  background: #E6F1FF;
  border-color: #0066CC;
}
```

**Pinned State** (Premium):

```css
.history-item.is-pinned {
  border-left: 4px solid #FFAA00;
  background: #FFFBF0;
}

.history-item.is-pinned::before {
  content: '📌';
  margin-right: 8px;
}
```

**Search Highlight**:

```css
.history-item__expression mark,
.history-item__note mark {
  background: #FFEB3B;
  color: #1A1A1A;
  font-weight: 600;
  padding: 0 2px;
  border-radius: 2px;
}
```

#### Swipe Gestures (Mobile)

**Swipe Left** → Reveal delete action  
**Swipe Right** → Reveal copy action

```css
.history-item__swipe-action {
  position: absolute;
  right: 0;
  top: 0;
  height: 100%;
  padding: 0 20px;
  display: flex;
  align-items: center;
  background: #DD3333; /* Delete */
  color: #FFFFFF;
  transform: translateX(100%);
  transition: transform 250ms ease;
}

.history-item.is-swiping-left .history-item__swipe-action {
  transform: translateX(0);
}
```

#### Accessibility

- **Keyboard**: Arrow keys to navigate, Enter to select, Delete to remove
- **Screen reader**: "Calculation: 125 multiplied by 8 equals 1,000. Time: Today at 10:23 AM. Actions: Copy, Delete."
- **Focus indicator**: 2px outline when focused

---

## Input Fields

### C-INPUT-001: Text Input

**Component ID**: C-INPUT-001  
**Category**: Form Input  
**Usage**: Email, search, text entry

#### Visual Specifications

```css
.input-text {
  /* Layout */
  width: 100%;
  height: 44px;
  padding: 12px 16px;
  
  /* Typography */
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  line-height: 1.5;
  color: #1A1A1A;
  
  /* Colors */
  background: #FFFFFF;
  border: 1px solid #CCCCCC;
  border-radius: 6px;
  
  /* Effects */
  transition: all 200ms ease;
  outline: none;
}

.input-text::placeholder {
  color: #999999;
}

.input-text:hover {
  border-color: #999999;
}

.input-text:focus {
  border-color: #0066CC;
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.1);
}
```

#### States

**Error State**:

```css
.input-text.is-error {
  border-color: #DD3333;
}

.input-text.is-error:focus {
  border-color: #DD3333;
  box-shadow: 0 0 0 3px rgba(221, 51, 51, 0.1);
}

.input-text + .input-error-message {
  display: block;
  margin-top: 4px;
  font-size: 14px;
  color: #DD3333;
}
```

**Disabled State**:

```css
.input-text:disabled {
  background: #F5F5F5;
  color: #999999;
  border-color: #E0E0E0;
  cursor: not-allowed;
}
```

**Success State** (Validated):

```css
.input-text.is-success {
  border-color: #00AA55;
}

.input-text.is-success:focus {
  border-color: #00AA55;
  box-shadow: 0 0 0 3px rgba(0, 170, 85, 0.1);
}
```

#### With Icon

```css
.input-wrapper {
  position: relative;
}

.input-wrapper .input-icon {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  color: #999999;
}

.input-wrapper .input-text {
  padding-left: 40px;
}
```

#### Accessibility

- **Label**: Always include visible or visually-hidden label
- **Error messaging**: Associate errors with `aria-describedby`
- **Placeholder**: Not a replacement for labels
- **Focus**: Clear focus indicator (met by focus state)

---

### C-INPUT-002: Search Input

**Component ID**: C-INPUT-002  
**Category**: Form Input  
**Usage**: Search calculation history

#### Visual Specifications

```css
.input-search {
  /* Extends input-text */
  /* Layout */
  width: 100%;
  height: 40px;
  padding: 8px 40px 8px 40px;
  
  /* Typography */
  font-size: 14px;
  
  /* Colors */
  background: #F8F9FA;
  border: 1px solid #E0E0E0;
  border-radius: 20px;
}

.input-search-icon {
  position: absolute;
  left: 14px;
  top: 50%;
  transform: translateY(-50%);
  width: 16px;
  height: 16px;
  color: #999999;
}

.input-search-clear {
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: transparent;
  color: #999999;
  cursor: pointer;
  display: none;
}

.input-search:not(:placeholder-shown) ~ .input-search-clear {
  display: flex;
}
```

#### Accessibility

- **ARIA role**: `searchbox`
- **Keyboard**: Escape to clear search
- **Screen reader**: "Search history, search box"

---

## Modals

### C-MODAL-001: Modal Dialog

**Component ID**: C-MODAL-001  
**Category**: Overlay  
**Usage**: Premium upgrade, settings, auth forms

#### Visual Specifications

```css
.modal-overlay {
  /* Layout */
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1000;
  
  /* Colors */
  background: rgba(0, 0, 0, 0.5);
  
  /* Effects */
  opacity: 0;
  transition: opacity 300ms ease;
  backdrop-filter: blur(4px);
}

.modal-overlay.is-open {
  opacity: 1;
}

.modal {
  /* Layout */
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0.95);
  max-width: 600px;
  width: calc(100% - 32px);
  max-height: calc(100vh - 32px);
  overflow: auto;
  
  /* Colors */
  background: #FFFFFF;
  border-radius: 12px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
  
  /* Effects */
  opacity: 0;
  transition: all 300ms cubic-bezier(0.4, 0, 0.2, 1);
}

.modal-overlay.is-open .modal {
  opacity: 1;
  transform: translate(-50%, -50%) scale(1);
}

.modal__header {
  padding: 24px 24px 16px;
  border-bottom: 1px solid #E0E0E0;
  position: relative;
}

.modal__title {
  font-size: 24px;
  font-weight: 700;
  color: #1A1A1A;
  margin: 0;
}

.modal__close {
  position: absolute;
  top: 20px;
  right: 20px;
  width: 32px;
  height: 32px;
  background: transparent;
  border: none;
  cursor: pointer;
  color: #666666;
  transition: color 200ms ease;
}

.modal__close:hover {
  color: #1A1A1A;
}

.modal__body {
  padding: 24px;
}

.modal__footer {
  padding: 16px 24px;
  border-top: 1px solid #E0E0E0;
  display: flex;
  justify-content: flex-end;
  gap: 12px;
}
```

#### Mobile Behavior

```css
@media (max-width: 767px) {
  .modal {
    top: auto;
    bottom: 0;
    left: 0;
    transform: translateY(100%);
    width: 100%;
    max-width: 100%;
    max-height: 90vh;
    border-radius: 12px 12px 0 0;
  }
  
  .modal-overlay.is-open .modal {
    transform: translateY(0);
  }
}
```

#### Accessibility

- **Focus trap**: Keep focus within modal when open
- **ARIA**: `role="dialog"`, `aria-modal="true"`, `aria-labelledby="modal-title"`
- **Keyboard**: Escape to close, Tab to cycle through focusable elements
- **Screen reader**: Announces modal opening and title

---

## Toasts & Notifications

### C-TOAST-001: Toast Notification

**Component ID**: C-TOAST-001  
**Category**: Feedback  
**Usage**: Success messages, errors, warnings

#### Visual Specifications

```css
.toast {
  /* Layout */
  position: fixed;
  bottom: 24px;
  right: 24px;
  min-width: 320px;
  max-width: 500px;
  padding: 16px 20px;
  z-index: 2000;
  
  /* Colors */
  background: #1A1A1A;
  color: #FFFFFF;
  border-radius: 8px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.3);
  
  /* Typography */
  font-size: 15px;
  line-height: 1.5;
  
  /* Effects */
  opacity: 0;
  transform: translateY(20px);
  transition: all 300ms cubic-bezier(0.4, 0, 0.2, 1);
}

.toast.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

#### Variants

**Success**:

```css
.toast.toast-success {
  background: #00AA55;
  color: #FFFFFF;
}

.toast.toast-success::before {
  content: '✓';
  margin-right: 8px;
  font-weight: bold;
}
```

**Error**:

```css
.toast.toast-error {
  background: #DD3333;
  color: #FFFFFF;
}

.toast.toast-error::before {
  content: '✕';
  margin-right: 8px;
  font-weight: bold;
}
```

**Warning**:

```css
.toast.toast-warning {
  background: #FFAA00;
  color: #1A1A1A;
}

.toast.toast-warning::before {
  content: '⚠';
  margin-right: 8px;
}
```

#### Accessibility

- **ARIA**: `role="alert"` (for errors) or `role="status"` (for success)
- **Auto-dismiss**: 4 seconds default, 6 seconds for errors
- **Screen reader**: Announces immediately
- **Keyboard**: Escape to dismiss

---

## Loading Indicators

### C-LOAD-001: Spinner

**Component ID**: C-LOAD-001  
**Category**: Feedback  
**Usage**: Loading states, async operations

```css
.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #E0E0E0;
  border-top-color: #0066CC;
  border-radius: 50%;
  animation: spin 800ms linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.spinner-sm {
  width: 20px;
  height: 20px;
  border-width: 2px;
}

.spinner-lg {
  width: 60px;
  height: 60px;
  border-width: 6px;
}
```

---

### C-LOAD-002: Skeleton Screen

**Component ID**: C-LOAD-002  
**Category**: Feedback  
**Usage**: Initial page load, content placeholder

```css
.skeleton {
  background: linear-gradient(
    90deg,
    #F0F0F0 0%,
    #E0E0E0 50%,
    #F0F0F0 100%
  );
  background-size: 200% 100%;
  animation: skeleton-loading 1.5s ease-in-out infinite;
  border-radius: 4px;
}

@keyframes skeleton-loading {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

.skeleton-text {
  height: 16px;
  margin-bottom: 8px;
}

.skeleton-title {
  height: 24px;
  width: 60%;
}

.skeleton-button {
  height: 44px;
  width: 120px;
  border-radius: 6px;
}
```

---

## Empty States

### C-EMPTY-001: Empty State

**Component ID**: C-EMPTY-001  
**Category**: Feedback  
**Usage**: No results, first-time experience

```css
.empty-state {
  /* Layout */
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 48px 24px;
  text-align: center;
}

.empty-state__icon {
  font-size: 64px;
  margin-bottom: 16px;
  opacity: 0.5;
}

.empty-state__title {
  font-size: 20px;
  font-weight: 600;
  color: #1A1A1A;
  margin-bottom: 8px;
}

.empty-state__description {
  font-size: 15px;
  color: #666666;
  max-width: 400px;
  margin-bottom: 24px;
  line-height: 1.5;
}

.empty-state__action {
  /* Primary button styles */
}
```

---

## Component Usage Matrix

| Component | Free Tier | Premium | Mobile | Desktop | Keyboard | Screen Reader |
|-----------|-----------|---------|--------|---------|----------|---------------|
| BTN-001 (Primary) | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| BTN-003 (Calc Number) | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| BTN-004 (Calc Op) | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| DISP-001 (Display) | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| HIST-ITEM-001 | ✓ | ✓ (enhanced) | ✓ | ✓ | ✓ | ✓ |
| INPUT-002 (Search) | ✗ | ✓ | ✓ | ✓ | ✓ | ✓ |
| MODAL-001 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| TOAST-001 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

---

**Document Status**: Ready for Design System Finalization  
**Last Updated**: 2025-11-07  
**Next Review**: 2025-12-07
