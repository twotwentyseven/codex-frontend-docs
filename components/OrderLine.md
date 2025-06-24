# OrderLine Component

## Overview
The OrderLine component displays individual line items within orders, featuring comprehensive product information, pricing details, subscription management, and trial period handling. It supports various line item types including one-time purchases, subscriptions, and bundled products with automatic formatting for prices, trial periods, and ongoing billing information. The component provides skeleton loading states and integrates seamlessly with order management systems.

## Basic Usage
```vue
<template>
  <div class="order-lines-container">
    <codex-order-line
      v-for="(line, index) in orderLines"
      :key="line.id"
      :line="line"
      :line-key="index"
      :last-index="orderLines.length - 1"
    >
      <template #item-header>
        <div class="custom-header">
          <span class="product-code">{{ line.sku }}</span>
          <span class="product-name">{{ line.product }}</span>
        </div>
      </template>
      
      <template #item-content>
        <div class="custom-content">
          <div class="product-details">
            <span>Quantity: {{ line.quantity }}</span>
            <span v-if="line.trial_days">Trial: {{ line.trial_days }} days</span>
          </div>
        </div>
      </template>
    </codex-order-line>
  </div>
</template>

<script setup>
const orderLines = ref([
  {
    id: 1,
    product: 'Premium Plan',
    final_price: '$29.99',
    ongoing_price: '$29.99/month', 
    quantity: 1,
    trial_days: 14,
    subscription_active: true
  },
  {
    id: 2,
    product: 'Setup Fee',
    final_price: '$9.99',
    quantity: 1,
    one_time: true
  }
])
</script>
```

## Key Features
- Individual order line item display
- Product name and pricing presentation
- Subscription and trial period management
- Quantity and billing information display
- Automatic divider management between items
- Skeleton loading states for better UX
- Customizable sections through slots
- Support for one-time and recurring items

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `line` | `Object` | `required` | Line item data containing product, price, and subscription information |
| `lineKey` | `Number` | `required` | Index of current line item for divider logic |
| `lastIndex` | `Number` | `undefined` | Index of last item to control divider display |

## Slots

| Slot | Description |
|------|-------------|
| `item-header` | Custom header content replacing product name and price |
| `item-content` | Custom content replacing quantity and billing information |
| `item-footer` | Custom footer content below line item details |

## Line Item Data Structure

### Basic Product Line
```javascript
{
  id: 1,
  product: 'Wireless Headphones',
  final_price: '$199.99',
  quantity: 1,
  sku: 'WH-001',
  one_time: true
}
```

### Subscription Line
```javascript
{
  id: 2,
  product: 'Premium Plan',
  final_price: '$0.00',
  ongoing_price: '$29.99/month',
  quantity: 1,
  trial_days: 14,
  subscription_active: true,
  next_billing_date: '2024-02-01'
}
```

### Bundle Line Item
```javascript
{
  id: 3,
  product: 'Complete Package',
  final_price: '$99.99',
  original_price: '$149.99',
  quantity: 1,
  bundle_items: [
    'Premium Plan',
    'Additional Storage',
    'Priority Support'
  ],
  discount_percentage: 33
}
```

## Internationalization

The `OrderLine` component uses several translation keys for displaying subscription/product line information:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `cart.joining_fee` | Displays the initial joining fee label | One-time subscription fees |
| `cart.recurring_payments` | Shows recurring payment label | Monthly/weekly subscription fees |
| `cart.trial_start_date` | Trial period start date label | When subscription has trial period |
| `cart.membership_start` | Membership activation date label | When payments begin |

### Implementation Examples

```vue
<!-- Joining fee display -->
<div v-if="line.ongoing_final_price_raw > 0" class="_c-joining-fee">
    {{ $t('cart.joining_fee') }}
    <span>{{ line.final_price }}</span>
</div>

<!-- Recurring payments -->
<div v-if="line.ongoing_final_price_raw > 0" class="_c-recurring-payments">
    {{ $t('cart.recurring_payments') }}
    <span>{{ line.ongoing_final_price }}</span>
</div>

<!-- Trial period information -->
<div v-if="line.buyable?.trial_days > 0" class="_c-trial-start">
    {{ $t('cart.trial_start_date') }}
    <span>{{ startAt }}</span>
</div>

<!-- Membership start date -->
<div v-if="line.lineOngoingPriceRaw > 0" class="_c-membership-start">
    {{ $t('cart.membership_start') }}
    <span>{{ $filters.dateFormat(line.properties.payment_starts_at, 'Do MMM YYYY') }}</span>
</div>
```

### Notes
- All translation keys are related to subscription billing and payment structure
- Conditional rendering based on subscription properties
- Integration with date formatting filters for membership start dates
- Pricing displays are managed through the line object properties rather than translations

## Examples

### E-commerce Order Lines
```vue
<template>
  <div class="ecommerce-order-lines">
    <codex-order-line
      v-for="(item, index) in productLines"
      :key="item.id"
      :line="item"
      :line-key="index"
      :last-index="productLines.length - 1"
    >
      <template #item-header>
        <div class="product-header">
          <div class="product-info">
            <h4 class="product-name">{{ item.product }}</h4>
            <div class="product-meta">
              <span class="sku">SKU: {{ item.sku }}</span>
              <span class="category">{{ item.category }}</span>
            </div>
          </div>
          <div class="pricing-info">
            <div v-if="item.original_price && item.original_price !== item.final_price" class="original-price">
              <span class="crossed-out">{{ item.original_price }}</span>
            </div>
            <div class="final-price">{{ item.final_price }}</div>
            <div v-if="item.discount_percentage" class="discount-badge">
              {{ item.discount_percentage }}% OFF
            </div>
          </div>
        </div>
      </template>
      
      <template #item-content>
        <div class="product-content">
          <div class="quantity-info">
            <span>Quantity:</span>
            <span class="quantity-value">{{ item.quantity }}</span>
          </div>
          <div v-if="item.specifications" class="specifications">
            <div v-for="spec in item.specifications" :key="spec.name" class="spec-item">
              <span>{{ spec.name }}:</span>
              <span>{{ spec.value }}</span>
            </div>
          </div>
          <div v-if="item.warranty" class="warranty-info">
            <i class="ri-shield-check-line"></i>
            <span>{{ item.warranty }} warranty included</span>
          </div>
        </div>
      </template>
      
      <template #item-footer>
        <div class="product-footer">
          <div v-if="item.estimated_delivery" class="delivery-info">
            <i class="ri-truck-line"></i>
            <span>Estimated delivery: {{ item.estimated_delivery }}</span>
          </div>
          <div v-if="item.tracking_number" class="tracking-info">
            <i class="ri-map-pin-line"></i>
            <span>Tracking: {{ item.tracking_number }}</span>
          </div>
        </div>
      </template>
    </codex-order-line>
  </div>
</template>

<script setup>
const productLines = ref([
  {
    id: 1,
    product: 'Wireless Noise-Canceling Headphones',
    sku: 'WH-XM4-001',
    category: 'Electronics',
    final_price: '$199.99',
    original_price: '$249.99',
    discount_percentage: 20,
    quantity: 1,
    specifications: [
      { name: 'Color', value: 'Black' },
      { name: 'Connectivity', value: 'Bluetooth 5.0' },
      { name: 'Battery Life', value: '30 hours' }
    ],
    warranty: '2-year',
    estimated_delivery: 'Jan 25, 2024',
    tracking_number: 'TRK123456789'
  },
  {
    id: 2,
    product: 'Premium Phone Case',
    sku: 'PC-IP14-BLK',
    category: 'Accessories',
    final_price: '$24.99',
    quantity: 2,
    specifications: [
      { name: 'Material', value: 'Silicone' },
      { name: 'Drop Protection', value: '6 feet' }
    ],
    warranty: '1-year',
    estimated_delivery: 'Jan 23, 2024'
  }
])
</script>
```

### Subscription Order Lines
```vue
<template>
  <div class="subscription-order-lines">
    <codex-order-line
      v-for="(subscription, index) in subscriptionLines"
      :key="subscription.id"
      :line="subscription"
      :line-key="index"
      :last-index="subscriptionLines.length - 1"
    >
      <template #item-header>
        <div class="subscription-header">
          <div class="plan-info">
            <h4 class="plan-name">
              {{ subscription.product }}
              <span class="plan-tier">{{ subscription.tier }}</span>
            </h4>
            <div class="billing-info">
              <span class="billing-cycle">{{ subscription.billing_cycle }}</span>
              <span v-if="subscription.trial_days" class="trial-badge">
                {{ subscription.trial_days }} day trial
              </span>
            </div>
          </div>
          <div class="subscription-pricing">
            <div v-if="subscription.trial_days" class="trial-price">
              <span class="trial-amount">{{ subscription.final_price }}</span>
              <span class="trial-period">for {{ subscription.trial_days }} days</span>
            </div>
            <div class="ongoing-price">
              <span class="then-label">{{ $t('order.ongoing_price') }}</span>
              <span class="recurring-amount">{{ subscription.ongoing_price }}</span>
            </div>
          </div>
        </div>
      </template>
      
      <template #item-content>
        <div class="subscription-content">
          <div class="subscription-details">
            <div class="detail-row">
              <span>Status:</span>
              <span class="status" :class="getStatusClass(subscription.status)">
                {{ subscription.status }}
              </span>
            </div>
            <div v-if="subscription.next_billing_date" class="detail-row">
              <span>Next billing:</span>
              <span>{{ formatDate(subscription.next_billing_date) }}</span>
            </div>
            <div v-if="subscription.cancel_at_period_end" class="detail-row">
              <span>Cancellation:</span>
              <span class="cancellation-info">
                Will cancel on {{ formatDate(subscription.period_end) }}
              </span>
            </div>
          </div>
          <div class="features-included">
            <h5>Features included:</h5>
            <ul class="feature-list">
              <li v-for="feature in subscription.features" :key="feature">
                <i class="ri-check-line"></i>
                <span>{{ feature }}</span>
              </li>
            </ul>
          </div>
        </div>
      </template>
      
      <template #item-footer>
        <div class="subscription-footer">
          <div class="subscription-actions">
            <button v-if="subscription.status === 'active'" @click="manageSubscription(subscription.id)" class="manage-btn">
              Manage Subscription
            </button>
            <button v-if="subscription.status === 'trialing'" @click="upgradeNow(subscription.id)" class="upgrade-btn">
              Upgrade Now
            </button>
          </div>
        </div>
      </template>
    </codex-order-line>
  </div>
</template>

<script setup>
const subscriptionLines = ref([
  {
    id: 1,
    product: 'Premium Plan',
    tier: 'Pro',
    billing_cycle: 'Monthly',
    final_price: '$0.00',
    ongoing_price: '$29.99/month',
    quantity: 1,
    trial_days: 14,
    status: 'trialing',
    next_billing_date: '2024-02-01',
    features: [
      'Unlimited API calls',
      'Priority support',
      'Advanced analytics',
      '100GB storage'
    ]
  },
  {
    id: 2,
    product: 'Enterprise Plan',
    tier: 'Enterprise',
    billing_cycle: 'Annual',
    final_price: '$299.99',
    ongoing_price: '$299.99/year',
    quantity: 1,
    status: 'active',
    next_billing_date: '2025-01-15',
    cancel_at_period_end: false,
    features: [
      'Everything in Pro',
      'Dedicated support',
      'Custom integrations',
      'Unlimited storage'
    ]
  }
])

const getStatusClass = (status) => {
  const classes = {
    'active': 'status-active',
    'trialing': 'status-trial',
    'canceled': 'status-canceled',
    'past_due': 'status-overdue'
  }
  return classes[status] || 'status-default'
}

const formatDate = (date) => {
  return new Date(date).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}

const manageSubscription = (subscriptionId) => {
  router.push(`/subscription/${subscriptionId}/manage`)
}

const upgradeNow = (subscriptionId) => {
  router.push(`/subscription/${subscriptionId}/upgrade`)
}
</script>
```

### Digital Product Order Lines
```vue
<template>
  <div class="digital-order-lines">
    <codex-order-line
      v-for="(digital, index) in digitalLines"
      :key="digital.id"
      :line="digital"
      :line-key="index"
      :last-index="digitalLines.length - 1"
    >
      <template #item-header>
        <div class="digital-header">
          <div class="product-info">
            <h4 class="product-name">
              {{ digital.product }}
              <span class="digital-badge">Digital</span>
            </h4>
            <div class="license-info">
              <span>{{ digital.license_type }} License</span>
            </div>
          </div>
          <div class="digital-pricing">
            <div class="price-container">
              <span class="final-price">{{ digital.final_price }}</span>
              <span v-if="digital.discount_code" class="discount-applied">
                Discount applied: {{ digital.discount_code }}
              </span>
            </div>
          </div>
        </div>
      </template>
      
      <template #item-content>
        <div class="digital-content">
          <div class="license-details">
            <div class="detail-row">
              <span>License Key:</span>
              <span class="license-key">
                {{ digital.license_key || 'Will be provided after payment' }}
              </span>
            </div>
            <div class="detail-row">
              <span>Valid until:</span>
              <span>{{ digital.expiry_date || 'Lifetime' }}</span>
            </div>
            <div class="detail-row">
              <span>Downloads:</span>
              <span>{{ digital.download_limit || 'Unlimited' }}</span>
            </div>
          </div>
          <div v-if="digital.system_requirements" class="requirements">
            <h5>System Requirements:</h5>
            <ul class="requirements-list">
              <li v-for="requirement in digital.system_requirements" :key="requirement">
                {{ requirement }}
              </li>
            </ul>
          </div>
        </div>
      </template>
      
      <template #item-footer>
        <div class="digital-footer">
          <div v-if="digital.download_links" class="download-section">
            <h5>Download Links:</h5>
            <div class="download-links">
              <a v-for="link in digital.download_links" :key="link.platform" 
                 :href="link.url" 
                 class="download-link"
                 :download="link.filename">
                <i :class="getPlatformIcon(link.platform)"></i>
                <span>{{ link.platform }}</span>
              </a>
            </div>
          </div>
          <div v-if="digital.documentation_url" class="documentation">
            <a :href="digital.documentation_url" target="_blank" class="doc-link">
              <i class="ri-book-line"></i>
              <span>View Documentation</span>
            </a>
          </div>
        </div>
      </template>
    </codex-order-line>
  </div>
</template>

<script setup>
const digitalLines = ref([
  {
    id: 1,
    product: 'Professional Design Software',
    license_type: 'Commercial',
    final_price: '$299.99',
    quantity: 1,
    license_key: 'XXXX-XXXX-XXXX-XXXX',
    expiry_date: 'Dec 31, 2024',
    download_limit: '5 downloads',
    system_requirements: [
      'Windows 10 or macOS 10.15+',
      '8GB RAM minimum',
      '2GB available storage',
      'Graphics card with OpenGL 3.3 support'
    ],
    download_links: [
      {
        platform: 'Windows',
        url: '/downloads/software-windows.exe',
        filename: 'design-software-v2.1-windows.exe'
      },
      {
        platform: 'macOS',
        url: '/downloads/software-macos.dmg',
        filename: 'design-software-v2.1-macos.dmg'
      }
    ],
    documentation_url: '/docs/design-software'
  }
])

const getPlatformIcon = (platform) => {
  const icons = {
    'Windows': 'ri-windows-line',
    'macOS': 'ri-apple-line',
    'Linux': 'ri-ubuntu-line',
    'Android': 'ri-android-line',
    'iOS': 'ri-app-store-line'
  }
  return icons[platform] || 'ri-download-line'
}
</script>
```

### Mobile Order Lines
```vue
<template>
  <div class="mobile-order-lines">
    <codex-order-line
      v-for="(item, index) in mobileLines"
      :key="item.id"
      :line="item"
      :line-key="index"
      :last-index="mobileLines.length - 1"
      class="mobile-line-item"
    >
      <template #item-header>
        <div class="mobile-header">
          <div class="product-image">
            <img :src="item.image" :alt="item.product" class="product-thumb">
          </div>
          <div class="product-info">
            <h4 class="product-name">{{ item.product }}</h4>
            <div class="mobile-price">{{ item.final_price }}</div>
          </div>
          <div class="quantity-pill">
            {{ item.quantity }}x
          </div>
        </div>
      </template>
      
      <template #item-content>
        <div class="mobile-content">
          <div v-if="item.status" class="status-row">
            <span class="status-label">Status:</span>
            <span class="status-value" :class="item.status.toLowerCase()">
              {{ getStatusText(item.status) }}
            </span>
          </div>
          <div v-if="item.tracking_number" class="tracking-row">
            <span class="tracking-label">Tracking:</span>
            <button @click="trackItem(item.tracking_number)" class="track-button">
              {{ item.tracking_number }}
            </button>
          </div>
        </div>
      </template>
      
      <template #item-footer>
        <div class="mobile-footer">
          <div class="mobile-actions">
            <button v-if="item.can_return" @click="returnItem(item.id)" class="mobile-action">
              <i class="ri-arrow-go-back-line"></i>
              <span>Return</span>
            </button>
            <button v-if="item.can_review" @click="reviewItem(item.id)" class="mobile-action">
              <i class="ri-star-line"></i>
              <span>Review</span>
            </button>
            <button @click="reorderItem(item.id)" class="mobile-action">
              <i class="ri-refresh-line"></i>
              <span>Reorder</span>
            </button>
          </div>
        </div>
      </template>
    </codex-order-line>
  </div>
</template>

<script setup>
const mobileLines = ref([
  {
    id: 1,
    product: 'Wireless Earbuds',
    image: '/images/products/earbuds.jpg',
    final_price: '$79.99',
    quantity: 1,
    status: 'delivered',
    tracking_number: 'TRK789012345',
    can_return: true,
    can_review: true
  },
  {
    id: 2,
    product: 'Phone Screen Protector',
    image: '/images/products/screen-protector.jpg',
    final_price: '$12.99',
    quantity: 2,
    status: 'shipped',
    tracking_number: 'TRK789012346',
    can_return: false,
    can_review: false
  }
])

const getStatusText = (status) => {
  const statusTexts = {
    'processing': 'Processing',
    'shipped': 'Shipped',
    'delivered': 'Delivered',
    'returned': 'Returned'
  }
  return statusTexts[status] || status
}

const trackItem = (trackingNumber) => {
  router.push(`/track/${trackingNumber}`)
}

const returnItem = (itemId) => {
  router.push(`/returns/${itemId}`)
}

const reviewItem = (itemId) => {
  router.push(`/review/${itemId}`)
}

const reorderItem = (itemId) => {
  // Add item to cart logic
  addToCart(itemId)
}
</script>

<style scoped>
.mobile-line-item {
  margin-bottom: 16px;
}

.mobile-header {
  display: flex;
  align-items: center;
  gap: 12px;
}

.product-thumb {
  width: 50px;
  height: 50px;
  object-fit: cover;
  border-radius: 8px;
}

.product-info {
  flex: 1;
}

.product-name {
  font-size: 14px;
  margin: 0 0 4px 0;
}

.mobile-price {
  font-weight: bold;
  color: #007bff;
}

.quantity-pill {
  background: #f0f0f0;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
}

.mobile-actions {
  display: flex;
  gap: 8px;
  margin-top: 12px;
}

.mobile-action {
  display: flex;
  align-items: center;
  gap: 4px;
  padding: 8px 16px;
  border: 1px solid #ddd;
  border-radius: 20px;
  background: white;
  font-size: 12px;
  flex: 1;
  justify-content: center;
}

.track-button {
  background: none;
  border: none;
  color: #007bff;
  text-decoration: underline;
  font-size: 12px;
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-cart-item` | Main line item container |
| `_c-divider` | Divider styling between items |
| `_c-item-header` | Header section container |
| `_c-flex` | Flexbox layout |
| `_c-justify-between` | Space between flex items |
| `_c-desc` | Product description styling |
| `_c-text-bold` | Bold text styling |
| `_c-price-container` | Price display container |
| `_c-price` | Price text styling |
| `_c-item-content` | Content section container |
| `_c-order-quantity` | Quantity display styling |
| `_c-ongoing-price` | Ongoing price styling |
| `_c-trial-info` | Trial period information |
| `_c-item-footer` | Footer section container |

## Best Practices

### Line Item Data Structure
- Ensure all required fields (product, final_price, quantity) are present
- Handle both one-time and subscription items appropriately
- Include trial period information for subscription items
- Provide meaningful product names and descriptions

### Subscription Handling
- Display trial periods clearly with day counts
- Show ongoing prices for subscription items
- Handle subscription status appropriately
- Provide clear billing cycle information

### Customization
- Use slots to customize presentation without breaking functionality
- Maintain consistent styling with parent Order component
- Consider responsive design for mobile order viewing
- Implement appropriate spacing and dividers between items

### Loading States
- Show skeleton loading states while data is being fetched
- Provide smooth transitions when data loads
- Handle empty states gracefully
- Consider progressive loading for large order lists

### Accessibility
- Use semantic HTML structure for line item information
- Provide clear labels for screen readers
- Ensure sufficient color contrast for all text
- Make interactive elements keyboard accessible

### Performance
- Optimize for rendering large numbers of line items
- Use efficient list rendering techniques
- Minimize re-renders when line item data changes
- Consider virtualization for very long item lists

## Component Registration
```javascript
// Global registration
app.component('CodexOrderLine', OrderLine)

// Local registration  
import OrderLine from '@/components/Cart/OrderLine.vue'

export default {
  components: {
    CodexOrderLine: OrderLine
  }
}
``` 