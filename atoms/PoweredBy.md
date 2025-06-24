# PoweredBy Component

## Overview
The PoweredBy component provides a secure, encapsulated branding element that displays "Powered by CodexFit" with an SVG logo. It uses Shadow DOM for style isolation, preventing CSS interference, and includes conditional visibility controls. The component automatically handles internationalization and can be globally disabled via configuration settings.

## Basic Usage
```vue
<template>
  <div class="footer">
    <codex-powered-by 
      :href="'https://codexfit.com'"
      :show-powered-by="true"
    />
  </div>
</template>
```

## Key Features
- Shadow DOM encapsulation for style isolation
- Conditional rendering based on global configuration
- Internationalized text support
- Customizable target URL
- SVG logo integration with current color inheritance
- Secure external link handling (target="_blank")
- Automatic hiding when globally disabled
- Minimal visual footprint

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `href` | `String` | `'https://codexfit.com'` | Target URL for the powered by link |
| `showPoweredBy` | `Boolean` | `true` | Whether to show the powered by component |

## Events
This component does not emit any events.

## Slots
This component does not provide any slots.

## States

### Default State
```vue
<codex-powered-by />
```

### Hidden State (Global Config)
```vue
<!-- Component automatically hides when window.codex.hidePoweredBy is true -->
<codex-powered-by :show-powered-by="false" />
```

### Custom URL State
```vue
<codex-powered-by 
  :href="'https://custom-domain.com'"
  :show-powered-by="true"
/>
```

## Internationalization Keys

| Key | Default | Description |
|-----|---------|-------------|
| `timetable.powered_by` | "Powered by" | Text displayed before logo |

## Examples

### Basic Footer Integration
```vue
<template>
  <footer class="site-footer">
    <div class="footer-content">
      <div class="footer-links">
        <a href="/privacy">Privacy Policy</a>
        <a href="/terms">Terms of Service</a>
      </div>
      
      <codex-powered-by />
    </div>
  </footer>
</template>

<style>
.site-footer {
  background: #f5f5f5;
  padding: 20px;
  margin-top: auto;
}

.footer-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
}
</style>
```

### Conditional Display Based on Plan
```vue
<template>
  <div class="app-footer">
    <codex-powered-by 
      :show-powered-by="shouldShowPoweredBy"
      :href="brandingUrl"
    />
  </div>
</template>

<script setup>
import { computed } from 'vue'

const userPlan = ref('free') // Could be 'free', 'premium', 'enterprise'
const customBranding = ref(false)

const shouldShowPoweredBy = computed(() => {
  return userPlan.value === 'free' && !customBranding.value
})

const brandingUrl = computed(() => {
  return customBranding.value ? 
    'https://your-brand.com' : 
    'https://codexfit.com'
})
</script>
```

### Timetable Integration
```vue
<template>
  <div class="timetable-widget">
    <div class="timetable-header">
      <h2>{{ $t('timetable.title') }}</h2>
    </div>
    
    <div class="timetable-content">
      <!-- Timetable content here -->
      <div class="time-slots">
        <!-- Time slot components -->
      </div>
    </div>
    
    <div class="timetable-footer">
      <codex-powered-by />
    </div>
  </div>
</template>

<style>
.timetable-widget {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
}

.timetable-footer {
  padding: 12px 16px;
  background: #fafafa;
  border-top: 1px solid #e0e0e0;
}
</style>
```

### Embeddable Widget
```vue
<template>
  <div class="booking-widget">
    <div class="widget-content">
      <!-- Booking form content -->
      <booking-form @submit="handleBooking" />
    </div>
    
    <div class="widget-branding">
      <codex-powered-by 
        :show-powered-by="!widget.hideBranding"
        :href="widget.brandingUrl || 'https://codexfit.com'"
      />
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  widget: {
    type: Object,
    default: () => ({
      hideBranding: false,
      brandingUrl: null
    })
  }
})

const handleBooking = (bookingData) => {
  // Handle booking submission
  console.log('Booking submitted:', bookingData)
}
</script>
```

### Multi-Language Support
```vue
<template>
  <div class="language-switcher-footer">
    <div class="language-controls">
      <button @click="setLanguage('en')">English</button>
      <button @click="setLanguage('es')">Español</button>
      <button @click="setLanguage('fr')">Français</button>
    </div>
    
    <codex-powered-by />
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'

const { locale } = useI18n()

const setLanguage = (lang) => {
  locale.value = lang
  // The PoweredBy component will automatically update its text
}
</script>
```

### White-Label Configuration
```vue
<template>
  <div class="app">
    <!-- Main app content -->
    <main-content />
    
    <!-- Conditionally show branding based on configuration -->
    <codex-powered-by 
      v-if="showBranding"
      :href="brandingConfig.url"
    />
  </div>
</template>

<script setup>
import { computed, onMounted } from 'vue'

const brandingConfig = ref({
  show: true,
  url: 'https://codexfit.com'
})

const showBranding = computed(() => {
  // Check global config and local setting
  return !window.codex?.hidePoweredBy && brandingConfig.value.show
})

onMounted(() => {
  // Load branding configuration from API
  loadBrandingConfig()
})

const loadBrandingConfig = async () => {
  try {
    const config = await fetchBrandingConfig()
    brandingConfig.value = config
  } catch (error) {
    console.error('Failed to load branding config:', error)
  }
}
</script>
```

## Global Configuration

### Hiding PoweredBy Globally
```javascript
// Set in your main application configuration
window.codex = {
  hidePoweredBy: true, // This will hide all PoweredBy components
  api: 'https://api.yourapp.com'
}
```

### Environment-Based Configuration
```javascript
// In your environment configuration
const config = {
  development: {
    hidePoweredBy: false
  },
  production: {
    hidePoweredBy: process.env.HIDE_POWERED_BY === 'true'
  }
}

window.codex = {
  ...config[process.env.NODE_ENV],
  api: process.env.API_URL
}
```

## CSS Classes (Shadow DOM)

Since this component uses Shadow DOM, its styles are encapsulated and cannot be overridden from the parent document. The internal styles include:

| Class | Description |
|-------|-------------|
| `.powered-by` | Main link container with flex layout |
| `.info` | Text container with small font size |
| `svg` | Logo styling with currentColor fill |

## Best Practices

### Accessibility
- Uses semantic anchor tag for proper link navigation
- Includes target="_blank" for external link indication
- Inherits color from parent for theme integration
- Maintains readable font size and spacing

### Performance
- Uses Shadow DOM for style isolation and performance
- Minimal DOM footprint with single host element
- SVG logo is inline to avoid additional network requests
- Lazy initialization prevents unnecessary rendering

### Integration
- Place in footer or bottom sections of layouts
- Respect global configuration settings
- Use consistent styling with application theme
- Consider white-label requirements for different clients

### Internationalization
- Always use translation keys for text content
- Test with different language strings for layout
- Consider text length variations across languages
- Ensure RTL language support if needed

### Configuration Management
- Check global configuration before rendering
- Respect client-specific branding requirements
- Use environment variables for deployment flexibility
- Provide clear documentation for configuration options

### Security
- Uses target="_blank" with implicit security measures
- Shadow DOM provides style isolation
- No user input processing reduces attack surface
- External links are properly handled

### User Experience
- Minimal visual impact on main content
- Consistent placement across application pages
- Respects user's color scheme preferences
- Professional appearance that builds trust

## Component Registration
```javascript
// Global registration
app.component('CodexPoweredBy', PoweredBy)

// Local registration  
import PoweredBy from '@/components/atoms/PoweredBy.vue'

export default {
  components: {
    CodexPoweredBy: PoweredBy
  }
}
``` 