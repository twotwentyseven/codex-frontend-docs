# Title Component

## Overview
The Title component provides a flexible atomic element for rendering customizable headings and titles with dynamic HTML tag support, CSS class application, and layout configuration. It features content rendering with v-html, configurable element tags, and responsive layout options for versatile title display throughout the application.

## Basic Usage
```vue
<codex-title
  :content="'Welcome to Our Application'"
  :tag="'h1'"
/>
```

## Key Features
- Dynamic HTML tag rendering (h1, h2, h3, div, etc.)
- Custom content with HTML support via v-html
- Configurable CSS class application
- Responsive layout system with predefined options
- Flexible content injection and styling
- Component-based title management
- Layout coordination for form field integration

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| tag | String | No | 'div' | HTML tag to render (h1, h2, h3, div, etc.) |
| content | String | No | '' | HTML content to display in the title |
| className | String | No | '_c-title' | CSS class to apply to the title element |
| layout | String | No | 'auto' | Layout configuration for responsive display |

## Layout Options
The component supports various layout configurations:
- **auto**: Default responsive layout behavior
- **1**: Full width layout (`_c-form-field--full`)
- **2**: Half width layout (`_c-form-field--half`)
- **3**: Third width layout (`_c-form-field--third`)
- **4**: Quarter width layout (`_c-form-field--quarter`)

## Dynamic Tag Rendering
The component uses Vue's `<component>` element for dynamic tag rendering:
- **Tag Processing**: Converts tag prop to lowercase for consistency
- **HTML Elements**: Supports any valid HTML tag (h1, h2, h3, h4, h5, h6, div, span, p, etc.)
- **Semantic Structure**: Enables proper semantic HTML for accessibility
- **Flexibility**: Allows runtime tag determination

## Content Rendering
The component uses `v-html` for content display:
- **HTML Support**: Renders HTML content directly
- **Dynamic Content**: Supports computed and reactive content
- **Security**: Ensure content is trusted to prevent XSS attacks
- **Formatting**: Preserves HTML formatting and structure

## CSS Class System
The component applies multiple CSS classes:
- **Base Class**: Uses className prop (default: `_c-title`)
- **Layout Classes**: Adds layout-specific classes based on props
- **Form Integration**: Uses form field classes for layout coordination
- **Responsive**: Layout classes provide responsive behavior

## Layout Integration
The component integrates with form field layout system:
- **Form Field Classes**: Uses `_c-form-field--*` classes for consistency
- **Grid Coordination**: Layout options coordinate with CSS grid systems
- **Responsive Design**: Layout classes adapt to different screen sizes
- **Alignment**: Coordinates with other form components

## HTML Security
When using v-html with dynamic content:
- **Trusted Content**: Only use with trusted, sanitized content
- **XSS Prevention**: Sanitize user-generated content before passing
- **Content Validation**: Validate HTML content structure
- **Security Audit**: Regular security review for v-html usage

## Internationalization
The Title component does not include built-in internationalization. Content should be localized before passing to the component.

## Examples

### Basic Title with H1 Tag
```vue
<codex-title
  :content="'Application Dashboard'"
  :tag="'h1'"
/>
```

### Page Section Title
```vue
<codex-title
  :content="'User Profile Settings'"
  :tag="'h2'"
  :className="'section-title'"
/>
```

### Form Section Title with Layout
```vue
<template>
  <div class="form-section">
    <codex-title
      :content="'Personal Information'"
      :tag="'h3'"
      :layout="'1'"
    />
    
    <div class="form-fields">
      <!-- Form fields -->
    </div>
  </div>
</template>
```

### Dynamic Title Content
```vue
<template>
  <codex-title
    :content="dynamicTitle"
    :tag="titleTag"
    :layout="titleLayout"
  />
</template>

<script setup>
import { ref, computed } from 'vue'

const userName = ref('John Doe')
const titleLevel = ref(2)

const dynamicTitle = computed(() => `Welcome back, ${userName.value}!`)
const titleTag = computed(() => `h${titleLevel.value}`)
const titleLayout = computed(() => 'auto')
</script>
```

### Title with HTML Content
```vue
<template>
  <codex-title
    :content="formattedTitle"
    :tag="'h2'"
    :className="'styled-title'"
  />
</template>

<script setup>
const formattedTitle = ref(`
  <span class="title-prefix">Chapter 1:</span>
  <strong>Getting Started</strong>
`)
</script>
```

### Responsive Title Layout
```vue
<template>
  <div class="title-grid">
    <codex-title
      :content="'Main Title'"
      :tag="'h1'"
      :layout="'1'"
    />
    
    <codex-title
      :content="'Sub Title Left'"
      :tag="'h3'"
      :layout="'2'"
    />
    
    <codex-title
      :content="'Sub Title Right'"
      :tag="'h3'"
      :layout="'2'"
    />
  </div>
</template>
```

### Conditional Title Rendering
```vue
<template>
  <div class="conditional-titles">
    <codex-title
      v-if="showMainTitle"
      :content="mainTitleContent"
      :tag="'h1'"
    />
    
    <codex-title
      v-if="showSubTitle"
      :content="subTitleContent"
      :tag="'h2'"
      :layout="subTitleLayout"
    />
  </div>
</template>

<script setup>
const showMainTitle = ref(true)
const showSubTitle = ref(false)
const mainTitleContent = ref('Dashboard Overview')
const subTitleContent = ref('Recent Activity')
const subTitleLayout = ref('auto')

const toggleSubTitle = () => {
  showSubTitle.value = !showSubTitle.value
}
</script>
```

### Multi-level Title Structure
```vue
<template>
  <article class="content-structure">
    <codex-title
      :content="'Documentation Guide'"
      :tag="'h1'"
      :layout="'1'"
    />
    
    <section class="section">
      <codex-title
        :content="'Getting Started'"
        :tag="'h2'"
        :layout="'1'"
      />
      
      <div class="subsection">
        <codex-title
          :content="'Installation'"
          :tag="'h3'"
          :layout="'2'"
        />
        
        <codex-title
          :content="'Configuration'"
          :tag="'h3'"
          :layout="'2'"
        />
      </div>
    </section>
  </article>
</template>
```

### Title with Custom Styling
```vue
<template>
  <codex-title
    :content="styledTitle"
    :tag="'h2'"
    :className="'custom-title-style'"
  />
</template>

<script setup>
const styledTitle = ref(`
  <i class="ri-star-fill"></i>
  Featured Content
  <i class="ri-star-fill"></i>
`)
</script>

<style scoped>
.custom-title-style {
  color: #3b82f6;
  text-align: center;
  font-weight: 700;
}

.custom-title-style i {
  color: #fbbf24;
  margin: 0 0.5rem;
}
</style>
```

### Form Field Title Integration
```vue
<template>
  <div class="form-with-titles">
    <codex-title
      :content="'Account Information'"
      :tag="'h2'"
      :layout="'1'"
    />
    
    <div class="form-row">
      <div class="form-group">
        <codex-title
          :content="'Personal Details'"
          :tag="'h4'"
          :layout="'2'"
        />
        <!-- Personal form fields -->
      </div>
      
      <div class="form-group">
        <codex-title
          :content="'Contact Information'"
          :tag="'h4'"
          :layout="'2'"
        />
        <!-- Contact form fields -->
      </div>
    </div>
  </div>
</template>
```

### Dynamic Title Hierarchy
```vue
<template>
  <div class="dynamic-hierarchy">
    <codex-title
      v-for="(title, index) in titleHierarchy"
      :key="title.id"
      :content="title.content"
      :tag="title.tag"
      :layout="title.layout"
      :className="title.className"
    />
  </div>
</template>

<script setup>
const titleHierarchy = ref([
  {
    id: 1,
    content: 'Main Documentation',
    tag: 'h1',
    layout: '1',
    className: '_c-title main-title'
  },
  {
    id: 2,
    content: 'Component Overview',
    tag: 'h2',
    layout: '1',
    className: '_c-title section-title'
  },
  {
    id: 3,
    content: 'Basic Usage',
    tag: 'h3',
    layout: '2',
    className: '_c-title subsection-title'
  },
  {
    id: 4,
    content: 'Advanced Features',
    tag: 'h3',
    layout: '2',
    className: '_c-title subsection-title'
  }
])
</script>
```

### Localized Title Content
```vue
<template>
  <codex-title
    :content="localizedTitle"
    :tag="'h2'"
    :layout="'1'"
  />
</template>

<script setup>
import { computed } from 'vue'

const currentLanguage = ref('en')
const titleKey = ref('dashboard.welcome')

const translations = {
  en: {
    'dashboard.welcome': 'Welcome to Dashboard'
  },
  es: {
    'dashboard.welcome': 'Bienvenido al Panel'
  },
  fr: {
    'dashboard.welcome': 'Bienvenue au Tableau de Bord'
  }
}

const localizedTitle = computed(() => {
  const lang = translations[currentLanguage.value] || translations.en
  return lang[titleKey.value] || titleKey.value
})
</script>
```

## CSS Classes
- `_c-title`: Default title styling (customizable via className prop)
- `_c-form-field--auto`: Auto layout (default)
- `_c-form-field--full`: Full width layout
- `_c-form-field--half`: Half width layout
- `_c-form-field--third`: Third width layout
- `_c-form-field--quarter`: Quarter width layout

## Best Practices

### Recommended Usage Patterns
- Use semantic HTML tags (h1, h2, h3, etc.) for proper document structure
- Apply appropriate heading hierarchy for accessibility
- Use layout options for responsive design coordination
- Sanitize HTML content when using dynamic content
- Provide meaningful and descriptive title content
- Use consistent className patterns across the application
- Coordinate with form field layouts when appropriate

### Common Pitfalls to Avoid
- Using v-html with unsanitized user content (XSS vulnerability)
- Breaking heading hierarchy (h1 → h3 without h2)
- Overusing HTML content when plain text is sufficient
- Not providing alt text or context for complex HTML content
- Using inappropriate HTML tags for semantic meaning
- Missing layout coordination in form contexts

### Accessibility Considerations
- Maintain proper heading hierarchy for screen readers
- Use semantic HTML tags appropriate to content structure
- Ensure sufficient color contrast for title text
- Provide meaningful title content for navigation
- Handle focus management when titles are interactive
- Use ARIA attributes when necessary for complex structures
- Support keyboard navigation for interactive titles

### HTML Tag Selection
- **h1**: Main page or section title (use sparingly, once per page)
- **h2**: Major section headings
- **h3**: Subsection headings
- **h4-h6**: Minor headings as needed
- **div**: Non-semantic titles or decorative content
- **span**: Inline title content
- **p**: Paragraph-style titles

### Content Management
- Sanitize HTML content from external sources
- Use trusted content sources for v-html rendering
- Validate HTML structure before rendering
- Handle empty or undefined content gracefully
- Consider content length for layout coordination
- Provide fallback content for missing translations

### Layout Coordination
- Use layout props to coordinate with CSS grid systems
- Match layout options with surrounding form elements
- Consider responsive breakpoints for layout changes
- Test layout coordination across different screen sizes
- Handle layout changes based on content length

### Performance Considerations
- Minimize complex HTML content processing
- Use computed properties for dynamic content generation
- Cache processed content when appropriate
- Optimize re-renders for frequently changing titles
- Handle large content efficiently

### Styling Integration
- Use consistent className patterns across components
- Coordinate with design system typography
- Handle responsive typography scaling
- Provide theme support through CSS classes
- Test styling across different content types

### Security Considerations
- Always sanitize HTML content from user input
- Use Content Security Policy (CSP) headers
- Validate HTML structure and content
- Avoid inline JavaScript in content
- Regular security audits for v-html usage
- Use trusted content sources only

### Form Integration
- Coordinate title layouts with form field layouts
- Use semantic headings for form section organization
- Handle form field grouping with appropriate titles
- Support form accessibility with proper heading structure
- Integrate with form validation and error display

## Component Registration
The component is registered as `codex-title` in the application. 