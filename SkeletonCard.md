# SkeletonCard Component

## Overview

The `SkeletonCard` component is a customizable loading placeholder that mimics the structure of content before it's loaded. It helps improve user experience by reducing perceived loading times and preventing layout shifts when actual content loads.


## Features

- Flexible layout system with rows and columns
- Customizable item sizes (small, medium, large)
- Animated shimmer effect
- Configurable spacing and alignment
- Responsive design

## Examples

The SkeletonCard component is used throughout the application to provide loading states. Here are some examples from the codebase:

### Timetable Component

In the Timetable component, SkeletonCard is used to show loading states for calendar cards and event listings:

```vue
<!-- Calendar loading state -->
<div v-else class="_c-flex _c-nowrap _c-gap-sm">
  <slot name="skeleton-calendar-card">
    <codex-skeleton-card v-for="i in 7" :key="i" />
  </slot>
</div>

<!-- Event card loading state -->
<div class="_c-event-card-container" v-for="i in 4" :key="i">
  <slot name="skeleton-event-card">
    <codex-skeleton-card :layout="{container: [
      {row: [{ size: 'sm', width: '122px' }, { size: 'sm', width: '93px' }], spaceBetween: true},
      {column: ['lg','sm']},
      {row: ['lg', { size: 'lg', width: '40px' }]}
    ]}" />
  </slot>
</div>
```

## Integration with Components

The SkeletonCard component can be easily integrated with other components in your application to provide a consistent loading experience:

### Using with v-if/v-else

The most common pattern is to use the SkeletonCard with v-if/v-else to show loading states:

```vue
<template>
  <!-- Loading state -->
  <div v-if="loading">
    <codex-skeleton-card :layout="cardLayout" />
  </div>
  
  <!-- Loaded content -->
  <div v-else>
    <!-- Your actual content here -->
  </div>
</template>
```

### Using with Slots

You can use slots to provide custom skeleton loaders for components:

```vue
<template>
  <MyComponent>
    <template #loading>
      <codex-skeleton-card :layout="customLayout" />
    </template>
    
    <template #default>
      <!-- Content when loaded -->
    </template>
  </MyComponent>
</template>
```

### Using in Lists

For lists or grids of items, you can use v-for to create multiple skeleton cards:

```vue
<template>
  <div class="grid">
    <template v-if="loading">
      <codex-skeleton-card 
        v-for="i in 6" 
        :key="`skeleton-${i}`" 
        :layout="itemLayout" 
        class="grid-item"
      />
    </template>
    
    <template v-else>
      <div v-for="item in items" :key="item.id" class="grid-item">
        <!-- Item content -->
      </div>
    </template>
  </div>
</template>
```

## Best Practices

To get the most out of the SkeletonCard component, follow these best practices:

### 1. Match the Skeleton Structure to the Content

Design your skeleton layout to closely match the structure of the actual content. This helps prevent layout shifts when the content loads and provides a more accurate preview of what's coming.

```vue
<!-- Good: Skeleton matches content structure -->
<codex-skeleton-card :layout="{
  container: [
    { row: [{ size: 'md', width: '30%' }, { size: 'sm', width: '20%' }], spaceBetween: true },
    { size: 'sm', width: '100%' },
    { size: 'sm', width: '90%' }
  ]
}" />

<!-- Content with similar structure -->
<div class="card">
  <div class="header">
    <h3>Card Title</h3>
    <span class="date">Today</span>
  </div>
  <p class="description">First line of description</p>
  <p class="description">Second line of description</p>
</div>
```

### 2. Use Consistent Sizing

Use consistent sizing between your skeleton and actual content to minimize layout shifts:

```vue
<!-- Skeleton -->
<codex-skeleton-card :layout="{
  container: [
    { size: 'lg', width: '80px' }, // 80px matches the image size
  ]
}" />

<!-- Content -->
<img src="avatar.jpg" width="80" height="80" />
```

### 3. Provide Appropriate Number of Items

When showing a list of skeleton items, try to match the expected number of items or use a reasonable default:

```vue
<!-- Show the same number of skeletons as expected items -->
<codex-skeleton-card 
  v-for="i in expectedItemCount" 
  :key="`skeleton-${i}`" 
  :layout="itemLayout" 
/>

<!-- Or use a reasonable default if count is unknown -->
<codex-skeleton-card 
  v-for="i in 3" 
  :key="`skeleton-${i}`" 
  :layout="itemLayout" 
/>
```

### 4. Apply Custom Styling When Needed

You can apply custom CSS classes to the SkeletonCard to match your design system:

```vue
<codex-skeleton-card 
  :layout="layout" 
  class="custom-card-style" 
/>

<style>
.custom-card-style {
  --c-card-border-radius: 12px;
  --c-_c-skeleton-card-background: #f0f0f0;
  --c-grid-gap-y: 12px;
}
</style>
```

### 5. Use Slots for Complex Scenarios

For complex loading scenarios, use slots to compose multiple skeleton cards:

```vue
<div class="dashboard">
  <div class="sidebar">
    <codex-skeleton-card :layout="sidebarLayout" />
  </div>
  <div class="main-content">
    <codex-skeleton-card :layout="headerLayout" />
    <div class="grid">
      <codex-skeleton-card 
        v-for="i in 6" 
        :key="`card-${i}`" 
        :layout="cardLayout" 
      />
    </div>
  </div>
</div>
```

## Usage

### Basic Usage

```vue
<template>
  <codex-skeleton-card />
</template>
```

This renders a default skeleton card with a single medium-sized placeholder.

### Custom Layout

The component accepts a `layout` prop that defines the structure of the skeleton:

```vue
<template>
  <codex-skeleton-card :layout="skeletonLayout" />
</template>

<script setup>
const skeletonLayout = {
  container: [
    { 
      row: ['sm', 'sm', 'sm'], 
      spaceBetween: true 
    },
    { 
      column: [
        'md',
        { size: 'lg', width: '75%' }
      ] 
    },
    'sm'
  ]
};
</script>
```

## Layout Configuration

The `layout` prop accepts an object with the following structure:

```js
{
  container: [
    // Container items can be:
    'sm' | 'md' | 'lg', // Simple item with predefined size
    { size: 'sm' | 'md' | 'lg', width: '100px' | '50%' }, // Item with custom width
    { 
      row: [ // Row of items
        'sm' | 'md' | 'lg',
        { size: 'sm' | 'md' | 'lg', width: '100px' | '50%' }
      ],
      spaceBetween: true | false, // Optional: adds space-between to justify-content
      justifyContent: 'flex-start' | 'center' | 'flex-end', // Optional
      alignItems: 'stretch' | 'center' | 'flex-start' | 'flex-end' // Optional
    },
    {
      column: [ // Column of items
        'sm' | 'md' | 'lg',
        { size: 'sm' | 'md' | 'lg', width: '100px' | '50%' }
      ],
      spaceBetween: true | false, // Optional
      justifyContent: 'flex-start' | 'center' | 'flex-end', // Optional
      alignItems: 'stretch' | 'center' | 'flex-start' | 'flex-end' // Optional
    }
  ]
}
```

## Size Reference

- `sm`: 16px height
- `md`: 24px height
- `lg`: 40px height

## Common Layout Examples

### Card with Header and Content

```js
const cardLayout = {
  container: [
    { 
      row: [
        { size: 'md', width: '70%' }, 
        { size: 'sm', width: '20%' }
      ], 
      spaceBetween: true,
      alignItems: 'center'
    },
    { size: 'sm', width: '40%' },
    { 
      column: [
        'md',
        'md',
        'md'
      ]
    }
  ]
};
```

### List Item

```js
const listItemLayout = {
  container: [
    {
      row: [
        { size: 'md', width: '40px' },
        {
          column: [
            { size: 'sm', width: '60%' },
            { size: 'sm', width: '40%' }
          ]
        }
      ],
      alignItems: 'center'
    }
  ]
};
```

### Form Skeleton

```js
const formLayout = {
  container: [
    { size: 'sm', width: '30%' },
    { size: 'md' },
    { size: 'sm', width: '30%' },
    { size: 'md' },
    { size: 'sm', width: '30%' },
    { size: 'md' },
    { size: 'lg', width: '30%' }
  ]
};
```

### Table Row (from Timetable example)

```js
const tableRowLayout = {
  container: [
    {row: [
      { size: 'sm', width: '122px' }, 
      { size: 'sm', width: '93px' }
    ], 
    spaceBetween: true},
    {column: ['lg','sm']},
    {row: ['lg', { size: 'lg', width: '40px' }]}
  ]
};
```

## CSS Variables

The component uses the following CSS variables that can be customized:

- `--c-card-border-colour`: Border color (default: #E5E5E5)
- `--c-card-border-radius`: Border radius (default: 8px)
- `--c-card-padding-y`: Vertical padding (default: 16px)
- `--c-card-padding-x`: Horizontal padding (default: 16px)
- `--c-_c-skeleton-card-background`: Background color (default: #E5E5E5)
- `--c-grid-gap-y`: Vertical gap between items (default: 8px)
- `--c-grid-gap-x`: Horizontal gap between items (default: 8px) 