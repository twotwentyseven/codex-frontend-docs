# Paragraph Component

## Overview
The Paragraph component provides a flexible atomic element for rendering text content with dynamic HTML tag support, layout configuration, and HTML content rendering. It features v-html content display, configurable CSS classes, responsive layout options, and semantic tag selection for versatile text presentation throughout the application.

## Basic Usage
```vue
<codex-paragraph
  :content="'This is a paragraph of text content.'"
  :tag="'p'"
/>
```

## Key Features
- Dynamic HTML tag rendering (p, div, span, etc.)
- HTML content support via v-html rendering
- Responsive layout system with predefined options
- Custom CSS class configuration with default styling
- Configurable semantic tag selection
- Form field layout integration
- Flexible content injection and formatting

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| tag | String | No | 'p' | HTML tag to render (p, div, span, etc.) |
| content | String | No | '' | HTML content to display in the paragraph |
| className | String | No | '' | Additional CSS class to apply |
| layout | String | No | 'auto' | Layout configuration for responsive display |

## Layout Options
The component supports various layout configurations:
- **auto**: Default responsive layout behavior
- **quarter**: Quarter width layout (`_c-form-field--quarter`)
- **third**: Third width layout (`_c-form-field--third`)
- **half**: Half width layout (`_c-form-field--half`)
- **full**: Full width layout (`_c-form-field--full`)

## Dynamic Tag Rendering
The component uses Vue's `<component>` element for dynamic tag rendering:
- **Tag Processing**: Converts tag prop to lowercase for consistency
- **HTML Elements**: Supports any valid HTML tag (p, div, span, section, etc.)
- **Semantic Structure**: Enables proper semantic HTML for accessibility
- **Flexibility**: Allows runtime tag determination

## Content Rendering
The component uses `v-html` for content display:
- **HTML Support**: Renders HTML content directly
- **Text Content**: Supports plain text content
- **Dynamic Content**: Supports computed and reactive content
- **Security**: Ensure content is trusted to prevent XSS attacks

## CSS Class System
The component applies multiple CSS classes:
- **Base Class**: `_c-desc` applied to all paragraph elements
- **Custom Class**: Additional className applied alongside base class
- **Layout Classes**: Form field layout classes based on layout prop
- **Responsive**: Layout classes provide responsive behavior

## Layout Integration
The component integrates with form field layout system:
- **Form Field Classes**: Uses `_c-form-field--*` classes for consistency
- **Grid Coordination**: Layout options coordinate with CSS grid systems
- **Responsive Design**: Layout classes adapt to different screen sizes
- **Text Alignment**: Coordinates with other text and form components

## HTML Security
When using v-html with dynamic content:
- **Trusted Content**: Only use with trusted, sanitized content
- **XSS Prevention**: Sanitize user-generated content before passing
- **Content Validation**: Validate HTML content structure
- **Security Audit**: Regular security review for v-html usage

## Internationalization
The Paragraph component does not include built-in internationalization. Content should be localized before passing to the component.

## Examples

### Basic Paragraph
```vue
<codex-paragraph
  :content="'This is a simple paragraph of text content.'"
/>
```

### Paragraph with Custom Tag
```vue
<codex-paragraph
  :content="'This content uses a div tag instead of p.'"
  :tag="'div'"
/>
```

### Paragraph with Layout
```vue
<codex-paragraph
  :content="'This paragraph takes up half the available width.'"
  :layout="'half'"
/>
```

### Paragraph with HTML Content
```vue
<template>
  <codex-paragraph
    :content="formattedContent"
    :tag="'div'"
  />
</template>

<script setup>
const formattedContent = ref(`
  This paragraph contains <strong>bold text</strong> and 
  <em>italic text</em> with <a href="/link">a link</a>.
`)
</script>
```

### Form Description Paragraph
```vue
<template>
  <div class="form-section">
    <codex-paragraph
      :content="'Please fill out all required fields below.'"
      :layout="'full'"
      :class-name="'form-description'"
    />
    
    <!-- Form fields -->
  </div>
</template>
```

### Dynamic Content Paragraph
```vue
<template>
  <codex-paragraph
    :content="dynamicContent"
    :tag="contentTag"
    :layout="contentLayout"
  />
</template>

<script setup>
const userName = ref('John Doe')
const userLevel = ref('premium')

const dynamicContent = computed(() => {
  return `Welcome back, <strong>${userName.value}</strong>! 
          You are currently on the ${userLevel.value} plan.`
})

const contentTag = computed(() => 
  userLevel.value === 'premium' ? 'div' : 'p'
)

const contentLayout = computed(() => 
  userLevel.value === 'premium' ? 'full' : 'auto'
)
</script>
```

### Multi-layout Paragraph Grid
```vue
<template>
  <div class="paragraph-grid">
    <codex-paragraph
      :content="'Main description content that spans the full width.'"
      :layout="'full'"
      :class-name="'main-description'"
    />
    
    <codex-paragraph
      :content="'Left column content with additional details.'"
      :layout="'half'"
      :class-name="'left-description'"
    />
    
    <codex-paragraph
      :content="'Right column content with supplementary information.'"
      :layout="'half'"
      :class-name="'right-description'"
    />
  </div>
</template>
```

### Conditional Paragraph Rendering
```vue
<template>
  <div class="conditional-content">
    <codex-paragraph
      v-if="showIntro"
      :content="introContent"
      :layout="'full'"
    />
    
    <codex-paragraph
      v-if="showDetails"
      :content="detailsContent"
      :layout="'auto'"
      :class-name="'details-text'"
    />
    
    <codex-paragraph
      v-if="hasWarning"
      :content="warningContent"
      :class-name="'warning-text'"
    />
  </div>
</template>

<script setup>
const showIntro = ref(true)
const showDetails = ref(false)
const hasWarning = ref(false)

const introContent = ref('Welcome to our application!')
const detailsContent = ref('Here are the detailed instructions...')
const warningContent = ref('⚠️ Please note: This action cannot be undone.')
</script>
```

### Rich Text Paragraph
```vue
<template>
  <codex-paragraph
    :content="richTextContent"
    :tag="'div'"
    :class-name="'rich-content'"
  />
</template>

<script setup>
const richTextContent = ref(`
  <h4>Important Notice</h4>
  <p>This is a <strong>rich text paragraph</strong> that contains:</p>
  <ul>
    <li>Multiple HTML elements</li>
    <li><em>Formatted text</em></li>
    <li><a href="/help">Help links</a></li>
  </ul>
  <p><small>Last updated: ${new Date().toLocaleDateString()}</small></p>
`)
</script>
```

### Article Content Paragraph
```vue
<template>
  <article class="article-content">
    <codex-paragraph
      :content="articleTitle"
      :tag="'h2'"
      :layout="'full'"
      :class-name="'article-title'"
    />
    
    <codex-paragraph
      :content="articleSummary"
      :layout="'full'"
      :class-name="'article-summary'"
    />
    
    <codex-paragraph
      :content="articleBody"
      :layout="'auto'"
      :class-name="'article-body'"
    />
  </article>
</template>

<script setup>
const articleTitle = ref('Understanding Vue Components')
const articleSummary = ref('A comprehensive guide to building reusable Vue.js components.')
const articleBody = ref(`
  Vue components are the building blocks of Vue applications. 
  They allow you to create reusable pieces of UI with their own 
  logic and styling. <strong>This article covers everything you need to know.</strong>
`)
</script>
```

### Error Message Paragraph
```vue
<template>
  <div class="error-container">
    <codex-paragraph
      v-if="hasError"
      :content="errorMessage"
      :class-name="'error-message'"
      :tag="'div'"
    />
  </div>
</template>

<script setup>
const hasError = ref(false)
const errorMessage = ref('')

const showError = (message) => {
  errorMessage.value = `<i class="ri-error-warning-line"></i> ${message}`
  hasError.value = true
}

// Example usage
// showError('Please check your input and try again.')
</script>
```

### Status Description Paragraph
```vue
<template>
  <div class="status-display">
    <codex-paragraph
      :content="statusDescription"
      :class-name="statusClass"
      :layout="'full'"
    />
  </div>
</template>

<script setup>
const status = ref('success')
const message = ref('Operation completed successfully!')

const statusDescription = computed(() => {
  const icons = {
    success: 'ri-check-circle-fill',
    warning: 'ri-alert-fill',
    error: 'ri-error-warning-fill',
    info: 'ri-information-fill'
  }
  
  return `<i class="${icons[status.value]}"></i> ${message.value}`
})

const statusClass = computed(() => `status-${status.value}`)
</script>
```

### Localized Content Paragraph
```vue
<template>
  <codex-paragraph
    :content="localizedContent"
    :layout="'auto'"
    :class-name="'localized-text'"
  />
</template>

<script setup>
import { computed } from 'vue'

const currentLanguage = ref('en')
const contentKey = ref('welcome.description')

const translations = {
  en: {
    'welcome.description': 'Welcome to our platform! Get started by exploring the features.'
  },
  es: {
    'welcome.description': '¡Bienvenido a nuestra plataforma! Comienza explorando las características.'
  },
  fr: {
    'welcome.description': 'Bienvenue sur notre plateforme ! Commencez par explorer les fonctionnalités.'
  }
}

const localizedContent = computed(() => {
  const lang = translations[currentLanguage.value] || translations.en
  return lang[contentKey.value] || contentKey.value
})
</script>
```

### Interactive Content Paragraph
```vue
<template>
  <codex-paragraph
    :content="interactiveContent"
    :tag="'div'"
    :class-name="'interactive-content'"
    @click="handleContentClick"
  />
</template>

<script setup>
const clickCount = ref(0)

const interactiveContent = computed(() => `
  <p>This is interactive content. Clicked: <strong>${clickCount.value}</strong> times.</p>
  <button onclick="this.closest('.interactive-content').__vueParentComponent.ctx.incrementClick()">
    Click me!
  </button>
`)

const handleContentClick = (event) => {
  // Handle clicks on interactive elements
  if (event.target.tagName === 'BUTTON') {
    incrementClick()
  }
}

const incrementClick = () => {
  clickCount.value++
}
</script>
```

### Responsive Typography Paragraph
```vue
<template>
  <div class="responsive-content">
    <codex-paragraph
      :content="responsiveContent"
      :layout="responsiveLayout"
      :class-name="responsiveClass"
    />
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const windowWidth = ref(window.innerWidth)

const responsiveLayout = computed(() => {
  if (windowWidth.value < 768) return 'full'
  if (windowWidth.value < 1024) return 'half'
  return 'third'
})

const responsiveClass = computed(() => {
  if (windowWidth.value < 768) return 'mobile-text'
  if (windowWidth.value < 1024) return 'tablet-text'
  return 'desktop-text'
})

const responsiveContent = computed(() => {
  const size = windowWidth.value < 768 ? 'small' : 
               windowWidth.value < 1024 ? 'medium' : 'large'
  return `This content adapts to your screen size (${size}).`
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

## CSS Classes
- `_c-desc`: Base paragraph styling applied to all instances
- `_c-form-field--auto`: Auto layout (default)
- `_c-form-field--quarter`: Quarter width layout
- `_c-form-field--third`: Third width layout
- `_c-form-field--half`: Half width layout
- `_c-form-field--full`: Full width layout
- Custom classes via className prop

## Best Practices

### Recommended Usage Patterns
- Use semantic HTML tags (p, div, span) appropriate to content type
- Apply layout options for responsive text presentation
- Sanitize HTML content when using dynamic content
- Use consistent className patterns for styling coordination
- Provide meaningful content that enhances user understanding
- Coordinate with form field layouts when appropriate
- Test text rendering across different screen sizes

### Common Pitfalls to Avoid
- Using v-html with unsanitized user content (XSS vulnerability)
- Overusing HTML content when plain text is sufficient
- Not providing accessible text content for screen readers
- Using inappropriate HTML tags for content semantics
- Missing layout coordination in responsive designs
- Not handling empty or undefined content gracefully

### Accessibility Considerations
- Use semantic HTML tags appropriate to content structure
- Ensure sufficient color contrast for text content
- Provide meaningful text content for screen readers
- Handle text scaling and zoom appropriately
- Use proper heading hierarchy when using heading tags
- Support keyboard navigation for interactive content
- Test with screen readers for proper content announcement

### Content Guidelines
- Write clear and concise text content
- Use plain language that's appropriate for your audience
- Structure content logically with proper HTML elements
- Provide helpful context without being redundant
- Use consistent tone and style across all text content
- Consider text length for different layout options

### HTML Tag Selection
- **p**: Standard paragraph text content
- **div**: Container for mixed content or when p semantics aren't appropriate
- **span**: Inline text content or small text snippets
- **section**: Semantic sections of content
- **article**: Self-contained article content
- **aside**: Supplementary content

### Performance Considerations
- Minimize complex HTML content processing
- Use computed properties for dynamic content generation
- Cache processed content when appropriate
- Optimize re-renders for frequently changing content
- Handle large text content efficiently

### Layout Coordination
- Use layout props to coordinate with CSS grid systems
- Match layout options with surrounding elements
- Consider responsive breakpoints for layout changes
- Test layout behavior across different screen sizes
- Handle text overflow in constrained layouts

### Security Considerations
- Always sanitize HTML content from user input
- Use Content Security Policy (CSP) headers
- Validate HTML structure and content
- Avoid inline JavaScript in content
- Regular security audits for v-html usage
- Use trusted content sources only

### Styling Integration
- Use consistent className patterns across components
- Coordinate with design system typography
- Handle responsive typography scaling
- Provide theme support through CSS classes
- Test styling across different content types

### State Management
- Use reactive properties for dynamic content
- Handle content updates efficiently
- Coordinate text state with parent components
- Manage content lifecycle properly
- Handle content loading and error states

### Form Integration
- Coordinate paragraph layouts with form field layouts
- Use paragraphs for form descriptions and help text
- Handle form accessibility with proper text structure
- Integrate with form validation and error display
- Support form field grouping with descriptive text

## Component Registration
The component is registered as `codex-paragraph` in the application. 