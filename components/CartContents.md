# CartContents Component

## Overview
The CartContents component renders the detailed view of cart items with comprehensive pricing displays, voucher management, and checkout controls. It features cart line item management, dynamic pricing calculations with discounts and ongoing charges, customer authentication state handling, announcement bar integration, and customizable empty cart states with login/register prompts.

## Basic Usage
```vue
<template>
  <div class="cart-display">
    <codex-cart-contents
      :cart="cartData"
      :announcement-bar="'Free shipping on orders over $50!'"
      :announcement-url="'/shipping-info'"
      @cartUpdated="handleCartUpdate"
      @checkout="proceedToCheckout"
    >
      <template #header>
        <h2>Your Shopping Cart</h2>
      </template>
      
      <template #empty-cart>
        <div class="custom-empty-cart">
          <h3>Your cart is empty</h3>
          <p>Start shopping to add items!</p>
          <a href="/products" class="shop-btn">Browse Products</a>
        </div>
      </template>
      
      <template #login-register>
        <div class="auth-prompt">
          <p>Please sign in to continue</p>
          <button @click="openLogin">Sign In</button>
          <button @click="openRegister">Create Account</button>
        </div>
      </template>
    </codex-cart-contents>
  </div>
</template>

<script setup>
const cartData = ref({
  lines: [],
  price_raw: 0,
  base_price_raw: 0,
  // ... other cart properties
})

const handleCartUpdate = () => {
  console.log('Cart was updated')
}

const proceedToCheckout = () => {
  console.log('Proceeding to checkout')
}

const openLogin = () => {
  // Open login modal
}

const openRegister = () => {
  // Open register modal
}
</script>
```

## Key Features
- Comprehensive cart line item display with quantity controls
- Dynamic pricing calculations with join fees and ongoing charges
- Voucher/discount code management and display
- Gift card and payment method integration
- Tax calculation and display
- Customer authentication state handling
- Announcement bar for promotions
- Customizable empty cart states
- Login/register prompts for unauthenticated users
- Responsive checkout button with loading states

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `cart` | `Object` | `required` | Cart data object with lines and pricing |
| `announcementBar` | `String` | `''` | Promotional text displayed at top |
| `announcementUrl` | `String` | `''` | URL for announcement bar link |

## Common Props

| Prop Name | Usage |
|-----------|-------|
| `formatCurrency` | Formats monetary values according to locale |

## Common Functions

| Function | Usage |
|----------|-------|
| `formatCurrency` | Used to display all price values in correct currency format |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `cartUpdated` | `none` | Emitted when cart contents change |
| `cartFinalized` | `none` | Emitted when cart is finalized |
| `showLoader` | `none` | Emitted to show loading state |
| `hideLoader` | `none` | Emitted to hide loading state |
| `checkout` | `none` | Emitted when checkout button is clicked |

## Slots

| Slot | Description |
|------|-------------|
| `header` | Custom header content above cart items |
| `content` | Complete custom content area |
| `footer` | Custom footer content below cart items |
| `empty-cart` | Custom content when cart is empty |
| `login-register` | Custom authentication prompts |

## States

### Cart with Items State
```vue
<codex-cart-contents 
  :cart="populatedCart"
  @checkout="proceedToCheckout"
/>
```

### Empty Cart State
```vue
<codex-cart-contents 
  :cart="emptyCart"
>
  <template #empty-cart>
    <div class="empty-cart-message">
      <p>Your cart is empty</p>
    </div>
  </template>
</codex-cart-contents>
```

### Unauthenticated User State
```vue
<codex-cart-contents 
  :cart="cart"
>
  <template #login-register>
    <div class="auth-required">
      <button @click="login">Login</button>
      <button @click="register">Register</button>
    </div>
  </template>
</codex-cart-contents>
```

### Loading State
```vue
<codex-cart-contents 
  :cart="cart"
  :loading="true"
/>
```

## Internationalization

The component uses the following translation keys:

### Core Cart Interface
| Key | Usage |
|-----|-------|
| `cart.title` | Main cart header title |
| `cart.shopping_cart` | Alternative cart title |
| `cart.empty_basket` | Empty cart message |

### Cart Summary and Pricing
| Key | Usage |
|-----|-------|
| `cart.subtotal` | Subtotal line label |
| `cart.discount` | Discount line label |
| `cart.total` | Total price label |
| `cart.including` | Tax inclusion prefix ("Including") |
| `cart.in_taxes` | Tax suffix ("in taxes") |

### Empty Cart Actions
| Key | Usage |
|-----|-------|
| `cart.buy_now` | Buy now button for empty cart |
| `cart.book_now` | Book now button for empty cart |

### Authentication Prompts
| Key | Usage |
|-----|-------|
| `cart.login` | Login button text |
| `cart.register` | Register button text |

### Checkout Actions
| Key | Usage |
|-----|-------|
| `cart.checkout` | Checkout button text |
| `cart.checking_out` | Checkout processing text |

### Translation Usage Examples
```vue
<!-- Cart header with item count -->
<div class="cart-title">
  {{ $t('cart.cart') }} 
  <span class="cart-count">(<codex-cart-counter></codex-cart-counter>)</span>
</div>

<!-- Empty cart state -->
<template v-if="cart?.lines?.length === 0">
  <codex-title :content="t('cart.empty_basket')" />
  <div class="empty-cart-actions">
    <a :href="window.codex.urls.buy_url" class="btn">
      {{ $t('cart.buy_now') }}
    </a>
    <a :href="window.codex.urls.booking_url" class="btn">
      {{ $t('cart.book_now') }}
    </a>
  </div>
</template>

<!-- Pricing summary -->
<div class="subtotal">
  <div>{{ t('cart.subtotal') }}</div>
  <div>{{ formatCurrency(combinedOriginalTotal) }}</div>
</div>

<div class="discount" v-if="cart.voucher">
  <div>{{ t('cart.discount') }}</div>
  <div>-{{ formatCurrency(combinedDiscountTotal) }}</div>
</div>

<div class="total">
  <div>{{ t('cart.total') }}</div>
  <div>{{ formatCurrency(combinedTotal) }}</div>
</div>

<div class="taxes">
  {{ t('cart.including') }} {{ cart.tax }} {{ t('cart.in_taxes') }}
</div>

<!-- Authentication prompts for guests -->
<template v-if="!customer">
  <button data-codex-modal-toggle="codex-login">
    {{ $t('cart.login') }}
  </button>
  <button data-codex-modal-toggle="codex-register">
    {{ $t('cart.register') }}
  </button>
</template>

<!-- Checkout button -->
<codex-button 
  :processingText="t('cart.checking_out')"
  :defaultText="t('cart.checkout')"
  @click="goToCheckout"
/>
```

## Examples

### Basic E-commerce Cart
```vue
<template>
  <div class="product-cart">
    <codex-cart-contents
      :cart="cart"
      :announcement-bar="seasonalPromo"
      :announcement-url="'/sale'"
      @cartUpdated="trackCartChange"
      @checkout="proceedToCheckout"
    >
      <template #header>
        <div class="cart-header">
          <h2>Shopping Cart</h2>
          <p>Review your items before checkout</p>
        </div>
      </template>
      
      <template #empty-cart>
        <div class="empty-cart-state">
          <i class="ri-shopping-cart-2-line"></i>
          <h3>Your cart is empty</h3>
          <p>Browse our collection and add items to your cart</p>
          <div class="empty-cart-actions">
            <a href="/products" class="btn primary">Shop Products</a>
            <a href="/categories" class="btn secondary">Browse Categories</a>
          </div>
        </div>
      </template>
    </codex-cart-contents>
  </div>
</template>

<script setup>
const cart = ref({
  lines: [],
  price_raw: 0,
  base_price_raw: 0,
  tax: '$0.00',
  voucher: null
})

const seasonalPromo = ref('Summer Sale: 20% off everything!')

const trackCartChange = () => {
  // Analytics tracking
  gtag('event', 'cart_updated', {
    event_category: 'ecommerce',
    event_label: 'cart_contents'
  })
}

const proceedToCheckout = () => {
  // Navigate to checkout
  router.push('/checkout')
}
</script>
```

### Subscription Service Cart
```vue
<template>
  <div class="subscription-cart">
    <codex-cart-contents
      :cart="subscriptionCart"
      @cartUpdated="updateSubscriptionMetrics"
      @checkout="startSubscriptionFlow"
    >
      <template #header>
        <div class="subscription-header">
          <h2>Your Subscription</h2>
          <div class="billing-info">
            <i class="ri-calendar-line"></i>
            <span>Next billing: {{ nextBillingDate }}</span>
          </div>
        </div>
      </template>
      
      <template #empty-cart>
        <div class="no-subscription">
          <h3>No active subscription</h3>
          <p>Choose a plan to get started</p>
          <div class="plan-options">
            <button @click="selectPlan('basic')" class="plan-btn">
              Basic Plan - $9.99/month
            </button>
            <button @click="selectPlan('premium')" class="plan-btn">
              Premium Plan - $19.99/month
            </button>
          </div>
        </div>
      </template>
    </codex-cart-contents>
  </div>
</template>

<script setup>
const subscriptionCart = ref({
  lines: [
    {
      name: 'Premium Subscription',
      line_price: '$19.99',
      line_ongoing_price: '$19.99/month',
      line_ongoing_description: 'per month',
      properties: {
        payment_starts_at: '2024-02-01'
      }
    }
  ],
  ongoing_price_raw: 1999
})

const nextBillingDate = ref('February 1, 2024')

const updateSubscriptionMetrics = () => {
  // Update subscription analytics
  trackSubscriptionChange()
}

const startSubscriptionFlow = () => {
  // Handle subscription checkout
  processSubscription()
}

const selectPlan = (planType) => {
  // Add plan to cart
  addPlanToCart(planType)
}
</script>
```

### Multi-Currency International Cart
```vue
<template>
  <div class="international-cart">
    <codex-cart-contents
      :cart="localizedCart"
      :announcement-bar="localizedPromo"
      @cartUpdated="updateCurrencyDisplay"
      @checkout="processInternationalCheckout"
    >
      <template #header>
        <div class="international-header">
          <h2>{{ $t('cart.shopping_cart') }}</h2>
          <div class="currency-selector">
            <select v-model="selectedCurrency" @change="updateCurrency">
              <option value="USD">USD ($)</option>
              <option value="EUR">EUR (€)</option>
              <option value="GBP">GBP (£)</option>
            </select>
          </div>
        </div>
      </template>
      
      <template #footer>
        <div class="international-footer">
          <div class="shipping-info">
            <p>Ships to: {{ shippingCountry }}</p>
            <p>Estimated delivery: {{ estimatedDelivery }}</p>
          </div>
        </div>
      </template>
    </codex-cart-contents>
  </div>
</template>

<script setup>
const selectedCurrency = ref('USD')
const shippingCountry = ref('United States')
const estimatedDelivery = ref('3-5 business days')

const localizedCart = computed(() => ({
  ...cart.value,
  price_raw: convertCurrency(cart.value.price_raw, selectedCurrency.value),
  base_price_raw: convertCurrency(cart.value.base_price_raw, selectedCurrency.value)
}))

const localizedPromo = computed(() => {
  const promos = {
    'USD': 'Free shipping over $75!',
    'EUR': 'Free shipping over €65!',
    'GBP': 'Free shipping over £60!'
  }
  return promos[selectedCurrency.value]
})

const updateCurrency = () => {
  // Update all prices in cart
  recalculateCartPrices()
}

const updateCurrencyDisplay = () => {
  // Refresh currency display
  refreshPriceDisplay()
}

const processInternationalCheckout = () => {
  // Handle international checkout
  processCheckoutWithCurrency(selectedCurrency.value)
}
</script>
```

### Gift Card and Voucher Cart
```vue
<template>
  <div class="voucher-cart">
    <codex-cart-contents
      :cart="cartWithVouchers"
      @cartUpdated="trackVoucherUsage"
      @checkout="validateVouchersAndCheckout"
    >
      <template #content>
        <!-- Custom content with voucher highlights -->
        <div class="voucher-highlights" v-if="hasActiveVouchers">
          <div class="savings-badge">
            <i class="ri-price-tag-3-line"></i>
            <span>You're saving {{ totalSavings }}!</span>
          </div>
        </div>
        
        <!-- Default cart content -->
        <slot />
        
        <div class="voucher-suggestions" v-if="!hasActiveVouchers">
          <h4>Available Offers</h4>
          <div class="voucher-list">
            <div v-for="voucher in availableVouchers" :key="voucher.code" class="voucher-suggestion">
              <span>{{ voucher.code }} - {{ voucher.description }}</span>
              <button @click="applyVoucher(voucher.code)">Apply</button>
            </div>
          </div>
        </div>
      </template>
    </codex-cart-contents>
  </div>
</template>

<script setup>
const cartWithVouchers = ref({
  lines: [],
  voucher: {
    code: 'SAVE20',
    amount: '$15.00'
  },
  payment_methods: {
    giftcard: {
      name: 'Gift Card',
      paymentSource: {
        last4: '1234',
        balance: '$25.00'
      }
    }
  },
  discount_raw: 1500
})

const hasActiveVouchers = computed(() => 
  cartWithVouchers.value.voucher || 
  Object.keys(cartWithVouchers.value.payment_methods).length > 0
)

const totalSavings = computed(() => {
  let savings = 0
  if (cartWithVouchers.value.discount_raw) {
    savings += cartWithVouchers.value.discount_raw
  }
  Object.values(cartWithVouchers.value.payment_methods).forEach(method => {
    if (method.paymentSource?.balance) {
      savings += parseFloat(method.paymentSource.balance.replace('$', '')) * 100
    }
  })
  return formatCurrency(savings)
})

const availableVouchers = ref([
  { code: 'WELCOME10', description: '10% off first order' },
  { code: 'SHIP FREE', description: 'Free shipping' }
])

const trackVoucherUsage = () => {
  // Track voucher application
  analytics.track('voucher_applied')
}

const applyVoucher = (code) => {
  // Apply voucher to cart
  applyVoucherCode(code)
}

const validateVouchersAndCheckout = () => {
  // Validate all vouchers before checkout
  validateAndProceed()
}
</script>
```

### Mobile-Optimized Cart
```vue
<template>
  <div class="mobile-cart" :class="{ 'mobile-view': isMobile }">
    <codex-cart-contents
      :cart="cart"
      @cartUpdated="optimizeForMobile"
      @checkout="mobileCheckout"
    >
      <template #header>
        <div class="mobile-header">
          <button @click="closeCart" class="close-btn">
            <i class="ri-close-line"></i>
          </button>
          <h2>Cart ({{ cartSize }})</h2>
        </div>
      </template>
      
      <template #empty-cart>
        <div class="mobile-empty">
          <div class="empty-icon">🛒</div>
          <h3>Your cart is empty</h3>
          <button @click="continueShopping" class="mobile-continue-btn">
            Continue Shopping
          </button>
        </div>
      </template>
      
      <template #footer>
        <div class="mobile-footer">
          <div class="price-summary">
            <span>Total: {{ cart.price }}</span>
          </div>
        </div>
      </template>
    </codex-cart-contents>
  </div>
</template>

<script setup>
import { useBreakpoints } from '@vueuse/core'

const breakpoints = useBreakpoints({
  mobile: 640,
  tablet: 768
})

const isMobile = breakpoints.smaller('tablet')

const optimizeForMobile = () => {
  if (isMobile.value) {
    // Scroll to top on mobile when cart updates
    scrollToTop()
  }
}

const mobileCheckout = () => {
  if (isMobile.value) {
    // Mobile-specific checkout flow
    openMobileCheckout()
  } else {
    // Standard checkout
    proceedToCheckout()
  }
}

const closeCart = () => {
  // Close mobile cart
  emit('close')
}

const continueShopping = () => {
  // Return to previous page
  window.history.back()
}
</script>

<style scoped>
.mobile-view {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: white;
  z-index: 1000;
}

.mobile-header {
  display: flex;
  align-items: center;
  padding: 1rem;
  border-bottom: 1px solid #eee;
}

.close-btn {
  margin-right: 1rem;
  background: none;
  border: none;
  font-size: 1.5rem;
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-cart-editor` | Main cart contents container |
| `codex` | Base component styling |
| `_c-header` | Cart header section |
| `_c-title` | Cart title styling |
| `_c-cart-count` | Cart item counter |
| `_c-content` | Main content area |
| `_c-announcement-bar` | Promotional announcement bar |
| `_c-line-items` | Cart items container |
| `_c-footer` | Cart footer section |
| `_c-sub-total` | Subtotal display section |
| `_c-price-container` | Price display container |
| `_c-discount-container` | Discount information |
| `_c-voucher` | Voucher code display |
| `_c-total` | Total price section |
| `_c-giftcard-container` | Gift card display |
| `_c-taxes` | Tax information |
| `_c-price-ongoing` | Recurring price display |
| `_c-btn-container` | Button container |
| `_c-btn-checkout` | Checkout button |

## Best Practices

### Cart Management
- Always provide loading states during cart operations
- Implement optimistic updates for better user experience
- Handle cart synchronization across multiple tabs/devices
- Provide clear feedback when cart operations fail

### Pricing Display
- Show original prices when discounts are applied
- Clearly separate one-time fees from recurring charges
- Display tax information according to regional requirements
- Use consistent currency formatting throughout

### User Experience
- Provide multiple empty cart call-to-action options
- Show clear authentication prompts for guest users
- Display cart count and total prominently
- Optimize for mobile with touch-friendly controls

### Voucher and Discount Handling
- Validate vouchers in real-time
- Show clear error messages for invalid vouchers
- Display voucher savings prominently
- Allow easy removal of applied vouchers

### Performance
- Lazy load cart contents when not immediately visible
- Debounce quantity changes to prevent excessive API calls
- Cache cart data appropriately
- Optimize for mobile network conditions

### Accessibility
- Use semantic HTML for price structures
- Provide screen reader announcements for cart changes
- Ensure keyboard navigation for all cart controls
- Use appropriate ARIA labels for complex pricing

### Security
- Validate all cart operations server-side
- Protect against price manipulation
- Implement rate limiting for cart updates
- Sanitize all user input in voucher codes

## Component Registration
```javascript
// Global registration
app.component('CodexCartContents', CartContents)

// Local registration  
import CartContents from '@/components/Cart/CartContents.vue'

export default {
  components: {
    CodexCartContents: CartContents
  }
}
``` 