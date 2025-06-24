# Subtitle Component

## Overview
The Subtitle component provides a flexible atomic element for rendering subtitle headings with dynamic HTML tag selection. It supports custom content, CSS class application, and semantic HTML structure for creating properly hierarchical subtitle elements throughout the application.

## Basic Usage
```vue
<codex-subtitle
  :content="'Section Overview'"
  :tag="'h3'"
  :class-name="'section-subtitle'"
/>
```

## Key Features
- Dynamic HTML tag rendering (h1-h6, div, span, etc.)
- Custom content display with string support
- CSS class customization through className prop
- Semantic HTML structure for accessibility
- Flexible tag selection for proper heading hierarchy
- Lightweight and minimal structure

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| tag | String | No | 'h2' | HTML tag to render (h1, h2, h3, h4, h5, h6, div, span, etc.) |
| content | String | No | - | Text content to display in the subtitle |
| className | String | No | - | Additional CSS classes to apply |

## Events
The Subtitle component does not emit any custom events.

## Slots
The Subtitle component does not use slots - content is passed via the `content` prop.

## Tag Selection Guidelines
- **h2**: Default subtitle level, main section subtitles
- **h3**: Sub-section subtitles under h2 headings
- **h4**: Minor section subtitles under h3 headings
- **h5**: Deep nested subtitles under h4 headings
- **h6**: Deepest nested subtitles under h5 headings
- **div/span**: Non-semantic subtitle styling without heading hierarchy

## Semantic HTML Structure
The component renders semantic HTML based on tag selection:
- **Heading tags (h1-h6)**: Contribute to document outline and accessibility
- **Generic tags (div/span)**: Provide styling without semantic meaning
- **Proper hierarchy**: Maintains logical heading structure for screen readers

## Internationalization
The Subtitle component does not include built-in internationalization. Content should be localized before passing to the component.

## Examples

### Basic Subtitle
```vue
<codex-subtitle
  :content="'Getting Started'"
/>
```

### Custom Tag Subtitle
```vue
<codex-subtitle
  :content="'Product Features'"
  :tag="'h3'"
/>
```

### Styled Subtitle with Custom Classes
```vue
<codex-subtitle
  :content="'Advanced Configuration'"
  :tag="'h4'"
  :class-name="'advanced-section highlight-subtitle'"
/>
```

### Page Section Structure
```vue
<template>
  <main class="documentation-page">
    <codex-title :content="'User Guide'" :tag="'h1'" />
    
    <section class="intro-section">
      <codex-subtitle 
        :content="'Introduction'" 
        :tag="'h2'"
        :class-name="'section-header'"
      />
      <codex-paragraph :content="'Welcome to our platform...'" />
    </section>
    
    <section class="features-section">
      <codex-subtitle 
        :content="'Key Features'" 
        :tag="'h2'"
        :class-name="'section-header'"
      />
      
      <div class="feature-group">
        <codex-subtitle 
          :content="'Authentication'" 
          :tag="'h3'"
          :class-name="'feature-subtitle'"
        />
        <codex-paragraph :content="'Secure login and registration...'" />
        
        <codex-subtitle 
          :content="'User Management'" 
          :tag="'h4'"
          :class-name="'sub-feature-title'"
        />
        <codex-paragraph :content="'Profile management and settings...'" />
      </div>
    </section>
  </main>
</template>
```

### Dynamic Subtitle Content
```vue
<template>
  <div class="dynamic-content">
    <codex-subtitle 
      :content="currentSectionTitle" 
      :tag="'h2'"
      :class-name="'dynamic-section-title'"
    />
  </div>
</template>

<script setup>
import { computed } from 'vue'

const currentStep = ref(1)

const currentSectionTitle = computed(() => {
  const sections = {
    1: 'Personal Information',
    2: 'Account Preferences', 
    3: 'Subscription Details',
    4: 'Review and Confirm'
  }
  return sections[currentStep.value] || 'Unknown Section'
})
</script>
```

### Form Section Subtitles
```vue
<template>
  <form class="registration-form">
    <codex-title :content="'Create Account'" :tag="'h1'" />
    
    <fieldset>
      <codex-subtitle 
        :content="'Basic Information'" 
        :tag="'h2'"
        :class-name="'form-section-title'"
      />
      
      <div class="form-group">
        <codex-label :label="'Full Name'" />
        <codex-input v-model="fullName" />
      </div>
      
      <div class="form-group">
        <codex-label :label="'Email Address'" />
        <codex-input v-model="email" />
      </div>
    </fieldset>
    
    <fieldset>
      <codex-subtitle 
        :content="'Security Settings'" 
        :tag="'h2'"
        :class-name="'form-section-title'"
      />
      
      <div class="form-group">
        <codex-label :label="'Password'" />
        <codex-input v-model="password" type="password" />
      </div>
    </fieldset>
  </form>
</template>
```

### Card Component Subtitles
```vue
<template>
  <div class="product-cards">
    <div v-for="product in products" :key="product.id" class="product-card">
      <codex-subtitle 
        :content="product.name" 
        :tag="'h3'"
        :class-name="'card-title'"
      />
      
      <codex-subtitle 
        :content="product.category" 
        :tag="'h4'"
        :class-name="'card-category'"
      />
      
      <codex-paragraph :content="product.description" />
    </div>
  </div>
</template>

<script setup>
const products = ref([
  {
    id: 1,
    name: 'Premium Package',
    category: 'Subscription Plans',
    description: 'Full access to all features...'
  },
  {
    id: 2,
    name: 'Basic Package', 
    category: 'Subscription Plans',
    description: 'Essential features for getting started...'
  }
])
</script>
```

### Navigation Section Subtitles
```vue
<template>
  <nav class="main-navigation">
    <div class="nav-section">
      <codex-subtitle 
        :content="'Account'" 
        :tag="'h3'"
        :class-name="'nav-section-title'"
      />
      <ul class="nav-links">
        <li><a href="/profile">Profile</a></li>
        <li><a href="/settings">Settings</a></li>
      </ul>
    </div>
    
    <div class="nav-section">
      <codex-subtitle 
        :content="'Services'" 
        :tag="'h3'"
        :class-name="'nav-section-title'"
      />
      <ul class="nav-links">
        <li><a href="/bookings">Bookings</a></li>
        <li><a href="/subscriptions">Subscriptions</a></li>
      </ul>
    </div>
  </nav>
</template>
```

### Responsive Subtitle Hierarchy
```vue
<template>
  <article class="responsive-content">
    <codex-subtitle 
      :content="'Mobile-First Design'" 
      :tag="responsiveTag"
      :class-name="responsiveClass"
    />
  </article>
</template>

<script setup>
import { computed } from 'vue'

const screenSize = ref('desktop') // Would be set by media query watcher

const responsiveTag = computed(() => {
  switch (screenSize.value) {
    case 'mobile': return 'h3'
    case 'tablet': return 'h2'
    case 'desktop': return 'h2'
    default: return 'h2'
  }
})

const responsiveClass = computed(() => {
  return `subtitle-${screenSize.value}`
})
</script>
```

### Non-Semantic Styled Subtitle
```vue
<template>
  <div class="decorative-section">
    <codex-subtitle 
      :content="'Featured Content'" 
      :tag="'div'"
      :class-name="'decorative-subtitle accent-text'"
    />
    
    <div class="featured-items">
      <!-- Featured content -->
    </div>
  </div>
</template>
```

## CSS Classes
- `_c-subtitle`: Base subtitle styling applied to all instances

## Best Practices

### Recommended Usage Patterns
- Use appropriate heading tags for semantic hierarchy
- Provide meaningful and descriptive subtitle content
- Apply consistent CSS classes for styling
- Maintain proper heading order (h1 → h2 → h3, etc.)
- Use non-semantic tags (div/span) only for decorative purposes
- Keep subtitle content concise and descriptive
- Use className prop for custom styling needs

### Common Pitfalls to Avoid
- Skipping heading levels (h1 → h3 without h2)
- Using heading tags purely for visual styling
- Not providing subtitle content
- Missing CSS class customization when needed
- Using overly long subtitle text
- Not considering responsive design needs
- Missing semantic structure in content hierarchy

### Accessibility Considerations
- Maintain logical heading hierarchy for screen readers
- Use appropriate heading tags for document outline
- Provide meaningful subtitle text
- Ensure sufficient color contrast for subtitle text
- Don't rely solely on visual styling to convey importance
- Use semantic HTML structure when possible
- Consider ARIA labeling for complex subtitle relationships

### Content Guidelines
- Keep subtitles concise and descriptive
- Use consistent capitalization (title case or sentence case)
- Avoid redundant information in subtitles
- Make subtitles scannable for quick reading
- Use parallel structure for related subtitles
- Consider internationalization needs for subtitle length

### Semantic Structure
- Use h1 for main page title
- Use h2 for major section subtitles  
- Use h3-h6 for nested section subtitles
- Don't skip heading levels in hierarchy
- Use div/span only for non-semantic decorative subtitles
- Maintain consistent heading structure across pages

### Performance Considerations
- Minimize dynamic content changes in subtitles
- Use computed properties for reactive subtitle content
- Cache subtitle content when possible
- Avoid complex logic in subtitle generation
- Handle large amounts of dynamic subtitles efficiently

### Styling Integration
- Use className prop for component-specific styles
- Coordinate with design system typography
- Maintain consistent subtitle styling patterns
- Consider responsive typography needs
- Use CSS custom properties for theming
- Handle dark mode and theme variations

### Form Integration
- Use subtitles to organize form sections
- Provide clear section boundaries with subtitles
- Use appropriate heading levels for form hierarchy
- Consider accessibility in form subtitle structure
- Group related form fields under subtitles

### Content Management
- Centralize subtitle content for easy updates
- Use consistent subtitle formatting across application
- Handle dynamic subtitle content appropriately
- Consider content loading states for subtitles
- Plan for internationalization of subtitle content

## Component Registration
The component is registered as `codex-subtitle` in the application. 