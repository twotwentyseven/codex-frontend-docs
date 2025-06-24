# Bundles Component

## Overview
The Bundles component displays a collection of bundle products with filtering capabilities, loading states, and comprehensive cart integration. It provides a complete interface for browsing and selecting bundle packages with support for various configuration options, custom layouts, and interactive features. The component integrates with filter contexts, handles empty states, and supports customizable templates for different business scenarios.

## Basic Usage
```vue
<template>
  <div class="bundles-section">
    <codex-bundles 
      :title="'Bundle Packages'"
      :hide-if-no-results="false"
      :group="'product-bundles'"
      :alignment="'center'"
    />
  </div>
</template>

<script setup>
// Bundles automatically load via composition API
</script>
```

## Key Features
- Bundle product display with card-based layout
- Filter integration with primary and secondary filters
- Loading states with skeleton placeholders
- Empty state handling with customizable messages
- Cart integration for bundle purchases
- Support for inline pricing and custom alignment
- Grid layout with responsive design
- Internationalization support
- Carousel functionality support (commented in template)
- Slot-based customization for all sections

## Configuration Props

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `title` | `String` | `''` | Page title, falls back to translation |

### Display Control Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `hideIfNoResults` | `Boolean` | `false` | Hide component when no bundles available |
| `hideDisabledBundles` | `Boolean` | `false` | Hide bundles that cannot be purchased |
| `showPricesInline` | `Boolean` | `false` | Display prices inline with bundle details |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `alignment` | `String` | `'center'` | Content alignment (start, end, center) |

### Content Wrapper Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `contentWrapperType` | `String` | `''` | Type of content wrapper |
| `contentWrapperSettings` | `Object` | `{}` | Settings for content wrapper |

### Filter Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `group` | `String` | `'default'` | Filter group identifier |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |
| `...filters.props` | `Various` | Inherits filter-related props |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Enables border styling on bundle cards |
| `titleTag` | HTML tag for the title element |
| `showPoweredBy` | Controls "Powered By" footer display |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatCurrency` | Formats bundle prices for display |
| `truncateString` | Truncates long descriptions |
| `toggleOpen` | Handles expandable content sections |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| N/A | N/A | Component uses internal cart and filter systems |

## Slots

### Header Slot
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ bundles }` | Custom header content |

### Filter Slots
| Slot | Props | Description |
|------|-------|-------------|
| `filters` | `{ group }` | Complete filter wrapper |
| `primary-filters` | `{ group }` | Primary filter content |
| `secondary-filters` | `{ group }` | Secondary filter content |

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `content` | `{ bundles }` | Main content area with bundles grid |
| `no-results` | N/A | Empty state message |

### Footer Slot
| Slot | Props | Description |
|------|-------|-------------|
| `footer` | `{ bundles }` | Footer content |

## Loading States

The component provides loading states through:
- Skeleton cards during data fetching
- Loading indicators on bundle cards
- Conditional rendering based on `loading` state
- Integration with filter context ready state

## Filter Integration

The component integrates with the filter system through:
- Filter context detection and registration
- Automatic data loading when filters change
- Primary and secondary filter slot support
- Results count display in filter wrapper

## Examples

### Basic Bundle Display
```vue
<template>
  <div class="bundles-page">
    <codex-bundles 
      :title="'Value Bundles'"
      :group="'value-bundles'"
      :alignment="'center'"
    />
  </div>
</template>
```

### Bundles with Custom Filtering
```vue
<template>
  <div class="filtered-bundles">
    <codex-filter-context :group="'fitness-bundles'" :default-values="defaultFilters">
      <template #default="{ filters }">
        <codex-bundles 
          :group="'fitness-bundles'"
          :title="'Fitness Bundles'"
          :hide-disabled-bundles="true"
          :show-prices-inline="true"
        >
          <template #primary-filters="{ group }">
            <codex-contextual-filter
              :group="group"
              :definition="'bundle_types'"
              :filter-key="'bundle_type'"
              :label="'Bundle Type'"
              :filter-type="'radio'"
              :primary-filter="true"
            />
            
            <codex-contextual-filter
              :group="group"
              :definition="'price_ranges'"
              :filter-key="'price_range'"
              :label="'Price Range'"
              :filter-type="'select'"
              :primary-filter="true"
            />
          </template>
          
          <template #secondary-filters="{ group }">
            <codex-contextual-filter
              :group="group"
              :definition="'bundle_categories'"
              :filter-key="'category_ids'"
              :label="'Categories'"
              :filter-type="'checkbox-group'"
            />
            
            <codex-contextual-filter
              :group="group"
              :definition="'difficulty_levels'"
              :filter-key="'difficulty'"
              :label="'Difficulty Level'"
              :filter-type="'multi-select'"
            />
          </template>
        </codex-bundles>
      </template>
    </codex-filter-context>
  </div>
</template>

<script setup>
const defaultFilters = {
  bundle_type: '',
  price_range: '',
  category_ids: [],
  difficulty: []
}
</script>
```

### Premium Bundle Packages with Custom Content
```vue
<template>
  <div class="premium-bundles">
    <codex-bundles 
      :group="'premium-bundles'"
      :hide-if-no-results="true"
      :show-prices-inline="true"
    >
      <template #header="{ bundles }">
        <div class="premium-header">
          <h2>Premium Bundle Packages</h2>
          <p>Comprehensive packages designed for serious fitness enthusiasts</p>
          
          <div class="bundle-statistics">
            <div class="stat-item">
              <span class="stat-value">{{ bundles?.length || 0 }}</span>
              <span class="stat-label">Bundle Packages</span>
            </div>
            <div class="stat-item">
              <span class="stat-value">{{ calculateTotalValue(bundles) }}</span>
              <span class="stat-label">Total Savings</span>
            </div>
            <div class="stat-item">
              <span class="stat-value">{{ getAverageCredits(bundles) }}</span>
              <span class="stat-label">Avg Credits</span>
            </div>
          </div>
          
          <!-- Bundle Benefits -->
          <div class="bundle-benefits">
            <h3>Why Choose Bundles?</h3>
            <div class="benefits-grid">
              <div class="benefit-item">
                <i class="ri-discount-percent-line"></i>
                <div>
                  <h4>Save Money</h4>
                  <p>Get more value compared to individual purchases</p>
                </div>
              </div>
              <div class="benefit-item">
                <i class="ri-gift-line"></i>
                <div>
                  <h4>Bonus Credits</h4>
                  <p>Extra credits included in every bundle</p>
                </div>
              </div>
              <div class="benefit-item">
                <i class="ri-time-line"></i>
                <div>
                  <h4>Extended Validity</h4>
                  <p>Longer expiration periods for bundle credits</p>
                </div>
              </div>
              <div class="benefit-item">
                <i class="ri-vip-crown-line"></i>
                <div>
                  <h4>Priority Access</h4>
                  <p>Skip waiting lists with bundle member status</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
      
      <template #content="{ bundles }">
        <div class="premium-bundles-showcase">
          <!-- Featured Bundles -->
          <div v-if="featuredBundles.length" class="featured-section">
            <h3>Staff Picks</h3>
            <div class="featured-bundles-grid">
              <codex-bundle-card
                v-for="bundle in featuredBundles"
                :key="bundle.id"
                :product="bundle"
                :enable-border="true"
                :show-price="true"
                :show-price-per-credit="true"
              />
            </div>
          </div>
          
          <!-- All Bundles -->
          <div class="all-bundles-section">
            <h3>All Bundle Packages</h3>
            <div class="bundles-comparison">
              <div class="comparison-header">
                <button 
                  @click="viewMode = 'grid'" 
                  :class="{ active: viewMode === 'grid' }"
                  class="view-toggle"
                >
                  <i class="ri-grid-line"></i> Grid View
                </button>
                <button 
                  @click="viewMode = 'comparison'" 
                  :class="{ active: viewMode === 'comparison' }"
                  class="view-toggle"
                >
                  <i class="ri-table-line"></i> Compare
                </button>
              </div>
              
              <!-- Grid View -->
              <div v-if="viewMode === 'grid'" class="bundles-grid">
                <codex-bundle-card
                  v-for="bundle in bundles"
                  :key="bundle.id"
                  :product="bundle"
                  :enable-border="false"
                  :show-price="true"
                  :show-price-per-credit="false"
                />
              </div>
              
              <!-- Comparison View -->
              <div v-else class="comparison-table">
                <bundle-comparison-table :bundles="bundles" />
              </div>
            </div>
          </div>
          
          <!-- Bundle Builder -->
          <div class="bundle-builder-section">
            <h3>Create Your Own Bundle</h3>
            <div class="builder-intro">
              <p>Can't find the perfect bundle? Build your own custom package!</p>
              <button @click="openBundleBuilder" class="builder-btn">
                <i class="ri-settings-3-line"></i>
                Build Custom Bundle
              </button>
            </div>
          </div>
        </div>
      </template>
      
      <template #footer="{ bundles }">
        <div class="premium-footer">
          <div class="guarantee-section">
            <h4>Bundle Guarantee</h4>
            <div class="guarantee-items">
              <div class="guarantee-item">
                <i class="ri-shield-check-line"></i>
                <span>30-day satisfaction guarantee</span>
              </div>
              <div class="guarantee-item">
                <i class="ri-exchange-line"></i>
                <span>Flexible credit exchange policy</span>
              </div>
              <div class="guarantee-item">
                <i class="ri-customer-service-line"></i>
                <span>Dedicated bundle support</span>
              </div>
            </div>
          </div>
          
          <div class="contact-support">
            <h4>Need Help Choosing?</h4>
            <p>Our bundle specialists can help you find the perfect package</p>
            <div class="support-options">
              <button @click="scheduleConsultation" class="consultation-btn">
                Schedule Consultation
              </button>
              <button @click="contactSupport" class="support-btn">
                Contact Support
              </button>
            </div>
          </div>
        </div>
      </template>
      
      <template #no-results>
        <div class="no-bundles-available">
          <div class="no-results-icon">
            <i class="ri-package-line"></i>
          </div>
          <h3>No Bundles Available</h3>
          <p>We're currently updating our bundle offerings. Check back soon for amazing deals!</p>
          <div class="no-results-actions">
            <button @click="notifyWhenAvailable" class="notify-btn">
              Get Notified
            </button>
            <button @click="viewIndividualProducts" class="individual-btn">
              View Individual Products
            </button>
          </div>
        </div>
      </template>
    </codex-bundles>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const viewMode = ref('grid')

const featuredBundles = computed(() => {
  return bundles.value?.filter(bundle => bundle.is_featured) || []
})

const calculateTotalValue = (bundles) => {
  if (!bundles?.length) return '$0'
  // Calculate total savings across all bundles
  const totalSavings = bundles.reduce((sum, bundle) => {
    return sum + (bundle.individual_price - bundle.price || 0)
  }, 0)
  return `$${totalSavings.toFixed(0)}`
}

const getAverageCredits = (bundles) => {
  if (!bundles?.length) return 0
  const totalCredits = bundles.reduce((sum, bundle) => sum + bundle.total_credits, 0)
  return Math.round(totalCredits / bundles.length)
}

const openBundleBuilder = () => {
  console.log('Opening bundle builder')
  // Implementation would open bundle builder interface
}

const scheduleConsultation = () => {
  console.log('Scheduling consultation')
  // Implementation would open consultation booking
}

const contactSupport = () => {
  console.log('Contacting support')
  // Implementation would open support chat/form
}

const notifyWhenAvailable = () => {
  console.log('Setting up availability notification')
  // Implementation would collect email for notifications
}

const viewIndividualProducts = () => {
  console.log('Redirecting to individual products')
  // Implementation would navigate to products page
}
</script>

<style scoped>
.premium-header {
  text-align: center;
  margin-bottom: 3rem;
}

.bundle-statistics {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin: 2rem 0;
  flex-wrap: wrap;
}

.stat-item {
  text-align: center;
}

.stat-value {
  display: block;
  font-size: 2rem;
  font-weight: bold;
  color: #007bff;
}

.stat-label {
  font-size: 0.875rem;
  color: #666;
}

.benefits-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}

.benefit-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding: 1.5rem;
  background: #f8f9fa;
  border-radius: 12px;
}

.benefit-item i {
  font-size: 1.5rem;
  color: #007bff;
  margin-top: 0.25rem;
}

.view-toggle {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  border: 1px solid #ddd;
  background: white;
  cursor: pointer;
  transition: all 0.2s;
}

.view-toggle.active {
  background: #007bff;
  color: white;
  border-color: #007bff;
}

.builder-intro {
  text-align: center;
  padding: 3rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 12px;
}

.builder-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 1rem 2rem;
  background: white;
  color: #333;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  margin-top: 1rem;
}

.guarantee-items {
  display: flex;
  justify-content: center;
  gap: 2rem;
  flex-wrap: wrap;
  margin-top: 1rem;
}

.guarantee-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.no-results-icon {
  font-size: 4rem;
  color: #ccc;
  margin-bottom: 1rem;
}

.no-results-actions {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-top: 2rem;
}
</style>

## Internationalization

The component uses the following translation keys:

### Bundle Interface Messages
| Key | Usage |
|-----|-------|
| `bundles.title` | Default bundles page title |
| `bundles.introduction` | Bundles page introduction text |
| `bundle.no_bundles_available` | No bundles available message |

### Translation Usage Examples
```vue
<!-- Bundles header -->
<div class="_c-header">
  <codex-title :tag="titleTag" :content="title || $t('bundles.title')" />
  <codex-paragraph tag="p" :content="$t('bundles.introduction')" />
  <codex-error :error="genericErrors" />
</div>

<!-- No results state -->
<div v-if="!bundles?.length && !hideIfNoResults && !loading" class="_c-no-results">
  <slot name="no-results">{{ $t("bundle.no_bundles_available") }}</slot>
</div>
```

### Translation Notes

#### Collection-Level Translations
The Bundles component handles collection-level messaging:
- Page title with fallback to translation when title prop is not provided
- Introduction text to describe bundle offerings
- No results messaging for empty collections

#### Child Component Integration
The component delegates detailed translations to child components:
- `codex-bundle-card` components handle individual bundle translations
- Bundle cards include pricing, features, and purchase action translations
- Filter components handle search and filtering translations

#### Filter System Integration
The component integrates with filter systems that have their own translations:
- Primary and secondary filter slots support
- Filter result count display
- Filter context integration for data loading

#### Customizable Content
The component supports content customization through slots:
- No results slot allows custom empty state messages
- Header and footer slots support custom content with translations
- Filter slots enable custom filter interfaces with appropriate translations

## Usage Examples

### Basic Bundles Display
```vue
<codex-bundles>
  <template #header>
    <h2>{{ $t('bundles.title') }}</h2>
    <p>{{ $t('bundles.description') }}</p>
  </template>
</codex-bundles>
```

### With Custom Empty State
```vue
<codex-bundles>
  <template #no-results>
    <div class="custom-empty-state">
      <h3>{{ $t('bundles.empty.title') }}</h3>
      <p>{{ $t('bundles.empty.description') }}</p>
    </div>
  </template>
</codex-bundles>
```

## Best Practices

### Bundle Configuration
- Provide clear value propositions for bundle packages
- Show savings compared to individual purchases
- Include comprehensive bundle feature lists
- Implement proper bundle availability logic
- Display clear expiration and usage policies

### User Experience
- Show loading states during data fetching
- Provide clear empty state messaging
- Enable bundle comparison functionality
- Support bundle preview and detailed views
- Implement clear call-to-action buttons

### Performance
- Implement lazy loading for large bundle collections
- Cache bundle data to reduce API calls
- Optimize images and assets used in bundle cards
- Use skeleton loading for better perceived performance
- Consider pagination for extensive bundle catalogs

### Filtering
- Provide intuitive filter options relevant to bundles
- Show filter result counts
- Allow filter clearing and reset
- Persist filter state in URL when appropriate
- Group filters logically (price, type, category)

### Accessibility
- Ensure proper heading hierarchy
- Provide descriptive alt text for bundle images
- Support keyboard navigation through bundles
- Use appropriate ARIA labels for interactive elements
- Test with screen readers

### Bundle Pricing
- Display clear pricing information including savings
- Show credit counts and value per credit
- Include any additional fees or restrictions
- Provide bundle comparison tools
- Implement dynamic pricing updates

## Component Registration
```javascript
// Global registration
app.component('CodexBundles', Bundles)

// Local registration  
import Bundles from '@/components/products/Bundles.vue'

export default {
  components: {
    CodexBundles: Bundles
  }
}
``` 