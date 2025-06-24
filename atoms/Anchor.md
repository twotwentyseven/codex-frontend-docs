# Anchor Component

## Overview
The Anchor component provides a flexible atomic link element with slot-based content customization, internationalization support, and target configuration. It features before/after slots for icons or decorative elements, custom CSS class application, and Vue i18n integration for comprehensive link management throughout the application.

## Basic Usage
```vue
<codex-anchor
  :href="'/dashboard'"
  :default-text="'Go to Dashboard'"
/>
```

## Key Features
- Flexible href configuration for internal and external links
- Target attribute support (_blank, _self, etc.)
- Before and after content slots for icons and decorative elements
- Custom CSS class application with default styling
- Vue i18n internationalization integration
- Default text with fallback behavior
- Event emission capability (though not currently implemented)
- Slot-based content customization

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| href | String | Yes | - | Link destination URL or path |
| target | String | No | undefined | Link target (_blank, _self, _parent, _top) |
| defaultText | String\|Boolean | No | false | Default link text when no slot content provided |
| anchorClass | String | No | '' | Custom CSS class to override default '_c-link' |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| submitted | - | Currently defined but not implemented in component |

## Slots
| Slot Name | Description |
|-----------|-------------|
| before | Content displayed before the link text (typically icons) |
| after | Content displayed after the link text (typically icons) |
| default | Main link content (overrides defaultText when provided) |

## Link Target Options
The component supports standard HTML anchor target values:
- **_blank**: Opens link in new tab/window
- **_self**: Opens link in same frame (default browser behavior)
- **_parent**: Opens link in parent frame
- **_top**: Opens link in full window
- **undefined**: Uses default browser behavior

## CSS Class Application
The component applies CSS classes as follows:
- **Default Class**: `_c-link` when anchorClass is empty
- **Custom Class**: Uses anchorClass prop value when provided
- **Override Behavior**: anchorClass completely replaces default class

## Content Priority
The component handles content display with the following priority:
1. **Slot Content**: Default slot content takes highest priority
2. **Default Text**: defaultText prop used when no slot content
3. **Before/After Slots**: Always displayed regardless of main content

## Internationalization
The component integrates with Vue i18n:
- **Translation Access**: Uses `useI18n()` composable for translation functions
- **Text Localization**: Supports localized link text through i18n
- **Dynamic Content**: Can integrate with reactive translation content

## Accessibility Features
- **Semantic HTML**: Uses proper anchor element structure
- **Target Indication**: External links with target="_blank" should include indication
- **Keyboard Navigation**: Standard anchor keyboard support (Enter key)
- **Screen Reader**: Compatible with assistive technologies
- **Focus Management**: Browser-native focus handling

## External Link Handling
When linking to external sites:
- Use `target="_blank"` for external links
- Consider adding `rel="noopener noreferrer"` for security
- Provide visual indication of external links
- Test cross-origin navigation requirements

## Internationalization
The Anchor component includes Vue i18n integration. Text content can be localized using the translation functions, though specific implementation depends on your i18n setup.

## Examples

### Basic Internal Link
```vue
<codex-anchor
  :href="'/profile'"
  :default-text="'View Profile'"
/>
```

### External Link with Target
```vue
<codex-anchor
  :href="'https://example.com'"
  :target="'_blank'"
  :default-text="'Visit External Site'"
/>
```

### Link with Before Icon
```vue
<codex-anchor
  :href="'/settings'"
  :default-text="'Settings'"
>
  <template #before>
    <i class="ri-settings-line"></i>
  </template>
</codex-anchor>
```

### Link with After Icon
```vue
<codex-anchor
  :href="'https://docs.example.com'"
  :target="'_blank'"
  :default-text="'Documentation'"
>
  <template #after>
    <i class="ri-external-link-line"></i>
  </template>
</codex-anchor>
```

### Link with Custom Styling
```vue
<codex-anchor
  :href="'/premium'"
  :default-text="'Upgrade to Premium'"
  :anchor-class="'premium-link'"
/>
```

### Navigation Menu Link
```vue
<template>
  <nav class="main-navigation">
    <codex-anchor
      :href="'/dashboard'"
      :default-text="'Dashboard'"
      :anchor-class="'nav-link'"
    >
      <template #before>
        <i class="ri-dashboard-line"></i>
      </template>
    </codex-anchor>
    
    <codex-anchor
      :href="'/projects'"
      :default-text="'Projects'"
      :anchor-class="'nav-link'"
    >
      <template #before>
        <i class="ri-folder-line"></i>
      </template>
    </codex-anchor>
    
    <codex-anchor
      :href="'/settings'"
      :default-text="'Settings'"
      :anchor-class="'nav-link'"
    >
      <template #before>
        <i class="ri-settings-line"></i>
      </template>
    </codex-anchor>
  </nav>
</template>
```

### Link with Complex Content
```vue
<codex-anchor
  :href="'/article/123'"
  :anchor-class="'article-link'"
>
  <template #default>
    <div class="article-preview">
      <h3>Article Title</h3>
      <p>Article summary text goes here...</p>
      <span class="read-more">Read more</span>
    </div>
  </template>
</codex-anchor>
```

### Social Media Links
```vue
<template>
  <div class="social-links">
    <codex-anchor
      :href="'https://twitter.com/example'"
      :target="'_blank'"
      :anchor-class="'social-link twitter'"
    >
      <template #before>
        <i class="ri-twitter-fill"></i>
      </template>
      <template #default>
        Twitter
      </template>
    </codex-anchor>
    
    <codex-anchor
      :href="'https://github.com/example'"
      :target="'_blank'"
      :anchor-class="'social-link github'"
    >
      <template #before>
        <i class="ri-github-fill"></i>
      </template>
      <template #default>
        GitHub
      </template>
    </codex-anchor>
    
    <codex-anchor
      :href="'https://linkedin.com/in/example'"
      :target="'_blank'"
      :anchor-class="'social-link linkedin'"
    >
      <template #before>
        <i class="ri-linkedin-fill"></i>
      </template>
      <template #default>
        LinkedIn
      </template>
    </codex-anchor>
  </div>
</template>
```

### Breadcrumb Navigation
```vue
<template>
  <nav class="breadcrumb">
    <codex-anchor
      :href="'/'"
      :default-text="'Home'"
      :anchor-class="'breadcrumb-link'"
    />
    
    <span class="breadcrumb-separator">/</span>
    
    <codex-anchor
      :href="'/category'"
      :default-text="'Category'"
      :anchor-class="'breadcrumb-link'"
    />
    
    <span class="breadcrumb-separator">/</span>
    
    <span class="breadcrumb-current">Current Page</span>
  </nav>
</template>
```

### Download Links
```vue
<template>
  <div class="download-section">
    <codex-anchor
      :href="'/downloads/manual.pdf'"
      :target="'_blank'"
      :default-text="'Download Manual'"
      :anchor-class="'download-link'"
    >
      <template #before>
        <i class="ri-download-line"></i>
      </template>
      <template #after>
        <span class="file-type">PDF</span>
      </template>
    </codex-anchor>
    
    <codex-anchor
      :href="'/downloads/app.zip'"
      :default-text="'Download App'"
      :anchor-class="'download-link'"
    >
      <template #before>
        <i class="ri-download-line"></i>
      </template>
      <template #after>
        <span class="file-size">2.4 MB</span>
      </template>
    </codex-anchor>
  </div>
</template>
```

### Dynamic Link Content
```vue
<template>
  <codex-anchor
    :href="dynamicHref"
    :target="linkTarget"
    :anchor-class="linkClass"
  >
    <template #before>
      <i :class="linkIcon"></i>
    </template>
    <template #default>
      {{ linkText }}
    </template>
  </codex-anchor>
</template>

<script setup>
const props = defineProps(['linkType', 'destination', 'text'])

const dynamicHref = computed(() => {
  switch (props.linkType) {
    case 'internal':
      return props.destination
    case 'external':
      return `https://${props.destination}`
    case 'email':
      return `mailto:${props.destination}`
    case 'phone':
      return `tel:${props.destination}`
    default:
      return props.destination
  }
})

const linkTarget = computed(() => 
  props.linkType === 'external' ? '_blank' : undefined
)

const linkClass = computed(() => `link-${props.linkType}`)

const linkIcon = computed(() => {
  const icons = {
    internal: 'ri-arrow-right-line',
    external: 'ri-external-link-line',
    email: 'ri-mail-line',
    phone: 'ri-phone-line'
  }
  return icons[props.linkType] || 'ri-link'
})

const linkText = computed(() => props.text || props.destination)
</script>
```

### Conditional Link Rendering
```vue
<template>
  <div class="conditional-links">
    <codex-anchor
      v-if="user.isAuthenticated"
      :href="'/dashboard'"
      :default-text="'Go to Dashboard'"
      :anchor-class="'primary-link'"
    >
      <template #before>
        <i class="ri-dashboard-line"></i>
      </template>
    </codex-anchor>
    
    <codex-anchor
      v-else
      :href="'/login'"
      :default-text="'Login'"
      :anchor-class="'primary-link'"
    >
      <template #before>
        <i class="ri-login-circle-line"></i>
      </template>
    </codex-anchor>
    
    <codex-anchor
      v-if="user.isAdmin"
      :href="'/admin'"
      :default-text="'Admin Panel'"
      :anchor-class="'admin-link'"
    >
      <template #before>
        <i class="ri-admin-line"></i>
      </template>
    </codex-anchor>
  </div>
</template>

<script setup>
const user = ref({
  isAuthenticated: false,
  isAdmin: false
})
</script>
```

### Localized Links
```vue
<template>
  <div class="localized-links">
    <codex-anchor
      :href="'/help'"
      :default-text="$t('navigation.help')"
      :anchor-class="'help-link'"
    >
      <template #before>
        <i class="ri-question-line"></i>
      </template>
    </codex-anchor>
    
    <codex-anchor
      :href="'/contact'"
      :default-text="$t('navigation.contact')"
      :anchor-class="'contact-link'"
    >
      <template #before>
        <i class="ri-mail-line"></i>
      </template>
    </codex-anchor>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'

const { t } = useI18n()
</script>
```

### Card Action Links
```vue
<template>
  <div class="action-cards">
    <div class="action-card">
      <h3>Create New Project</h3>
      <p>Start a new project from scratch</p>
      <codex-anchor
        :href="'/projects/new'"
        :default-text="'Create Project'"
        :anchor-class="'card-action'"
      >
        <template #after>
          <i class="ri-arrow-right-line"></i>
        </template>
      </codex-anchor>
    </div>
    
    <div class="action-card">
      <h3>Import Project</h3>
      <p>Import an existing project</p>
      <codex-anchor
        :href="'/projects/import'"
        :default-text="'Import Project'"
        :anchor-class="'card-action'"
      >
        <template #after>
          <i class="ri-upload-line"></i>
        </template>
      </codex-anchor>
    </div>
  </div>
</template>
```

### Link List with Descriptions
```vue
<template>
  <div class="link-list">
    <div class="link-item">
      <codex-anchor
        :href="'/docs/getting-started'"
        :anchor-class="'link-item-anchor'"
      >
        <template #before>
          <i class="ri-book-open-line"></i>
        </template>
        <template #default>
          <div class="link-content">
            <h4>Getting Started Guide</h4>
            <p>Learn the basics and set up your first project</p>
          </div>
        </template>
      </codex-anchor>
    </div>
    
    <div class="link-item">
      <codex-anchor
        :href="'/docs/api'"
        :anchor-class="'link-item-anchor'"
      >
        <template #before>
          <i class="ri-code-line"></i>
        </template>
        <template #default>
          <div class="link-content">
            <h4>API Reference</h4>
            <p>Complete API documentation and examples</p>
          </div>
        </template>
      </codex-anchor>
    </div>
  </div>
</template>
```

## CSS Classes
- `_c-link`: Default anchor styling (applied when anchorClass is empty)
- Custom classes via anchorClass prop

## Best Practices

### Recommended Usage Patterns
- Use descriptive link text that clearly indicates destination
- Add target="_blank" for external links opening in new tabs
- Include visual indicators for external links using after slots
- Use before slots for consistent icon placement
- Apply semantic CSS classes for different link types
- Provide meaningful anchor text for accessibility
- Use internal routing paths for same-site navigation

### Common Pitfalls to Avoid
- Using generic link text like "click here" or "read more"
- Not indicating external links that open in new tabs
- Missing href attribute (required prop)
- Using links for actions that should be buttons
- Not providing accessible link text
- Overusing custom CSS classes without consistency

### Accessibility Considerations
- Always provide descriptive and meaningful link text
- Indicate external links that open in new tabs
- Ensure sufficient color contrast for link text
- Support keyboard navigation (Enter key to activate)
- Use semantic HTML structure consistently
- Provide context for links when necessary
- Test with screen readers for proper announcements

### Link Text Guidelines
- Use clear and descriptive text that indicates the link destination
- Avoid generic phrases like "click here" or "more info"
- Keep link text concise but informative
- Use consistent language across similar link types
- Consider the context where the link appears
- Make link purpose obvious from the text alone

### External Link Handling
- Always use target="_blank" for external links
- Consider adding rel="noopener noreferrer" for security
- Provide visual indication (icon) for external links
- Test external links regularly for validity
- Handle external link failures gracefully
- Warn users when leaving the application

### Internal Navigation
- Use application routing paths for internal links
- Coordinate with Vue Router for SPA navigation
- Handle authentication requirements for protected routes
- Provide loading states for navigation when needed
- Use consistent URL patterns across the application

### Performance Considerations
- Minimize re-renders when link props don't change
- Use computed properties for dynamic link generation
- Optimize icon loading for before/after slots
- Handle large link lists efficiently
- Implement proper cleanup for event listeners

### SEO Considerations
- Use descriptive anchor text for search engine optimization
- Include relevant keywords in link text when appropriate
- Structure internal linking to support site navigation
- Handle canonical URLs appropriately
- Test link crawlability for important pages

### Security Considerations
- Validate external URLs to prevent malicious links
- Use rel="noopener noreferrer" for external links with target="_blank"
- Sanitize dynamic href values to prevent XSS
- Handle user-generated link content carefully
- Implement Content Security Policy for external resources

### Mobile Considerations
- Ensure links have sufficient touch target size
- Test link interaction on touch devices
- Handle responsive design for link layouts
- Consider mobile-specific link behaviors
- Test accessibility on mobile screen readers

### State Management
- Use reactive properties for dynamic href values
- Handle link state changes appropriately
- Coordinate with application routing state
- Track link interaction for analytics when needed
- Handle authentication state changes

## Component Registration
The component is registered as `codex-anchor` in the application. 