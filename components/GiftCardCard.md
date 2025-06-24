# GiftCardCard Component

## Overview
The GiftCardCard component displays individual gift card products with comprehensive cart integration, variant selection, and customizable content. It provides a complete interface for showcasing gift card details, pricing options, and purchase functionality with support for multiple denominations, loading states, and extensive customization. The component handles variant selection automatically and integrates seamlessly with the cart system.

## Basic Usage
```vue
<template>
  <div class="gift-card-display">
    <codex-gift-card-card 
      :product="selectedGiftCard"
      :enable-border="true"
      :show-price="true"
      :show-title="true"
    />
  </div>
</template>

<script setup>
const selectedGiftCard = {
  id: 1,
  name: 'Store Gift Card',
  handle: 'store-gift-card',
  featured: false,
  expires_offset: '12 months',
  variants: [
    { id: 1, formatted_price: '$25.00' },
    { id: 2, formatted_price: '$50.00' },
    { id: 3, formatted_price: '$100.00' }
  ]
}
</script>
```

## Key Features
- Individual gift card display with detailed information
- Cart integration with add-to-cart functionality
- Multiple variant/denomination support
- Automatic variant selection for single variants
- Loading states with skeleton card
- Conditional rendering and display options
- Custom SVG branding integration
- Pricing display with currency formatting
- Internationalization support
- Extensive slot customization

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `product` | `Object\|Number` | `required` | Gift card object or ID |

### Display Control Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `loading` | `Boolean` | `false` | Show skeleton loading state |
| `hideIfDisabled` | `Boolean` | `false` | Hide card if gift card cannot be purchased |
| `showPrice` | `Boolean` | `false` | Display price in button text |
| `showTitle` | `Boolean` | `true` | Display gift card name in header |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Enables border styling on the gift card |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatCurrency` | Formats gift card prices for display |

## Computed Properties

### Variant Management
- `hasVariants` - Whether gift card has multiple variants
- `currentVariant` - Currently selected variant object
- `giftcard` - Reference to the product prop

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| N/A | N/A | Component uses internal cart system |

## Slots

### Header Slot
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ giftcard, add, adding, added, error, fieldErrors, genericErrors, formatCurrency, selectedVariant }` | Custom header content |

### Content Slot
| Slot | Props | Description |
|------|-------|-------------|
| `content` | `{ giftcard, add, adding, added, error, fieldErrors, genericErrors, formatCurrency, selectedVariant }` | Main gift card content |

### Footer Slot
| Slot | Props | Description |
|------|-------|-------------|
| `footer` | `{ giftcard, add, adding, added, error, fieldErrors, genericErrors, formatCurrency, selectedVariant }` | Footer with variant selection and purchase button |

## Loading States

The component provides comprehensive loading states:
- Skeleton card with structured layout during loading
- Loading indicators on purchase buttons
- Conditional rendering based on data availability
- Loading states for cart operations

## Variant Selection

The component handles variant selection automatically:
- Single variant: Auto-selected on mount
- Multiple variants: Dropdown selection required
- Purchase button disabled until variant selected
- Price display updates based on selection

## Examples

### Basic Gift Card
```vue
<template>
  <div class="simple-gift-card">
    <codex-gift-card-card 
      :product="basicGiftCard"
      :show-title="true"
      :show-price="false"
    />
  </div>
</template>

<script setup>
const basicGiftCard = {
  id: 1,
  name: 'Digital Gift Card',
  handle: 'digital-gift-card',
  featured: false,
  expires_offset: '24 months',
  variants: [
    { id: 1, formatted_price: '$50.00' }
  ]
}
</script>
```

### Multi-Variant Gift Card with Custom Content
```vue
<template>
  <div class="premium-gift-card">
    <codex-gift-card-card 
      :product="premiumGiftCard"
      :show-price="true"
      :show-title="true"
    >
      <template #content="{ giftcard, formatCurrency, selectedVariant }">
        <div class="premium-content">
          <!-- Custom Header -->
          <div class="gift-card-header">
            <h3 class="gift-card-title">{{ giftcard.name }}</h3>
            <div class="premium-badge">
              <i class="ri-vip-crown-line"></i>
              <span>Premium Collection</span>
            </div>
          </div>
          
          <!-- Price Display -->
          <div class="price-section">
            <div class="main-price">
              <span v-if="hasMultipleVariants && !selectedVariant" class="from-text">
                From
              </span>
              <span class="price-amount">
                {{ getCurrentPrice(giftcard, selectedVariant) }}
              </span>
            </div>
            
            <div class="value-proposition">
              <div class="value-item">
                <i class="ri-shield-check-line"></i>
                <span>Never expires</span>
              </div>
              <div class="value-item">
                <i class="ri-smartphone-line"></i>
                <span>Digital delivery</span>
              </div>
              <div class="value-item">
                <i class="ri-gift-line"></i>
                <span>Perfect for any occasion</span>
              </div>
            </div>
          </div>
          
          <!-- Custom Features -->
          <div class="gift-card-features">
            <h4>What Makes This Special</h4>
            <div class="features-grid">
              <div class="feature">
                <i class="ri-palette-line"></i>
                <div>
                  <h5>Custom Design</h5>
                  <p>Choose from beautiful themes</p>
                </div>
              </div>
              <div class="feature">
                <i class="ri-calendar-event-line"></i>
                <div>
                  <h5>Scheduled Delivery</h5>
                  <p>Send on any date you choose</p>
                </div>
              </div>
              <div class="feature">
                <i class="ri-message-3-line"></i>
                <div>
                  <h5>Personal Message</h5>
                  <p>Add your own heartfelt note</p>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Usage Information -->
          <div class="usage-info">
            <h4>How to Use</h4>
            <ol class="usage-steps">
              <li>Receive your digital gift card via email</li>
              <li>Present the code at checkout online or in-store</li>
              <li>Enjoy your purchase with no hidden fees</li>
            </ol>
          </div>
          
          <!-- Company Branding -->
          <div class="company-branding">
            <svg class="company-logo" viewBox="0 0 365 38" xmlns="http://www.w3.org/2000/svg">
              <path d="M26.9,44.65 C21.3999725,44.65 17.016683,42.8000185 13.75,39.1 C10.5833175,35.5333155 9,31.1000265 9,25.8 C9,20.4999735 10.5833175,16.083351 13.75,12.55 C17.08335,8.8499815 21.4666395,7 26.9,7 C30.833353,7 34.366651,8.0833225 37.5,10.25 C40.16668,12.050009 42.133327,14.5999835 43.4,17.9 L36.9,20.85 L36.85,20.85 L36.4,19.8 C35.666663,18.066658 34.5500075,16.700005 33.05,15.7 C31.516659,14.6666615 29.75001,14.15 27.75,14.15 C25.616656,14.15 23.8666735,14.449997 22.5,15.05 C21.1333265,15.650003 20.0000045,16.449995 19.1,17.45 C17.233324,19.5500105 16.3,22.333316 16.3,25.8 C16.3,29.200017 17.233324,31.9833225 19.1,34.15 C21.0000095,36.350011 23.883314,37.45 27.75,37.45 C29.816677,37.45 31.583326,36.950005 33.05,35.95 C34.6166745,34.9166615 35.73333,33.5500085 36.4,31.85 L36.85,30.75 L36.9,30.75 L43.4,33.75 C42.0999935,36.9833495 40.1333465,39.533324 37.5,41.4 C34.3999845,43.5666775 30.8666865,44.65 26.9,44.65 Z"/>
            </svg>
          </div>
        </div>
      </template>
      
      <template #footer="{ giftcard, add, adding, selectedVariant }">
        <div class="premium-footer">
          <!-- Variant Selection -->
          <div v-if="hasMultipleVariants" class="variant-selection">
            <label for="gift-card-amount">Choose Amount</label>
            <select 
              id="gift-card-amount"
              v-model="selectedVariant"
              class="amount-select"
            >
              <option value="">Select amount</option>
              <option 
                v-for="variant in giftcard.variants" 
                :key="variant.id"
                :value="variant.id"
              >
                {{ variant.formatted_price }}
              </option>
            </select>
          </div>
          
          <!-- Purchase Section -->
          <div class="purchase-section">
            <button
              @click="add"
              :disabled="!selectedVariant"
              :class="['purchase-btn', { 'loading': adding, 'disabled': !selectedVariant }]"
            >
              <span v-if="adding" class="loading-content">
                <i class="ri-loader-4-line spinning"></i>
                Adding to Cart...
              </span>
              <span v-else>
                Add Gift Card
                <span v-if="selectedVariant && showPrice" class="btn-price">
                  - {{ getCurrentVariantPrice(selectedVariant, giftcard.variants) }}
                </span>
              </span>
            </button>
            
            <!-- Security Badge -->
            <div class="security-info">
              <i class="ri-secure-payment-line"></i>
              <span>Secure checkout with SSL encryption</span>
            </div>
          </div>
          
          <!-- Delivery Information -->
          <div class="delivery-info">
            <div class="delivery-method">
              <i class="ri-mail-line"></i>
              <span>Instant email delivery</span>
            </div>
            <div class="expiry-info" v-if="giftcard.expires_offset">
              <i class="ri-time-line"></i>
              <span>Valid for {{ giftcard.expires_offset }}</span>
            </div>
          </div>
        </div>
      </template>
    </codex-gift-card-card>
  </div>
</template>

<script setup>
const premiumGiftCard = {
  id: 2,
  name: 'Premium Gift Experience',
  handle: 'premium-gift-experience',
  featured: true,
  expires_offset: 'never expires',
  variants: [
    { id: 10, formatted_price: '$25.00' },
    { id: 11, formatted_price: '$50.00' },
    { id: 12, formatted_price: '$75.00' },
    { id: 13, formatted_price: '$100.00' },
    { id: 14, formatted_price: '$150.00' },
    { id: 15, formatted_price: '$200.00' }
  ]
}

const hasMultipleVariants = computed(() => {
  return premiumGiftCard.variants.length > 1
})

const getCurrentPrice = (giftcard, selectedVariant) => {
  if (!selectedVariant && giftcard.variants.length > 0) {
    return giftcard.variants[0].formatted_price
  }
  
  const variant = giftcard.variants.find(v => v.id === selectedVariant)
  return variant ? variant.formatted_price : giftcard.variants[0].formatted_price
}

const getCurrentVariantPrice = (selectedVariantId, variants) => {
  const variant = variants.find(v => v.id === selectedVariantId)
  return variant ? variant.formatted_price : ''
}
</script>

<style scoped>
.premium-content {
  padding: 1.5rem;
}

.gift-card-header {
  text-align: center;
  margin-bottom: 1.5rem;
}

.premium-badge {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: linear-gradient(45deg, #gold, #orange);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 15px;
  font-size: 0.875rem;
  margin-top: 0.5rem;
}

.price-section {
  text-align: center;
  margin: 1.5rem 0;
}

.main-price {
  font-size: 2rem;
  font-weight: bold;
  margin-bottom: 1rem;
}

.value-proposition {
  display: flex;
  justify-content: center;
  gap: 1rem;
  flex-wrap: wrap;
}

.value-item {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  font-size: 0.875rem;
  color: #666;
}

.features-grid {
  display: grid;
  gap: 1rem;
  margin-top: 1rem;
}

.feature {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
}

.usage-steps {
  margin: 1rem 0;
  padding-left: 1.5rem;
}

.company-logo {
  height: 30px;
  fill: currentColor;
}

.variant-selection {
  margin-bottom: 1rem;
}

.amount-select {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  margin-top: 0.5rem;
}

.purchase-btn {
  width: 100%;
  padding: 1rem;
  border: none;
  border-radius: 8px;
  background: #007bff;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.2s;
}

.purchase-btn:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.spinning {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.security-info,
.delivery-method,
.expiry-info {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  color: #666;
  margin-top: 0.5rem;
}
</style>
```

### Corporate Gift Cards with Bulk Options
```vue
<template>
  <div class="corporate-gift-cards">
    <div class="bulk-options">
      <h3>Corporate Gift Cards</h3>
      <p>Perfect for employee rewards and client appreciation</p>
    </div>
    
    <div class="gift-cards-grid">
      <codex-gift-card-card 
        v-for="giftCard in corporateGiftCards"
        :key="giftCard.id"
        :product="giftCard"
        :show-title="true"
        :show-price="true"
      >
        <template #header="{ giftcard }">
          <div class="corporate-header">
            <div class="corporate-badge">
              <i class="ri-building-line"></i>
              <span>Corporate</span>
            </div>
            <h4>{{ giftcard.name }}</h4>
          </div>
        </template>
        
        <template #footer="{ add, adding, selectedVariant, giftcard }">
          <div class="corporate-footer">
            <!-- Variant Selection -->
            <select v-model="selectedVariant" class="corporate-amount-select">
              <option value="">Select Amount</option>
              <option 
                v-for="variant in giftcard.variants" 
                :key="variant.id"
                :value="variant.id"
              >
                {{ variant.formatted_price }}
              </option>
            </select>
            
            <!-- Bulk Quantity -->
            <div class="quantity-section">
              <label for="quantity">Quantity</label>
              <select id="quantity" v-model="selectedQuantity">
                <option value="1">1 card</option>
                <option value="5">5 cards (5% off)</option>
                <option value="10">10 cards (10% off)</option>
                <option value="25">25 cards (15% off)</option>
                <option value="50">50 cards (20% off)</option>
              </select>
            </div>
            
            <!-- Total Calculation -->
            <div class="total-calculation" v-if="selectedVariant && selectedQuantity">
              <div class="base-total">
                Subtotal: {{ calculateSubtotal(selectedVariant, selectedQuantity, giftcard.variants) }}
              </div>
              <div class="discount" v-if="getDiscount(selectedQuantity) > 0">
                Discount ({{ getDiscount(selectedQuantity) }}%): 
                -{{ calculateDiscount(selectedVariant, selectedQuantity, giftcard.variants) }}
              </div>
              <div class="final-total">
                Total: {{ calculateTotal(selectedVariant, selectedQuantity, giftcard.variants) }}
              </div>
            </div>
            
            <!-- Purchase Button -->
            <button
              @click="addBulkOrder"
              :disabled="!selectedVariant || adding"
              class="corporate-purchase-btn"
            >
              <span v-if="adding">Processing...</span>
              <span v-else>Add to Corporate Cart</span>
            </button>
            
            <!-- Corporate Features -->
            <div class="corporate-features">
              <div class="feature">
                <i class="ri-file-text-line"></i>
                <span>Detailed invoice provided</span>
              </div>
              <div class="feature">
                <i class="ri-calendar-line"></i>
                <span>Scheduled delivery available</span>
              </div>
              <div class="feature">
                <i class="ri-customer-service-line"></i>
                <span>Dedicated support</span>
              </div>
            </div>
          </div>
        </template>
      </codex-gift-card-card>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const selectedQuantity = ref(1)

const corporateGiftCards = [
  {
    id: 100,
    name: 'Corporate Dining Card',
    handle: 'corporate-dining',
    featured: false,
    expires_offset: '12 months',
    variants: [
      { id: 101, formatted_price: '$25.00' },
      { id: 102, formatted_price: '$50.00' },
      { id: 103, formatted_price: '$100.00' }
    ]
  },
  {
    id: 200,
    name: 'Employee Wellness Card',
    handle: 'employee-wellness',
    featured: true,
    expires_offset: '24 months',
    variants: [
      { id: 201, formatted_price: '$30.00' },
      { id: 202, formatted_price: '$60.00' },
      { id: 203, formatted_price: '$120.00' }
    ]
  }
]

const getDiscount = (quantity) => {
  if (quantity >= 50) return 20
  if (quantity >= 25) return 15
  if (quantity >= 10) return 10
  if (quantity >= 5) return 5
  return 0
}

const calculateSubtotal = (variantId, quantity, variants) => {
  const variant = variants.find(v => v.id === variantId)
  if (!variant) return '$0.00'
  
  const price = parseFloat(variant.formatted_price.replace('$', ''))
  const subtotal = price * quantity
  return `$${subtotal.toFixed(2)}`
}

const calculateDiscount = (variantId, quantity, variants) => {
  const variant = variants.find(v => v.id === variantId)
  if (!variant) return '$0.00'
  
  const price = parseFloat(variant.formatted_price.replace('$', ''))
  const subtotal = price * quantity
  const discountPercent = getDiscount(quantity)
  const discount = subtotal * (discountPercent / 100)
  return `$${discount.toFixed(2)}`
}

const calculateTotal = (variantId, quantity, variants) => {
  const variant = variants.find(v => v.id === variantId)
  if (!variant) return '$0.00'
  
  const price = parseFloat(variant.formatted_price.replace('$', ''))
  const subtotal = price * quantity
  const discountPercent = getDiscount(quantity)
  const total = subtotal * (1 - discountPercent / 100)
  return `$${total.toFixed(2)}`
}

const addBulkOrder = () => {
  console.log('Adding bulk corporate gift card order')
  // Implementation would handle bulk cart addition
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-card` | Base card styling |
| `_c-giftcard-card` | Gift card specific styling |
| `_c-featured` | Featured gift card styling |
| `_c-header` | Header section styling |
| `_c-content` | Content area styling |
| `_c-footer` | Footer section styling |
| `_c-title` | Title styling |
| `_c-product-title` | Product title styling |
| `_c-focal-text` | Prominent text styling |
| `_c-product-price` | Price display styling |
| `_c-from` | "From" price prefix styling |
| `_c-product-expiry` | Expiry information styling |
| `_c-text-sm` | Small text styling |
| `_c-btn-container` | Button container styling |
| `_c-product-btn` | Product button styling |

## Internationalization

The component uses the following translation keys:

### Core Gift Card Information
| Key | Usage |
|-----|-------|
| `giftcards.from` | "From" prefix for variable pricing |
| `giftcards.expiry` | Expiry information label |
| `giftcards.choose_value` | Variant selection label and placeholder |

### Cart and Purchase Actions
| Key | Usage |
|-----|-------|
| `product.add_to_cart` | Add to cart button text |
| `product.adding` | Adding to cart loading state |
| `product.unavailable` | Unavailable state text |
| `product.error` | Generic error state text |
| `cart.added_to_cart` | Success toast message |

### Translation Usage Examples
```vue
<!-- Gift card pricing display -->
<div class="gift-card-price">
  <template v-if="!hasVariants || selectedVariant == false">
    <span v-if="selectedVariant == false" class="from-text">
      {{ $t("giftcards.from") }}
    </span>
    {{ giftcard.variants[0].formatted_price }}
  </template>
</div>

<!-- Gift card expiry information -->
<div v-if="giftcard.expires_offset" class="gift-card-expiry">
  {{ $t("giftcards.expiry") }} {{ giftcard.expires_offset }}
</div>

<!-- Variant selection dropdown -->
<codex-select-field 
  v-if="hasVariants"
  option-name="formatted_price"
  option-value="id"
  :label="$t('giftcards.choose_value')"
  :options="giftcard.variants"
  v-model="selectedVariant"
  :placeholder="$t('giftcards.choose_value')"
/>

<!-- Add to cart button with states -->
<codex-button
  :processingText="$t('product.adding')"
  :defaultText="$t('product.add_to_cart')"
  :disabledText="$t('product.unavailable')"
  :errorText="$t('product.error')"
  :processing="adding"
  :disabled="selectedVariant == false"
  @click="add"
>
  <template v-if="showPrice" #after>
    - {{ currentVariant.formatted_price }}
  </template>
</codex-button>
```

### Additional Translation Considerations

#### Variant Handling
- The component automatically handles variant selection and pricing display
- Translation keys support both single and multiple variant scenarios
- The "from" prefix is only shown for multiple variants when no specific variant is selected

#### Cart Integration
- Uses standard product translation keys for consistency with other product cards
- Success messages are displayed via toast notifications using `cart.added_to_cart`
- Error handling follows the same pattern as other cart-enabled components

#### Expiry Information
- The `giftcards.expiry` key is used to display expiration information
- Actual expiry periods come from the product data and may need localization based on the time format preferences

## Best Practices

### Gift Card Display
- Use clear, attractive visual presentation
- Provide comprehensive pricing information
- Support multiple denomination options effectively
- Include relevant expiry and usage information
- Implement proper variant selection UX

### User Experience
- Auto-select single variants for immediate purchase
- Show loading states during cart operations
- Provide clear feedback for selection states
- Implement smooth cart integration
- Handle errors gracefully with user feedback

### Performance
- Use skeleton loading for better perceived performance
- Optimize gift card imagery for fast loading
- Cache variant data appropriately
- Implement efficient cart operations
- Consider lazy loading for multiple cards

### Accessibility
- Ensure proper keyboard navigation through variant selection
- Provide descriptive labels for all form inputs
- Use appropriate ARIA attributes for dynamic content
- Test with screen readers for gift card information
- Maintain sufficient color contrast for all elements

### Security
- Implement proper gift card validation
- Use secure payment processing
- Validate variant selections server-side
- Monitor for fraudulent purchases
- Implement proper gift card code generation

### Mobile Responsiveness
- Ensure variant selection works well on touch devices
- Optimize card layout for small screens
- Test purchase flow on mobile devices
- Consider collapsible content for detailed information
- Maintain readability across all screen sizes

## Component Registration
```javascript
// Global registration
app.component('CodexGiftCardCard', GiftCardCard)

// Local registration  
import GiftCardCard from '@/components/products/GiftCardCard.vue'

export default {
  components: {
    CodexGiftCardCard: GiftCardCard
  }
}
``` 