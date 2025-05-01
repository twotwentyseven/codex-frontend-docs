## HTML Component Structure

Components in our system follow a consistent structure with specific naming conventions. This document outlines the standard structure and class naming patterns used across components like `PlanCard.vue`.

## Class Naming Convention

We use the `_c-` prefix for component-specific classes to:
- Clearly identify component-specific styles
- Prevent style conflicts with other frameworks or libraries
- Maintain consistent naming across the codebase

Examples of class naming:
- `_c-card` - Base component class
- `_c-plan-card` - Specific component type
- `_c-header` - Component section
- `_c-product-title` - Specific element within a component

## Component Structure

A component (or card component) is typically structured with three main sections:

```html
<div class="_c-card _c-[component-type]-card">
    <div class="_c-header">
        <!-- Header content -->
    </div>
    <div class="_c-content">
        <!-- Main content -->
    </div>
    <div class="_c-footer">
        <!-- Footer content -->
    </div>
</div>
```

### Root Element

The root element typically has multiple classes:
- A base class (e.g., `_c-card`)
- A specific component type class (e.g., `_c-plan-card`)
- Conditional classes for different states:
  ```html
  :class="{
    '_c-border': enableBorder, 
    '_c-in-cart': isInCart, 
    '_c-disabled': !canPurchase, 
    '_c-featured': plan.featured 
  }"
  ```

## Header Section

The header section contains introductory content for the component.

```html
<div class="_c-header">
    <slot name="header" 
          :title="title"
          :description="description">
            <!-- Form Title -->
            <codex-title :tag="titleTag" :content="title"/>

            <!-- Message / Prompt -->
            <codex-paragraph tag="p" :content="description"/>
    </slot>
</div>
```

The header slot can be customized in parent components:

```html
<template #header="slotProps">
    <!-- Custom header content using slotProps -->
</template>
```

## Content Section

The content section contains the main information of the component.

```html
<div class="_c-content" :class="{'_c-content-featured': featured}">
    <slot name="content" 
          :instuctors="instructors">
        <!-- Default content -->
        <codex-instructor v-for="instructor in instructors"/>
        
        <!-- Other default content elements -->
    </slot>
</div>
```

The content slot can be customized in parent components:

```html
<template #content="slotProps">
    <!-- Custom content using slotProps -->
</template>
```

## Footer Section

The footer section typically contains actions related or additional information to the component, however is not limited to this.

```html
<div class="_c-footer">
    <slot name="footer" 
          :add="add" 
          :adding="adding" 
          :added="added" 
          :error="error" 
          :formatCurrency="formatCurrency" 
          :canPurchase="canPurchase" 
          :isInCart="isInCart" 
          :start="start" 
          :updateStart="updateStart" 
          :startDates="startDates" 
          :startDateRequired="startDateRequired">
        <!-- Default footer content -->
        <div class="_c-btn-container">
            <codex-button class-name="_c-product-btn">
                <!-- Button content -->
            </codex-button>
        </div>
    </slot>
</div>
```

The footer slot can be customized in parent components:

```html
<template #footer="slotProps">
    <!-- Custom footer content using slotProps -->
</template>
```

## Common Element Classes

Within components, we use consistent class naming for similar elements:

- `_c-badge` - For badges or labels
- `_c-title` - For product titles
- `_c-desc` - For product descriptions
- `_c-focal-text` - For emphasized text (like prices)
- `_c-btn-container` - For button containers
- `_c-btn` - For product-related buttons
- `_c-row` / `_c-column` - For layout direction
- `_c-desc` - For descriptive text
- `_c-subtitle` - For secondary headings

## Conditional Wrappers

We often use conditional wrappers to apply different layouts based on component state:

```html
<codex-conditional-wrapper :condition="featured" class-name="_c-content-column">
    <!-- Content that changes based on the condition -->
</codex-conditional-wrapper>
```

## Styling Components

Detailed instructions on how to style components can be found here:

[Styling Components](styling.md)




