# ConditionalWrapper Component

## Overview
The ConditionalWrapper component provides a flexible atomic element for conditionally rendering content with optional wrapper elements. It features dynamic tag selection based on boolean conditions, slot-based content rendering, and the ability to render content directly without a wrapper when conditions are not met.

## Basic Usage
```vue
<codex-conditional-wrapper
  :condition="showWrapper"
  :tag="'div'"
  :class-name="'wrapper-class'"
>
  <p>Content that may or may not have a wrapper</p>
</codex-conditional-wrapper>
```

## Key Features
- Conditional wrapper rendering based on boolean or numeric conditions
- Dynamic HTML tag selection for wrapper elements
- Template-based rendering when no wrapper is needed
- Slot-based content rendering
- CSS class application for wrapper elements
- Flexible condition evaluation (Boolean or Number)
- Minimal performance impact when condition is false

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| condition | Boolean\|Number | No | false | Condition that determines whether to render wrapper |
| tag | String | No | 'div' | HTML tag to use for wrapper when condition is true |
| className | String | No | '' | CSS class to apply to wrapper element |

## Conditional Rendering Logic
The component uses computed logic for wrapper determination:
- **True Condition**: Renders specified tag with className and slot content
- **False Condition**: Renders content directly in template (no wrapper)
- **Number Evaluation**: Non-zero numbers are treated as truthy
- **Template Mode**: Uses Vue template when no wrapper is needed

## Dynamic Tag Rendering
When condition is true, the component:
- **Tag Processing**: Converts tag prop to lowercase for consistency
- **HTML Elements**: Supports any valid HTML tag (div, span, section, etc.)
- **Component Element**: Uses Vue's `<component>` for dynamic rendering
- **Class Application**: Applies className to wrapper element

## Slot Integration
The component provides:
- **Default Slot**: Renders slot content with or without wrapper
- **Consistent Content**: Slot content remains the same regardless of wrapper
- **Template Rendering**: Direct slot rendering when no wrapper needed
- **Wrapper Rendering**: Slot content inside wrapper element when condition is true

## Performance Considerations
The component optimizes performance by:
- **Template Rendering**: No DOM wrapper when condition is false
- **Computed Properties**: Efficient condition evaluation
- **Minimal Overhead**: Low performance impact for conditional rendering
- **Dynamic Tags**: Only creates wrapper elements when needed

## Use Cases
Suitable for:
- **Conditional Layout**: Wrapping content based on state conditions
- **Responsive Design**: Different wrappers for different screen sizes
- **Feature Flags**: Conditional component wrapping
- **Dynamic Styling**: Conditional CSS class application

## Internationalization
The ConditionalWrapper component does not include built-in internationalization features.

## Examples

### Basic Conditional Wrapper
```vue
<template>
  <codex-conditional-wrapper
    :condition="isWrapped"
    :tag="'div'"
    :class-name="'content-wrapper'"
  >
    <h2>This content may be wrapped</h2>
    <p>Depending on the condition value</p>
  </codex-conditional-wrapper>
</template>

<script setup>
const isWrapped = ref(true)
</script>
```

### Responsive Wrapper
```vue
<template>
  <codex-conditional-wrapper
    :condition="isMobile"
    :tag="'section'"
    :class-name="'mobile-section'"
  >
    <div class="content">
      <h3>Responsive Content</h3>
      <p>Wrapped in section on mobile, bare on desktop</p>
    </div>
  </codex-conditional-wrapper>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const windowWidth = ref(window.innerWidth)
const isMobile = computed(() => windowWidth.value < 768)

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

### Feature Flag Wrapper
```vue
<template>
  <codex-conditional-wrapper
    :condition="featureEnabled"
    :tag="'div'"
    :class-name="'feature-container'"
  >
    <experimental-feature />
  </codex-conditional-wrapper>
</template>

<script setup>
const featureEnabled = ref(false)

// Load feature flag from API or config
onMounted(async () => {
  const config = await loadFeatureFlags()
  featureEnabled.value = config.experimentalFeature
})
</script>
```

### Numeric Condition
```vue
<template>
  <codex-conditional-wrapper
    :condition="itemCount"
    :tag="'ul'"
    :class-name="'item-list'"
  >
    <li v-for="item in items" :key="item.id">
      {{ item.name }}
    </li>
  </codex-conditional-wrapper>
</template>

<script setup>
const items = ref([])
const itemCount = computed(() => items.value.length)

// When itemCount is 0, content renders without <ul> wrapper
// When itemCount > 0, content is wrapped in <ul class="item-list">
</script>
```

### Dynamic Tag Selection
```vue
<template>
  <codex-conditional-wrapper
    :condition="shouldWrap"
    :tag="wrapperTag"
    :class-name="wrapperClass"
  >
    <dynamic-content />
  </codex-conditional-wrapper>
</template>

<script setup>
const contentType = ref('article')
const shouldWrap = ref(true)

const wrapperTag = computed(() => {
  switch (contentType.value) {
    case 'article':
      return 'article'
    case 'sidebar':
      return 'aside'
    case 'navigation':
      return 'nav'
    default:
      return 'div'
  }
})

const wrapperClass = computed(() => `${contentType.value}-wrapper`)
</script>
```

### Form Field Wrapper
```vue
<template>
  <codex-conditional-wrapper
    :condition="hasLabel || hasError"
    :tag="'div'"
    :class-name="'form-field'"
  >
    <label v-if="hasLabel">{{ label }}</label>
    <input v-model="inputValue" />
    <span v-if="hasError" class="error">{{ error }}</span>
  </codex-conditional-wrapper>
</template>

<script setup>
const inputValue = ref('')
const label = ref('Username')
const error = ref('')

const hasLabel = computed(() => !!label.value)
const hasError = computed(() => !!error.value)
</script>
```

### Loading State Wrapper
```vue
<template>
  <codex-conditional-wrapper
    :condition="isLoading"
    :tag="'div'"
    :class-name="'loading-container'"
  >
    <template v-if="isLoading">
      <spinner />
      <p>Loading content...</p>
    </template>
    <template v-else>
      <loaded-content />
    </template>
  </codex-conditional-wrapper>
</template>

<script setup>
const isLoading = ref(true)

onMounted(async () => {
  await loadData()
  isLoading.value = false
})
</script>
```

### Theme-based Wrapper
```vue
<template>
  <codex-conditional-wrapper
    :condition="isDarkTheme"
    :tag="'div'"
    :class-name="'dark-theme-wrapper'"
  >
    <themed-content />
  </codex-conditional-wrapper>
</template>

<script setup>
const theme = ref('light')
const isDarkTheme = computed(() => theme.value === 'dark')

// Content gets dark theme wrapper only when dark theme is active
</script>
```

### Authentication Wrapper
```vue
<template>
  <codex-conditional-wrapper
    :condition="isAuthenticated"
    :tag="'section'"
    :class-name="'authenticated-content'"
  >
    <template v-if="isAuthenticated">
      <user-dashboard />
    </template>
    <template v-else>
      <login-prompt />
    </template>
  </codex-conditional-wrapper>
</template>

<script setup>
import { useAuth } from '@/composables/useAuth'

const { isAuthenticated } = useAuth()
</script>
```

### Error Boundary Wrapper
```vue
<template>
  <codex-conditional-wrapper
    :condition="hasError"
    :tag="'div'"
    :class-name="'error-boundary'"
  >
    <template v-if="hasError">
      <error-message :error="errorMessage" />
    </template>
    <template v-else>
      <safe-content />
    </template>
  </codex-conditional-wrapper>
</template>

<script setup>
const hasError = ref(false)
const errorMessage = ref('')

const handleError = (error) => {
  hasError.value = true
  errorMessage.value = error.message
}
</script>
```

### Multi-condition Wrapper
```vue
<template>
  <codex-conditional-wrapper
    :condition="shouldShowWrapper"
    :tag="wrapperTag"
    :class-name="wrapperClass"
  >
    <multi-state-content />
  </codex-conditional-wrapper>
</template>

<script setup>
const userRole = ref('guest')
const isActive = ref(false)
const hasPermission = ref(false)

const shouldShowWrapper = computed(() => 
  userRole.value !== 'guest' && isActive.value && hasPermission.value
)

const wrapperTag = computed(() => 
  userRole.value === 'admin' ? 'section' : 'div'
)

const wrapperClass = computed(() => 
  `${userRole.value}-content ${isActive.value ? 'active' : 'inactive'}`
)
</script>
```

## CSS Classes
The component applies CSS classes based on props:
- **className**: Applied to wrapper element when condition is true
- **No Classes**: No CSS classes applied when condition is false (template mode)

## Best Practices

### Recommended Usage Patterns
- Use for conditional layout wrapping based on application state
- Apply for responsive design where wrapper elements change
- Implement for feature flag-based conditional rendering
- Use with authentication or permission-based content wrapping
- Apply for theme-based conditional styling
- Use meaningful condition expressions for clarity

### Common Pitfalls to Avoid
- Using complex objects as conditions (use computed booleans instead)
- Not handling the case where content needs to render without wrapper
- Applying CSS that depends on wrapper when condition might be false
- Using the component for simple v-if scenarios where native Vue directives suffice
- Forgetting that className only applies when condition is true

### Accessibility Considerations
- Ensure content remains accessible regardless of wrapper presence
- Use semantic HTML tags when wrapper is needed for document structure
- Handle focus management when wrapper affects keyboard navigation
- Test screen reader compatibility with and without wrapper
- Provide appropriate ARIA attributes through className when necessary

### Performance Considerations
- Use computed properties for complex condition logic
- Minimize re-evaluation of condition by using reactive dependencies efficiently
- Consider the performance impact of frequent condition changes
- Use stable references for tag and className props when possible
- Avoid unnecessary wrapper rendering for purely stylistic purposes

### Condition Logic
- Use clear and descriptive boolean expressions for conditions
- Leverage computed properties for complex condition evaluation
- Handle edge cases where condition might be undefined or null
- Use numeric conditions appropriately (0 is falsy, other numbers are truthy)
- Provide fallback values for conditions that might not be set

### Tag Selection
- Choose semantic HTML tags appropriate for the content structure
- Use div as default for generic wrapper needs
- Consider accessibility implications of tag choice
- Use section, article, aside for semantic document structure
- Avoid using inappropriate tags that might confuse screen readers

### State Management
- Use reactive properties for condition evaluation
- Handle condition changes smoothly without content flashing
- Coordinate wrapper state with parent component logic
- Provide consistent behavior across condition state changes
- Test wrapper behavior during state transitions

### Content Coordination
- Ensure slot content works well with and without wrapper
- Test content layout in both wrapped and unwrapped states
- Handle responsive design considerations for both modes
- Provide appropriate styling for both wrapper and direct content
- Consider content semantics when choosing wrapper tags

## Component Registration
The component is registered as `codex-conditional-wrapper` in the application. 