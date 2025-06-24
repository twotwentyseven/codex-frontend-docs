# SkeletonCard Component

## Overview
The SkeletonCard component provides a flexible loading state interface that displays animated skeleton placeholders while content is being loaded. It features configurable layout structures with row and column arrangements, customizable sizing options, flexbox layout controls, optional border styling, and dynamic skeleton loader generation based on layout configuration.

## Basic Usage
```vue
<codex-skeleton-card
  :enable-border="true"
  :layout="{
    container: [
      { size: 'lg' },
      { row: ['sm', 'md'] },
      { size: 'md' }
    ]
  }"
/>
```

## Key Features
- Configurable layout structure with containers, rows, and columns
- Dynamic skeleton loader generation based on layout configuration
- Flexible sizing options (sm, md, lg, xl, etc.)
- Flexbox layout controls for alignment and spacing
- Optional border styling for card appearance
- Row and column layout arrangements
- Custom width and styling for individual skeleton items
- Animated loading placeholders for improved user experience

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| enableBorder | Boolean | No | false | Whether to display border around the skeleton card |
| layout | Object | No | `{ container: [{ size: 'md' }] }` | Layout configuration defining skeleton structure |

## Layout Configuration

### Layout Object Structure
The layout prop accepts an object with a `container` array that defines the skeleton structure:

```javascript
{
  container: [
    // Simple item
    { size: 'md' },
    
    // Row layout
    { 
      row: ['sm', 'md', 'lg'],
      spaceBetween: true,
      alignItems: 'center'
    },
    
    // Column layout
    { 
      column: [
        { size: 'lg', width: '100%' },
        { size: 'sm', width: '60%' }
      ]
    }
  ]
}
```

### Container Item Types
- **Simple Item**: `{ size: 'md' }` - Single skeleton element
- **Row Layout**: `{ row: [...] }` - Horizontal arrangement of skeleton elements
- **Column Layout**: `{ column: [...] }` - Vertical arrangement of skeleton elements

### Size Options
Standard size classes for skeleton elements:
- **xs**: Extra small skeleton element
- **sm**: Small skeleton element
- **md**: Medium skeleton element (default)
- **lg**: Large skeleton element
- **xl**: Extra large skeleton element

### Layout Properties
| Property | Type | Description |
|----------|------|-------------|
| spaceBetween | Boolean | Applies `justify-content: space-between` to layout |
| justifyContent | String | CSS justify-content value for layout alignment |
| alignItems | String | CSS align-items value for cross-axis alignment |
| flexGrow | String | CSS flex-grow value for element growth |
| width | String | Custom width for skeleton elements |

## CSS Classes
The component applies various CSS classes for styling:
- `_c-skeleton-card`: Base skeleton card styling
- `_c-card`: Card container styling
- `_c-border`: Applied when enableBorder is true
- `_c-skeleton-row`: Row layout container
- `_c-skeleton-column`: Column layout container
- `_c-skeleton-item`: Individual skeleton element
- `_c-skeleton-loader`: Animated loading placeholder
- Size-specific classes: `_c-skeleton-item-sm`, `_c-skeleton-item-md`, etc.

## Layout Patterns
Common layout patterns for different content types:

### Article Card Layout
```javascript
{
  container: [
    { size: 'xl' }, // Title
    { size: 'sm' }, // Subtitle
    { row: ['xs', 'xs'], spaceBetween: true }, // Date and author
    { size: 'lg' }, // Content preview
    { row: ['sm', 'sm', 'sm'] } // Action buttons
  ]
}
```

### User Profile Layout
```javascript
{
  container: [
    { row: [
      { size: 'md', width: '60px' }, // Avatar
      { column: [
        { size: 'md', width: '80%' }, // Name
        { size: 'sm', width: '60%' }  // Email
      ]}
    ]},
    { size: 'lg' } // Bio
  ]
}
```

## Use Cases
Suitable for:
- **Loading States**: Content placeholders while data loads
- **Card Interfaces**: Skeleton versions of card components
- **List Items**: Placeholder for list item content
- **Form Fields**: Loading state for form sections
- **Image Galleries**: Placeholder for media content
- **Dashboard Widgets**: Loading state for dashboard components

## Internationalization
The SkeletonCard component does not require internationalization as it displays loading placeholders without text content.

## Examples

### Basic Skeleton Card
```vue
<template>
  <div class="content-area">
    <codex-skeleton-card v-if="isLoading" />
    <actual-content v-else :data="content" />
  </div>
</template>

<script setup>
const isLoading = ref(true)
const content = ref(null)

onMounted(async () => {
  content.value = await loadContent()
  isLoading.value = false
})
</script>
```

### Article Preview Skeleton
```vue
<template>
  <codex-skeleton-card
    :enable-border="true"
    :layout="{
      container: [
        { size: 'xl' },
        { size: 'md', width: '70%' },
        { row: ['xs', 'xs'], spaceBetween: true },
        { size: 'lg' },
        { size: 'lg' },
        { size: 'md', width: '40%' }
      ]
    }"
  />
</template>
```

### User Card Skeleton
```vue
<template>
  <codex-skeleton-card
    :enable-border="true"
    :layout="{
      container: [
        { row: [
          { size: 'lg', width: '64px' },
          { column: [
            { size: 'md', width: '80%' },
            { size: 'sm', width: '60%' },
            { size: 'xs', width: '40%' }
          ], alignItems: 'flex-start' }
        ], alignItems: 'center' },
        { size: 'lg' },
        { row: ['sm', 'sm', 'sm'], spaceBetween: true }
      ]
    }"
  />
</template>
```

### Product Card Skeleton
```vue
<template>
  <codex-skeleton-card
    :enable-border="true"
    :layout="{
      container: [
        { size: 'xl' },
        { size: 'lg', width: '90%' },
        { size: 'sm', width: '60%' },
        { row: [
          { size: 'md', width: '50%' },
          { size: 'sm', width: '30%' }
        ], spaceBetween: true },
        { size: 'md', width: '100%' }
      ]
    }"
  />
</template>
```

### Dashboard Widget Skeleton
```vue
<template>
  <codex-skeleton-card
    :layout="{
      container: [
        { row: [
          { size: 'md', width: '60%' },
          { size: 'xs', width: '20%' }
        ], spaceBetween: true },
        { column: [
          { size: 'xl' },
          { row: ['sm', 'sm', 'sm'] }
        ] }
      ]
    }"
  />
</template>
```

### List Item Skeleton
```vue
<template>
  <div class="skeleton-list">
    <codex-skeleton-card
      v-for="n in 5"
      :key="n"
      :layout="{
        container: [
          { row: [
            { size: 'md', width: '48px' },
            { column: [
              { size: 'md', width: '80%' },
              { size: 'sm', width: '60%' }
            ] },
            { size: 'xs', width: '24px' }
          ], alignItems: 'center' }
        ]
      }"
    />
  </div>
</template>
```

### Comment Thread Skeleton
```vue
<template>
  <div class="comment-skeleton">
    <codex-skeleton-card
      v-for="level in commentLevels"
      :key="level"
      :style="{ marginLeft: `${level * 20}px` }"
      :layout="{
        container: [
          { row: [
            { size: 'sm', width: '32px' },
            { column: [
              { size: 'sm', width: '70%' },
              { size: 'lg' },
              { row: ['xs', 'xs', 'xs'] }
            ] }
          ], alignItems: 'flex-start' }
        ]
      }"
    />
  </div>
</template>

<script setup>
const commentLevels = [0, 1, 0, 2, 1]
</script>
```

### Form Section Skeleton
```vue
<template>
  <codex-skeleton-card
    :enable-border="true"
    :layout="{
      container: [
        { size: 'lg', width: '40%' },
        { column: [
          { size: 'sm', width: '30%' },
          { size: 'md', width: '100%' }
        ] },
        { column: [
          { size: 'sm', width: '30%' },
          { size: 'md', width: '100%' }
        ] },
        { row: ['md', 'md'], spaceBetween: true }
      ]
    }"
  />
</template>
```

### Media Gallery Skeleton
```vue
<template>
  <div class="gallery-skeleton">
    <codex-skeleton-card
      v-for="n in 6"
      :key="n"
      :layout="{
        container: [
          { size: 'xl' },
          { size: 'md', width: '80%' },
          { row: ['xs', 'xs'], spaceBetween: true }
        ]
      }"
    />
  </div>
</template>

<style scoped>
.gallery-skeleton {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 16px;
}
</style>
```

### Complex Layout Skeleton
```vue
<template>
  <codex-skeleton-card
    :enable-border="true"
    :layout="{
      container: [
        { row: [
          { column: [
            { size: 'xl' },
            { size: 'md', width: '80%' }
          ], flexGrow: '1' },
          { size: 'lg', width: '120px' }
        ], alignItems: 'flex-start' },
        { size: 'lg' },
        { row: [
          { column: [
            { size: 'sm', width: '60%' },
            { size: 'xs', width: '40%' }
          ] },
          { column: [
            { size: 'sm', width: '60%' },
            { size: 'xs', width: '40%' }
          ] }
        ], spaceBetween: true },
        { row: ['sm', 'sm', 'sm', 'sm'] }
      ]
    }"
  />
</template>
```

### Responsive Skeleton Layout
```vue
<template>
  <codex-skeleton-card
    :layout="responsiveLayout"
    :enable-border="true"
  />
</template>

<script setup>
import { computed, ref, onMounted, onUnmounted } from 'vue'

const windowWidth = ref(window.innerWidth)

const responsiveLayout = computed(() => {
  if (windowWidth.value < 768) {
    // Mobile layout
    return {
      container: [
        { size: 'lg' },
        { size: 'md', width: '80%' },
        { column: [
          { size: 'sm', width: '60%' },
          { size: 'sm', width: '40%' }
        ] }
      ]
    }
  } else {
    // Desktop layout
    return {
      container: [
        { row: [
          { size: 'lg', width: '300px' },
          { column: [
            { size: 'xl' },
            { size: 'md', width: '90%' },
            { row: ['sm', 'sm'] }
          ], flexGrow: '1' }
        ], alignItems: 'flex-start' }
      ]
    }
  }
})

const updateWidth = () => {
  windowWidth.value = window.innerWidth
}

onMounted(() => {
  window.addEventListener('resize', updateWidth)
})

onUnmounted(() => {
  window.removeEventListener('resize', updateWidth)
})
</script>
```

### Loading State Manager
```vue
<template>
  <div class="content-manager">
    <div v-if="loadingState.isLoading" class="skeleton-container">
      <codex-skeleton-card
        v-for="(skeleton, index) in loadingState.skeletons"
        :key="index"
        :layout="skeleton.layout"
        :enable-border="skeleton.border"
      />
    </div>
    
    <div v-else class="actual-content">
      <!-- Loaded content -->
    </div>
  </div>
</template>

<script setup>
const loadingState = ref({
  isLoading: true,
  skeletons: [
    {
      layout: {
        container: [
          { size: 'xl' },
          { row: ['md', 'sm'], spaceBetween: true }
        ]
      },
      border: true
    },
    {
      layout: {
        container: [
          { row: [
            { size: 'lg', width: '60px' },
            { column: [{ size: 'md' }, { size: 'sm' }] }
          ] }
        ]
      },
      border: false
    }
  ]
})

// Simulate loading
setTimeout(() => {
  loadingState.value.isLoading = false
}, 2000)
</script>
```

### Dynamic Skeleton Generation
```vue
<template>
  <div class="dynamic-skeletons">
    <codex-skeleton-card
      v-for="(layout, index) in generatedLayouts"
      :key="index"
      :layout="layout"
      :enable-border="true"
    />
  </div>
</template>

<script setup>
const contentTypes = ['article', 'user', 'product', 'comment']

const skeletonLayouts = {
  article: {
    container: [
      { size: 'xl' },
      { size: 'md', width: '70%' },
      { row: ['xs', 'xs'], spaceBetween: true },
      { size: 'lg' }
    ]
  },
  user: {
    container: [
      { row: [
        { size: 'md', width: '48px' },
        { column: [
          { size: 'md', width: '80%' },
          { size: 'sm', width: '60%' }
        ] }
      ], alignItems: 'center' }
    ]
  },
  product: {
    container: [
      { size: 'xl' },
      { size: 'lg', width: '90%' },
      { row: [
        { size: 'md', width: '50%' },
        { size: 'sm', width: '30%' }
      ], spaceBetween: true }
    ]
  },
  comment: {
    container: [
      { row: [
        { size: 'sm', width: '32px' },
        { column: [
          { size: 'sm', width: '70%' },
          { size: 'md' }
        ] }
      ], alignItems: 'flex-start' }
    ]
  }
}

const generatedLayouts = computed(() => {
  return contentTypes.map(type => skeletonLayouts[type])
})
</script>
```

## Best Practices

### Recommended Usage Patterns
- Use skeleton cards that match the structure of actual content
- Implement consistent skeleton patterns across similar content types
- Provide skeleton layouts for different screen sizes
- Use appropriate border styling to match actual card appearance
- Coordinate skeleton timing with actual content loading
- Test skeleton layouts with various content structures
- Use meaningful size variations to create realistic loading states

### Common Pitfalls to Avoid
- Creating skeleton layouts that don't match actual content structure
- Using too many or too few skeleton elements
- Not handling responsive layout changes in skeletons
- Missing border styling coordination with actual cards
- Using inappropriate sizing that doesn't match content
- Not testing skeleton behavior across different loading scenarios
- Creating overly complex layouts that impact performance

### Accessibility Considerations
- Provide appropriate ARIA labels for loading states when needed
- Ensure skeleton animations don't cause motion sensitivity issues
- Use appropriate color contrast for skeleton elements
- Handle keyboard navigation during loading states
- Test with screen readers for loading state announcements
- Provide alternative loading indicators for users who need them

### Performance Considerations
- Use efficient CSS animations for skeleton loading effects
- Minimize the number of skeleton elements for better performance
- Optimize skeleton rendering for large lists or grids
- Use CSS-only animations when possible instead of JavaScript
- Consider lazy loading for skeleton elements in large collections
- Cache skeleton layout configurations when appropriate

### Layout Design
- Design skeleton layouts that closely match actual content proportions
- Use consistent spacing and alignment with loaded content
- Test skeleton layouts with various content types and lengths
- Provide flexible layout options for different use cases
- Handle edge cases where content might be empty or minimal
- Consider content density when designing skeleton structures

### State Management
- Coordinate skeleton display with loading state management
- Handle transitions between skeleton and loaded content smoothly
- Provide appropriate loading duration feedback
- Manage skeleton state across component lifecycles
- Handle error states that might occur during loading
- Synchronize skeleton display with data fetching operations

### Visual Design
- Use subtle animations that enhance user experience without being distracting
- Coordinate skeleton colors with application theme
- Provide appropriate visual hierarchy in skeleton layouts
- Test skeleton appearance across different themes and color schemes
- Handle dark mode and accessibility color requirements
- Ensure skeleton elements are distinguishable but not overwhelming

### Content Matching
- Analyze actual content structure to design appropriate skeletons
- Create skeleton variants for different content types and layouts
- Test skeleton accuracy with real content data
- Handle variable content length and structure in skeleton design
- Provide skeleton layouts for empty states and error conditions
- Update skeleton layouts when content structure changes

### Loading Strategy
- Implement progressive loading with skeleton states
- Coordinate skeleton timing with actual data loading
- Provide feedback for different loading phases
- Handle network delays and loading errors appropriately
- Test skeleton behavior under various network conditions
- Implement intelligent skeleton display duration

### Responsive Design
- Create skeleton layouts that adapt to different screen sizes
- Test skeleton behavior across various device types
- Handle orientation changes and responsive breakpoints
- Coordinate skeleton layouts with responsive content layouts
- Provide appropriate skeleton sizing for mobile and desktop
- Handle touch interactions during skeleton display

## Component Registration
The component is registered as `codex-skeleton-card` in the application. 