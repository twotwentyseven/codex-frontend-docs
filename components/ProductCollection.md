# ProductCollection Component

## Overview
The ProductCollection component displays a mixed collection of different product types (plans, bundles, gift cards) in a unified interface. It dynamically renders the appropriate card component for each product type and provides a complete solution for showcasing diverse product collections with loading states, empty state handling, and customizable layouts. The component automatically detects product types and renders the corresponding card components.

## Basic Usage
```vue
<template>
  <div class="product-collection">
    <codex-product-collection 
      :handle="'featured-products'"
      :title="'Featured Products'"
      :per-page="12"
    />
  </div>
</template>

<script setup>
// Product collection automatically loads based on handle
</script>
```

## Key Features
- Mixed product type display (plans, bundles, gift cards)
- Dynamic component rendering based on product type
- Loading states with skeleton placeholders
- Empty state handling with customizable messages
- Automatic product type detection and card selection
- Grid layout with responsive design
- Handle-based or URL-based collection loading
- Internationalization support
- Slot-based customization for all sections
- "Powered By" footer integration

## Configuration Props

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `title` | `String` | `''` | Page title, falls back to translation |
| `handle` | `String` | `undefined` | Collection handle, auto-detected from URL if not provided |

### Display Control Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `hideIfNoResults` | `Boolean` | `false` | Hide component when no products available |
| `hideDisabledBundles` | `Boolean` | `false` | Hide bundles that cannot be purchased |
| `showPricesInline` | `Boolean` | `false` | Display prices inline with product details |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `alignment` | `String` | `'center'` | Content alignment (start, end, center) |
| `perPage` | `Number` | `27` | Number of products to display per page |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Enables border styling on product cards |
| `titleTag` | HTML tag for the title element |
| `showPoweredBy` | Controls "Powered By" footer display |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatCurrency` | Formats product prices for display |
| `truncateString` | Truncates long descriptions |
| `toggleOpen` | Handles expandable content sections |

## Product Type Mapping

The component automatically maps product types to their corresponding card components:

| Product Type | Card Component | Description |
|--------------|----------------|-------------|
| `plan` | `codex-plan-card` | Subscription plans |
| `bundle` | `codex-bundle-card` | Bundle packages |
| `gift_card_product` | `codex-gift-card-card` | Gift card products |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| N/A | N/A | Component uses internal loading system |

## Slots

### Header Slot
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ productCollection }` | Custom header content |

### Content Slot
| Slot | Props | Description |
|------|-------|-------------|
| `content` | `{ productCollection }` | Main content area with products grid |
| `no-results` | N/A | Empty state message |

### Footer Slot
| Slot | Props | Description |
|------|-------|-------------|
| `footer` | `{ productCollection }` | Footer content |

## Loading States

The component provides loading states through:
- Skeleton cards during data fetching
- Loading indicators on product cards
- Conditional rendering based on `loading` state
- Dynamic component loading for different product types

## URL Integration

The component can automatically detect the collection handle from the URL:
- Uses the last path segment as the handle if no handle prop provided
- Supports clean URL structures like `/collections/featured-products`
- Falls back to provided handle prop if URL detection fails

## Examples

### Basic Product Collection
```vue
<template>
  <div class="collection-page">
    <codex-product-collection 
      :handle="'best-sellers'"
      :title="'Best Selling Products'"
      :per-page="9"
    />
  </div>
</template>
```

### Mixed Product Collection with Custom Content
```vue
<template>
  <div class="featured-collection">
    <codex-product-collection 
      :handle="'featured-collection'"
      :hide-if-no-results="true"
      :show-prices-inline="true"
    >
      <template #header="{ productCollection }">
        <div class="collection-header">
          <h2>Featured Product Collection</h2>
          <p>Handpicked products for the best value and experience</p>
          
          <div class="collection-stats">
            <div class="stat-item">
              <span class="stat-value">{{ productCollection?.products?.length || 0 }}</span>
              <span class="stat-label">Products</span>
            </div>
            <div class="stat-item">
              <span class="stat-value">{{ getProductTypeCount(productCollection, 'plan') }}</span>
              <span class="stat-label">Plans</span>
            </div>
            <div class="stat-item">
              <span class="stat-value">{{ getProductTypeCount(productCollection, 'bundle') }}</span>
              <span class="stat-label">Bundles</span>
            </div>
            <div class="stat-item">
              <span class="stat-value">{{ getProductTypeCount(productCollection, 'gift_card_product') }}</span>
              <span class="stat-label">Gift Cards</span>
            </div>
          </div>
          
          <!-- Collection Features -->
          <div class="collection-features">
            <div class="feature-item">
              <i class="ri-star-line"></i>
              <span>Curated Selection</span>
            </div>
            <div class="feature-item">
              <i class="ri-discount-percent-line"></i>
              <span>Best Value Guarantee</span>
            </div>
            <div class="feature-item">
              <i class="ri-customer-service-line"></i>
              <span>Expert Support</span>
            </div>
          </div>
        </div>
      </template>
      
      <template #content="{ productCollection }">
        <div class="collection-showcase">
          <!-- Product Type Sections -->
          <div v-if="getProductsByType(productCollection, 'plan').length" class="product-section">
            <h3>Subscription Plans</h3>
            <p>Ongoing access to our services with flexible billing</p>
            <div class="products-grid">
              <codex-plan-card
                v-for="plan in getProductsByType(productCollection, 'plan')"
                :key="plan.id"
                :product="plan"
                :enable-border="true"
                :show-price="true"
                :variable-start-date="true"
              />
            </div>
          </div>
          
          <div v-if="getProductsByType(productCollection, 'bundle').length" class="product-section">
            <h3>Bundle Packages</h3>
            <p>Value-packed combinations with significant savings</p>
            <div class="products-grid">
              <codex-bundle-card
                v-for="bundle in getProductsByType(productCollection, 'bundle')"
                :key="bundle.id"
                :product="bundle"
                :enable-border="true"
                :show-price="true"
                :show-price-per-credit="true"
              />
            </div>
          </div>
          
          <div v-if="getProductsByType(productCollection, 'gift_card_product').length" class="product-section">
            <h3>Gift Cards</h3>
            <p>Perfect gifts for friends and family</p>
            <div class="products-grid">
              <codex-gift-card-card
                v-for="giftCard in getProductsByType(productCollection, 'gift_card_product')"
                :key="giftCard.id"
                :product="giftCard"
                :enable-border="true"
                :show-price="true"
                :show-title="true"
              />
            </div>
          </div>
          
          <!-- All Products Mixed View -->
          <div class="mixed-products-section">
            <div class="section-header">
              <h3>All Products</h3>
              <div class="view-controls">
                <button 
                  @click="viewMode = 'mixed'" 
                  :class="{ active: viewMode === 'mixed' }"
                  class="view-btn"
                >
                  Mixed View
                </button>
                <button 
                  @click="viewMode = 'grouped'" 
                  :class="{ active: viewMode === 'grouped' }"
                  class="view-btn"
                >
                  Grouped View
                </button>
              </div>
            </div>
            
            <div v-if="viewMode === 'mixed'" class="mixed-grid">
              <component 
                v-for="product in productCollection.products" 
                :key="product.id"
                :is="`codex-${getCardComponent(product.type)}-card`"
                :product="product"
                :enable-border="true"
                :show-price="true"
              />
            </div>
          </div>
          
          <!-- Product Comparison -->
          <div class="comparison-section">
            <h3>Compare Products</h3>
            <div class="comparison-intro">
              <p>Not sure which option is right for you? Compare our products side by side.</p>
              <button @click="openComparison" class="comparison-btn">
                <i class="ri-scales-line"></i>
                Compare All Products
              </button>
            </div>
          </div>
        </div>
      </template>
      
      <template #footer="{ productCollection }">
        <div class="collection-footer">
          <div class="help-section">
            <h4>Need Help Choosing?</h4>
            <p>Our product specialists can help you find the perfect option for your needs.</p>
            <div class="help-options">
              <button @click="scheduleConsultation" class="consultation-btn">
                Free Consultation
              </button>
              <button @click="openLiveChat" class="chat-btn">
                Live Chat
              </button>
            </div>
          </div>
          
          <div class="guarantee-section">
            <h4>Our Promise to You</h4>
            <div class="guarantees">
              <div class="guarantee-item">
                <i class="ri-shield-check-line"></i>
                <span>30-day money back guarantee</span>
              </div>
              <div class="guarantee-item">
                <i class="ri-customer-service-line"></i>
                <span>24/7 customer support</span>
              </div>
              <div class="guarantee-item">
                <i class="ri-secure-payment-line"></i>
                <span>Secure payment processing</span>
              </div>
            </div>
          </div>
        </div>
      </template>
      
      <template #no-results>
        <div class="no-products-available">
          <div class="no-results-icon">
            <i class="ri-shopping-cart-line"></i>
          </div>
          <h3>No Products Available</h3>
          <p>This collection is currently being updated. Please check back soon for new products!</p>
          <div class="no-results-actions">
            <button @click="browseAllProducts" class="browse-btn">
              Browse All Products
            </button>
            <button @click="notifyWhenAvailable" class="notify-btn">
              Get Notified
            </button>
          </div>
        </div>
      </template>
    </codex-product-collection>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const viewMode = ref('mixed')

const getProductTypeCount = (collection, type) => {
  return collection?.products?.filter(product => product.type === type).length || 0
}

const getProductsByType = (collection, type) => {
  return collection?.products?.filter(product => product.type === type) || []
}

const getCardComponent = (type) => {
  switch (type) {
    case 'plan': return 'plan'
    case 'bundle': return 'bundle'
    case 'gift_card_product': return 'gift-card'
    default: return 'plan'
  }
}

const openComparison = () => {
  console.log('Opening product comparison')
  // Implementation would open comparison modal
}

const scheduleConsultation = () => {
  console.log('Scheduling consultation')
  // Implementation would open booking interface
}

const openLiveChat = () => {
  console.log('Opening live chat')
  // Implementation would open chat widget
}

const browseAllProducts = () => {
  console.log('Browsing all products')
  // Implementation would navigate to all products page
}

const notifyWhenAvailable = () => {
  console.log('Setting up notifications')
  // Implementation would collect email for notifications
}
</script>

<style scoped>
.collection-header {
  text-align: center;
  margin-bottom: 3rem;
}

.collection-stats {
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

.collection-features {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin-top: 2rem;
  flex-wrap: wrap;
}

.feature-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: #666;
}

.product-section {
  margin: 3rem 0;
}

.product-section h3 {
  text-align: center;
  margin-bottom: 0.5rem;
}

.product-section p {
  text-align: center;
  color: #666;
  margin-bottom: 2rem;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.view-controls {
  display: flex;
  gap: 0.5rem;
}

.view-btn {
  padding: 0.5rem 1rem;
  border: 1px solid #ddd;
  background: white;
  cursor: pointer;
  transition: all 0.2s;
}

.view-btn.active {
  background: #007bff;
  color: white;
  border-color: #007bff;
}

.mixed-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.comparison-intro {
  text-align: center;
  padding: 2rem;
  background: #f8f9fa;
  border-radius: 12px;
}

.comparison-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 1rem 2rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  margin-top: 1rem;
}

.collection-footer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 3rem;
  margin-top: 3rem;
  padding: 3rem 0;
  border-top: 1px solid #eee;
}

.help-options {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
}

.guarantees {
  display: grid;
  gap: 1rem;
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
```

### URL-Based Collection Loading
```vue
<template>
  <div class="dynamic-collection">
    <!-- Component automatically uses URL path for handle -->
    <codex-product-collection 
      :title="'Dynamic Collection'"
      :per-page="15"
    />
  </div>
</template>

<script setup>
// When visiting /collections/summer-sale, 
// component will automatically load 'summer-sale' collection
</script>
```

### Holiday Collection with Custom Layout
```vue
<template>
  <div class="holiday-collection">
    <codex-product-collection 
      :handle="'holiday-collection'"
      :hide-if-no-results="true"
    >
      <template #header="{ productCollection }">
        <div class="holiday-header">
          <div class="holiday-banner">
            <h2>🎄 Holiday Collection 🎁</h2>
            <p>Special holiday offers and gift ideas</p>
          </div>
          
          <div class="holiday-countdown">
            <h4>Limited Time Offers End In:</h4>
            <countdown-timer :end-date="holidayEndDate" />
          </div>
        </div>
      </template>
      
      <template #content="{ productCollection }">
        <div class="holiday-content">
          <!-- Gift Cards Section -->
          <div v-if="hasGiftCards(productCollection)" class="gift-section">
            <h3>🎁 Perfect Gifts</h3>
            <div class="gift-cards-row">
              <codex-gift-card-card
                v-for="giftCard in getGiftCards(productCollection)"
                :key="giftCard.id"
                :product="giftCard"
                :enable-border="true"
                :show-title="true"
              />
            </div>
          </div>
          
          <!-- Holiday Bundles -->
          <div v-if="hasBundles(productCollection)" class="bundle-section">
            <h3>🎄 Holiday Bundles</h3>
            <div class="bundles-showcase">
              <codex-bundle-card
                v-for="bundle in getBundles(productCollection)"
                :key="bundle.id"
                :product="bundle"
                :enable-border="true"
                :show-price-per-credit="true"
              />
            </div>
          </div>
          
          <!-- All Holiday Products -->
          <div class="all-products">
            <h3>All Holiday Products</h3>
            <div class="products-grid">
              <component 
                v-for="product in productCollection.products" 
                :key="product.id"
                :is="`codex-${getCardComponent(product.type)}-card`"
                :product="product"
                :enable-border="false"
              />
            </div>
          </div>
        </div>
      </template>
    </codex-product-collection>
  </div>
</template>

<script setup>
const holidayEndDate = new Date('2024-12-31T23:59:59')

const hasGiftCards = (collection) => {
  return collection?.products?.some(p => p.type === 'gift_card_product') || false
}

const getGiftCards = (collection) => {
  return collection?.products?.filter(p => p.type === 'gift_card_product') || []
}

const hasBundles = (collection) => {
  return collection?.products?.some(p => p.type === 'bundle') || false
}

const getBundles = (collection) => {
  return collection?.products?.filter(p => p.type === 'bundle') || []
}

const getCardComponent = (type) => {
  switch (type) {
    case 'plan': return 'plan'
    case 'bundle': return 'bundle'
    case 'gift_card_product': return 'gift-card'
    default: return 'plan'
  }
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-products` | Main container class |
| `_c-header` | Header section styling |
| `_c-content` | Content area styling |
| `_c-footer` | Footer section styling |
| `_c-grid` | Grid layout for product cards |
| `_c-no-results` | Empty state styling |

## Internationalization

The component uses the following translation keys:

### Core Interface Messages
| Key | Usage |
|-----|-------|
| `product_collection.title` | Default collection title when title prop is not provided |
| `product_collection.introduction` | Collection introduction/description text |
| `product_collection.no_products_available` | No products available message |

### Translation Usage Examples
```vue
<!-- Collection title with fallback -->
<codex-title 
  :tag="titleTag" 
  class-name="_c-text-3xl _c-text-bold" 
  :content="title || $t('product_collection.title')" 
/>

<!-- Collection introduction -->
<codex-paragraph 
  tag="p" 
  :content="$t('product_collection.introduction')" 
/>

<!-- No products message -->
<div v-if="!productCollection?.products?.length && !loading" class="_c-no-results">
  <slot name="no-results">{{ $t("product_collection.no_products_available") }}</slot>
</div>
```

### Translation Notes

#### Collection-Level Translations
The ProductCollection component provides translations for:
- Collection title and header information
- Introduction and description text
- Empty state messaging when no products are available

#### Product Card Integration
The component dynamically renders different product card types, each handling their own translations:

**Plan Cards** (`codex-plan-card`):
- Plan names, descriptions, and pricing
- Billing interval translations (monthly, yearly, etc.)
- Credit and usage information
- Action buttons (subscribe, manage, etc.)

**Bundle Cards** (`codex-bundle-card`):
- Bundle names and descriptions
- Credit quantities and pricing
- Expiry information and time periods
- Add to cart and purchase actions

**Gift Card Cards** (`codex-gift-card-card`):
- Gift card denominations and values
- Recipient and sender information
- Purchase and delivery options
- Terms and conditions

#### Dynamic Component Loading
The component uses a mapping function to determine which card component to render:

```javascript
const getCardComponent = (type) => {
  switch (type) {
    case 'plan': return 'plan';           // Uses plan translation keys
    case 'bundle': return 'bundle';       // Uses bundle translation keys  
    case 'gift_card_product': return 'gift-card'; // Uses gift card translation keys
  }
};
```

#### Error Handling Integration
The component integrates with error handling that uses translations for:
- Loading state messages
- API error responses
- Network connectivity issues
- Data validation errors

#### Customization Through Slots
Translation can be customized through slot content:

```vue
<codex-product-collection handle="featured">
  <template #header>
    <h2>{{ $t('collections.featured.title') }}</h2>
    <p>{{ $t('collections.featured.description') }}</p>
  </template>
  
  <template #no-results>
    <div class="custom-empty-state">
      <h3>{{ $t('collections.empty.title') }}</h3>
      <p>{{ $t('collections.empty.message') }}</p>
      <button>{{ $t('collections.empty.browse_all') }}</button>
    </div>
  </template>
</codex-product-collection>
```

## Best Practices

### Collection Management
- Use descriptive collection handles that reflect content
- Organize collections logically by theme, category, or purpose
- Implement proper SEO for collection pages
- Consider collection hierarchy and navigation
- Provide clear collection descriptions and purposes

### Product Display
- Ensure consistent styling across different product types
- Provide appropriate spacing and layout for mixed content
- Handle different card sizes and content appropriately
- Implement responsive design for various screen sizes
- Use consistent interaction patterns across product types

### Performance
- Implement lazy loading for large collections
- Cache collection data to reduce API calls
- Optimize images and assets for all product types
- Use efficient rendering for mixed product grids
- Consider pagination for very large collections

### User Experience
- Provide clear navigation within collections
- Implement product filtering and sorting when appropriate
- Show loading states during collection fetching
- Handle empty states gracefully with helpful messaging
- Enable easy product comparison functionality

### Accessibility
- Ensure proper heading hierarchy throughout collections
- Provide descriptive alt text for all product images
- Support keyboard navigation through product grids
- Use appropriate ARIA labels for dynamic content
- Test with screen readers for all product types

### URL Structure
- Use clean, SEO-friendly URLs for collections
- Implement proper URL routing for collection handles
- Support deep linking to specific collections
- Handle URL parameters for filtering and sorting
- Provide breadcrumb navigation when appropriate

## Component Registration
```javascript
// Global registration
app.component('CodexProductCollection', ProductCollection)

// Local registration  
import ProductCollection from '@/components/products/ProductCollection.vue'

export default {
  components: {
    CodexProductCollection: ProductCollection
  }
}
``` 