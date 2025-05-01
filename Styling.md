## Styling up components

Detailed instructions on how to style up components can be found here:

[Styling up components](styling.md)


## CSS variables

Add the desired value to the css variable. Blank variables shouldn't be removed or commented out so they don't impact the defaults

### Typography
```css
/* Font Families */
--c-heading-font-family: ; /* sans-serif */
--c-body-font-family: ; /* sans-serif */

/* Font Sizes */
--c-text-3xl: ; /* 2.5rem */
--c-text-2xl: ; /* 2.25rem */
--c-text-xxl: ; /* 2rem */
--c-text-xl: ; /* 1.75rem */
--c-text-lg: ; /* 1.25rem */
--c-text-md: ; /* 1rem */
--c-text-sm: ; /* 0.875rem */
--c-text-xs: ; /* 0.75rem */
--c-text-icon: ; /* 1.5rem */

/* Line Heights */
--c-leading-3xl: ; /* 1.2em */
--c-leading-2xl: ; /* 1.2em */
--c-leading-xxl: ; /* 1.2em */
--c-leading-xl: ; /* 1.2em */
--c-leading-lg: ; /* 1.2em */
--c-leading-md: ; /* 1.2em */
--c-leading-sm: ; /* 1.2em */
--c-leading-xs: ; /* 1.66em */
--c-leading-icon: ; /* 1em */

/* Font Weights */
--c-text-bold: ; /* 600 */
--c-text-normal: ; /* 400 */
--c-text-light: ; /* 200 */
```

### Layout & Grid
```css
/* Grid */
--c-grid-alignment-y: ; /* stretch */
--c-grid-alignment-x: ; /* start */
--c-grid-gap-y: ; /* 24px */
--c-grid-gap-x: ; /* 24px */
--c-card-width: ; /* 33.33% */
--c-column-count: ; /* 4 */
--c-nav-count: ; /* 7 */
--c-nav-gap-x: ; /* 8px */
```

### Cards
```css
/* Card Base */
--c-card-border-color: ; /* #000000 */
--c-card-border-width: ; /* 1px */
--c-card-border-radius: ; /* 8px */
--c-card-background-color: ; /* #ffffff */
--c-card-text-color: ; /* #000000 */
--c-card-padding-x: ; /* 16px */
--c-card-padding-y: ; /* 16px */

/* Card Footer */
--c-card-footer-padding-y: ; /* 16px */
--c-card-footer-background-color: ; /* #ffffff */
--c-card-footer-text-color: ; /* #000000 */

/* Card Selected States */
--c-card-selected-background-color: ; /* #000000 */
--c-card-selected-text-color: ; /* #ffffff */
```

### Inputs & Forms
```css
/* Input Base */
--c-input-padding-x: ; /* 8px */
--c-input-padding-y: ; /* 12px */
--c-input-border-width: ; /* 1px */
--c-input-border-color: ; /* #000000 */
--c-input-border-radius: ; /* 8px */
--c-input-background-color: ; /* #ffffff */
--c-input-text-color: ; /* #000000 */

/* Form Fields */
--c-form-field-gap: ; /* 0.5em */
```

### Buttons
```css
/* Button Base */
--c-btn-padding-x: ; /* 16px */
--c-btn-padding-y: ; /* 12px */
--c-btn-border-width: ; /* 1px */
--c-btn-border-color: ; /* transparent */
--c-btn-border-radius: ; /* 8px */
--c-btn-font-size: ; /* 16px */
--c-btn-line-height: ; /* 1.25em */
--c-btn-alignment: ; /* start */
--c-btn-gap-x: ; /* 8px */
--c-btn-gap-y: ; /* 8px */

/* Button Variants */
--c-btn-background-color: ; /* #000000 */
--c-btn-text-color: ; /* #ffffff */
--c-btn-2-border-color: ; /* #000000 */
--c-btn-2-background-color: ; /* #ffffff */
--c-btn-2-text-color: ; /* #000000 */

/* Button Hover States */
--c-btn-hover-border: ; /* #000000 */
--c-btn-hover-background-color: ; /* #ffffff */
--c-btn-hover-text-color: ; /* #000000 */

/* Floating Button */
--c-floating-btn-margin-x: ; /* 24px */
--c-floating-btn-margin-y: ; /* 40px */
```

### Modal
```css
--c-modal-width: ; /* 336px */
--c-modal-background-color: ; /* #00000066 */
--c-modal-footer-background-color: ; /* #ffffff */
--c-modal-footer-text-color: ; /* #000000 */
```

### Status Colors
```css
/* Processing/Pending */
--c-processing-color: ; /* #123bc0 */
--c-processing-text-color: ; /* #ffffff */

/* Danger/Cancelled */
--c-danger-color: ; /* #C72121 */
--c-danger-text-color: ; /* #ffffff */

/* Success/Completed */
--c-success-color: ; /* #78C012 */
--c-success-text-color: ; /* #ffffff */

/* Warning */
--c-warning-color: ; /* #F59F0A */
--c-warning-text-color: ; /* #ffffff */
```

### Links
```css
--c-link-color: ; /* #000 */
--c-link-decoration: ; /* underline */
--c-link-weight: ; /* 700 */
--c-link-hover-color: ; /* #000 */
--c-link-hover-decoration: ; /* underline */
```

### Tags & Badges
```css
/* Tags */
--c-tag-padding-x: ; /* 8px */
--c-tag-padding-y: ; /* 4px */
--c-tag-border-width: ; /* 1px */
--c-tag-border-color: ; /* #000000 */
--c-tag-border-radius: ; /* 8px */
--c-tag-font-size: ; /* 12px */
--c-tag-line-height: ; /* 1.25em */
--c-tag-background-color: ; /* #000000 */
--c-tag-text-color: ; /* #ffffff */

/* Badges */
--c-badge-padding-x: ; /* 8px */
--c-badge-padding-y: ; /* 4px */
--c-badge-border-width: ; /* 1px */
--c-badge-border-color: ; /* #000000 */
--c-badge-border-radius: ; /* 8px */
--c-badge-font-size: ; /* 16px */
--c-badge-line-height: ; /* 1.25em */
--c-badge-background-color: ; /* #000000 */
--c-badge-text-color: ; /* #ffffff */
```

### Slots & Days
```css
/* Slots */
--c-slot-border-radius: ; /* 4px */
--c-slot-border-width: ; /* 1px */
--c-slot-border-color: ; /* #000000 */
--c-slot-available-color: ; /* transparent */
--c-slot-available-text-color: ; /* #000000 */
--c-slot-selected-color: ; /* #000000 */
--c-slot-selected-text-color: ; /* #ffffff */
--c-slot-reserved-color: ; /* #1F1F1F33 */
--c-slot-reserved-text-color: ; /* #1F1F1F33 */
--c-slot-unavailable-color: ; /* #000000 */
--c-slot-booked-color: ; /* #78C012 */
--c-slot-booked-text-color: ; /* #ffffff */
--c-slot-instructor-color: ; /* #123bc0 */
--c-slot-instructor-text-color: ; /* #ffffff */

/* Days */
--c-day-selected-color: ; /* #000000 */
--c-day-selected-text-color: ; /* #ffffff */
--c-day-unavailable-color: ; /* #1F1F1F33 */
--c-day-unavailable-text-color: ; /* #1F1F1F33 */
--c-day-high-color: ; /* #00B828 */
--c-day-high-text-color: ; /* #ffffff */
--c-day-medium-color: ; /* #FFAB1F */
--c-day-medium-text-color: ; /* #ffffff */
--c-day-low-color: ; /* #EE0000 */
--c-day-low-text-color: ; /* #ffffff */
```

### Miscellaneous
```css
/* Labels */
--c-label-text-color: ; /* #000000 */

/* Icons */
--c-icon-size: ; /* 1.5rem */

/* Pagination */
--c-paginate-padding: ; /* 4px */
--c-paginate-background: ; /* transparent */
--c-paginate-border-radius: ; /* 4px */
--c-paginate-border-color: ; /* #1F1F1F */
--c-paginate-text-color: ; /* #1F1F1F */
--c-paginate-background-active: ; /* #1F1F1F */
--c-paginate-text-color-active: ; /* #ffffff */

/* Accent Borders */
--c-accent-border-width: ; /* 1px */
--c-accent-border-color: ; /* #1F1F1F33 */
--c-accent-border-padding-bottom: ; /* 1rem */

/* Position Transforms */
--c-x-translate: ; /* -50% */
--c-y-translate: ; /* -50% */
```





