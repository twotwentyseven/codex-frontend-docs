# Cart Component

## Overview
The Cart component serves as the main orchestrator for the entire shopping cart and checkout experience. It manages state transitions between cart viewing, checkout, order confirmation, and payment processing. The component provides Stripe integration with customizable appearance, handles 3D Secure authentication, manages cart/order state through centralized composables, and coordinates multiple child components to deliver a complete e-commerce workflow.

## Basic Usage
```vue
<template>
  <div class="shopping-experience">
    <codex-cart
      :announcement-bar="'Free shipping on orders over $50!'"
      :announcement-url="'/shipping-info'"
      @updated="handleCartUpdate"
      @cartLoaded="handleCartLoaded"
      @cartItemAdded="handleItemAdded"
    >
      <template #empty-cart>
        <div class="custom-empty-cart">
          <h3>Your cart is empty</h3>
          <p>Browse our products to get started!</p>
        </div>
      </template>
      
      <template #cart-success-cta>
        <button @click="continueShopping">Continue Shopping</button>
      </template>
    </codex-cart>
  </div>
</template>

<script setup>
const handleCartUpdate = () => {
  console.log('Cart was updated')
}

const handleCartLoaded = () => {
  console.log('Cart loaded successfully')
}

const handleItemAdded = () => {
  console.log('Item added to cart')
}

const continueShopping = () => {
  window.location.href = '/products'
}
</script>
```

## Key Features
- Complete cart-to-order state management
- Stripe payment integration with customizable themes and colors
- 3D Secure (SCA) authentication support
- Centralized payment status handling and error management
- Multi-component orchestration (cart contents, checkout, order display)
- URL-based order retrieval and payment intent handling
- Customer authentication state management
- Configurable Stripe appearance and layout options
- Real-time cart size tracking and event emission
- Announcement bar integration for promotions

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `announcementBar` | `String` | `''` | Promotional text displayed at top of cart |
| `announcementUrl` | `String` | `''` | URL for announcement bar link |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `open` | `none` | Emitted when cart is opened |
| `close` | `none` | Emitted when cart is closed |
| `updated` | `none` | Emitted when cart contents change |
| `cartLoaded` | `cart` | Emitted when cart data is loaded |
| `cartItemAdded` | `item` | Emitted when item is added to cart |
| `cartItemRemoved` | `item` | Emitted when item is removed from cart |
| `cartItemIncremented` | `item` | Emitted when item quantity increases |
| `cartItemDecremented` | `item` | Emitted when item quantity decreases |
| `cartVoucherAdded` | `voucher` | Emitted when voucher is applied |
| `cartVoucherCleared` | `none` | Emitted when voucher is removed |

## Slots

| Slot | Description |
|------|-------------|
| `empty-cart` | Custom content when cart is empty |
| `cart-success-cta` | Call-to-action buttons after successful order |

## States

### Cart View State
```vue
<!-- Default state showing cart contents -->
<codex-cart />
```

### Checkout State
```vue
<!-- Automatically transitions to checkout when checkout button clicked -->
<codex-cart />
```

### Order Confirmation State
```vue
<!-- Displays order details after successful payment -->
<codex-cart />
```

### Processing State
```vue
<!-- Shows processing indicator during payment -->
<codex-cart />
```

### 3D Secure Authentication State
```vue
<!-- Handles Stripe SCA when required -->
<codex-cart />
```

## Stripe Configuration

The component automatically configures Stripe based on `window.codex.stripe` settings:

### Default Stripe Configuration
```javascript
window.codex = {
  stripe: {
    theme: 'stripe',
    locale: 'auto',
    fontFamily: '"Gill Sans", system-ui, sans-serif',
    colors: {
      primary: '#0570de',
      background: '#ffffff',
      text: '#000000',
      error: '#d93025',
      textSecondary: '#6b7280',
      backgroundSecondary: '#f9fafb'
    },
    layout: {
      type: 'accordion',
      defaultCollapsed: false,
      radios: false,
      spacedAccordionItems: true
    }
  }
}
```

### Custom Stripe Styling
```javascript
window.codex = {
  stripe: {
    theme: 'night',
    colors: {
      primary: '#ff6b35',
      background: '#1a1a1a',
      text: '#ffffff'
    },
    rules: {
      '.Input': {
        padding: '16px',
        borderRadius: '8px'
      },
      '.Tab': {
        border: '2px solid transparent'
      }
    }
  }
}
```

## Examples

### Basic E-commerce Cart
```vue
<template>
  <div class="store-cart">
    <codex-cart
      @updated="trackCartChange"
      @cartLoaded="initializeAnalytics"
    >
      <template #empty-cart>
        <div class="empty-state">
          <i class="ri-shopping-cart-line"></i>
          <h3>Your cart is empty</h3>
          <p>Add some products to get started!</p>
          <a href="/products" class="shop-now-btn">Shop Now</a>
        </div>
      </template>
    </codex-cart>
  </div>
</template>

<script setup>
const trackCartChange = () => {
  // Google Analytics tracking
  gtag('event', 'cart_updated', {
    event_category: 'ecommerce'
  })
}

const initializeAnalytics = () => {
  console.log('Cart loaded - analytics ready')
}
</script>
```

### Subscription Service Cart
```vue
<template>
  <div class="subscription-cart">
    <codex-cart
      :announcement-bar="subscriptionPromo"
      :announcement-url="'/pricing'"
      @cartItemAdded="handleSubscriptionAdded"
      @updated="calculateSubscriptionTotal"
    >
      <template #cart-success-cta>
        <div class="subscription-success">
          <button @click="goToDashboard" class="primary-btn">
            Go to Dashboard
          </button>
          <button @click="downloadApp" class="secondary-btn">
            Download App
          </button>
        </div>
      </template>
    </codex-cart>
  </div>
</template>

<script setup>
const subscriptionPromo = ref('Save 20% on annual plans!')

const handleSubscriptionAdded = () => {
  // Track subscription signup
  analytics.track('subscription_started')
}

const calculateSubscriptionTotal = () => {
  // Update subscription analytics
  updateSubscriptionMetrics()
}

const goToDashboard = () => {
  window.location.href = '/dashboard'
}

const downloadApp = () => {
  window.open('/download-app', '_blank')
}
</script>
```

### International Commerce Setup
```vue
<template>
  <div class="international-cart">
    <codex-cart
      :announcement-bar="localizedPromo"
      @updated="updateCurrencyDisplay"
      @cartLoaded="setRegionalSettings"
    />
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'

const { locale } = useI18n()

const localizedPromo = computed(() => {
  const promos = {
    'en': 'Free international shipping!',
    'es': '¡Envío internacional gratuito!',
    'fr': 'Livraison internationale gratuite!'
  }
  return promos[locale.value] || promos['en']
})

const updateCurrencyDisplay = () => {
  // Update currency based on region
  setCurrencyByRegion()
}

const setRegionalSettings = () => {
  // Configure Stripe for region
  configureRegionalStripe()
}

// Configure Stripe for international use
onMounted(() => {
  window.codex.stripe = {
    ...window.codex.stripe,
    locale: locale.value,
    layout: {
      type: 'accordion',
      defaultCollapsed: false
    }
  }
})
</script>
```

### High-Value Purchase Cart
```vue
<template>
  <div class="premium-cart">
    <codex-cart
      :announcement-bar="'White-glove service included'"
      :announcement-url="'/premium-service'"
      @cartItemAdded="validatePremiumItem"
      @updated="checkCartValue"
    >
      <template #empty-cart>
        <div class="premium-empty">
          <h3>Premium Collection</h3>
          <p>Curated for discerning customers</p>
          <button @click="requestConsultation">
            Request Personal Consultation
          </button>
        </div>
      </template>
      
      <template #cart-success-cta>
        <div class="premium-success">
          <h4>Thank you for your premium purchase!</h4>
          <p>A specialist will contact you within 24 hours.</p>
          <button @click="scheduleDelivery">
            Schedule White-Glove Delivery
          </button>
        </div>
      </template>
    </codex-cart>
  </div>
</template>

<script setup>
const validatePremiumItem = () => {
  // Verify customer eligibility for premium items
  verifyPremiumAccess()
}

const checkCartValue = () => {
  // Trigger premium services for high-value carts
  const cartValue = calculateCartValue()
  if (cartValue > 10000) {
    enablePremiumServices()
  }
}

const requestConsultation = () => {
  // Open consultation booking modal
  openConsultationModal()
}

const scheduleDelivery = () => {
  // Navigate to delivery scheduling
  window.location.href = '/schedule-delivery'
}
</script>
```

### Mobile-Optimized Cart
```vue
<template>
  <div class="mobile-cart" :class="{ 'mobile-view': isMobile }">
    <codex-cart
      @open="handleMobileOpen"
      @close="handleMobileClose"
      @updated="optimizeForMobile"
    >
      <template #empty-cart>
        <div class="mobile-empty">
          <div class="mobile-icon">🛒</div>
          <h3>Cart is empty</h3>
          <button @click="continueBrowsing" class="mobile-cta">
            Continue Browsing
          </button>
        </div>
      </template>
    </codex-cart>
  </div>
</template>

<script setup>
import { useBreakpoints } from '@vueuse/core'

const breakpoints = useBreakpoints({
  mobile: 640,
  tablet: 768,
  desktop: 1024
})

const isMobile = breakpoints.smaller('tablet')

const handleMobileOpen = () => {
  if (isMobile.value) {
    // Lock body scroll on mobile
    document.body.style.overflow = 'hidden'
  }
}

const handleMobileClose = () => {
  if (isMobile.value) {
    // Restore body scroll
    document.body.style.overflow = 'auto'
  }
}

const optimizeForMobile = () => {
  if (isMobile.value) {
    // Optimize layout for mobile
    optimizeMobileLayout()
  }
}

const continueBrowsing = () => {
  window.history.back()
}

// Configure mobile-friendly Stripe
onMounted(() => {
  if (isMobile.value) {
    window.codex.stripe = {
      ...window.codex.stripe,
      layout: {
        type: 'tabs',
        defaultCollapsed: true
      }
    }
  }
})
</script>
```

## Translation Support

The Cart component supports comprehensive internationalization through Vue i18n integration. All user-facing text is translatable and the component includes extensive translation patterns for various cart states and interactions.

### Translation Keys

#### Cart Interface
- `cart.cart` - Cart title/header
- `cart.empty_basket` - Empty cart message
- `cart.subtotal` - Subtotal label
- `cart.discount` - Discount label  
- `cart.total` - Total label
- `cart.including` - "Including" text for taxes
- `cart.in_taxes` - Tax inclusion suffix
- `cart.buy_now` - Buy now button (empty cart)
- `cart.book_now` - Book now button (empty cart)
- `cart.login` - Login button
- `cart.register` - Register button
- `cart.checkout` - Checkout button
- `cart.checking_out` - Processing checkout text

#### Payment Processing
- `cart.processing` - Generic processing state
- `cart.payment_processing` - Payment processing message
- `cart.wait_while_confirm_payment` - Payment confirmation wait message
- `cart.payment_failed` - Payment failure message
- `cart.payment_failed_choose_alt` - Payment failure instruction
- `cart.nothing_to_pay` - Free payment button text
- `cart.pay_using_gift_card` - Gift card payment text (with parameter)

#### Cart Summary & Navigation
- `cart.hide_cart_summary` - Hide summary button
- `cart.show_cart_summary` - Show summary button

#### Order Status
- `cart.order_fully_paid` - Fully paid order status
- `cart.order_refunded` - Refunded order status
- `cart.order_partially_refunded` - Partially refunded status
- `cart.payment_pending` - Payment pending status
- `cart.order_cancelled_title` - Cancelled order title
- `cart.order_cancelled_description` - Cancelled order description

#### Success States
- `cart.transaction_successfull` - Transaction success title
- `cart.transaction_congratulation` - Success congratulations message
- `cart.bank_requested_confirmation` - Bank confirmation request
- `cart.bank_requested_confirmation_more_info` - Additional bank confirmation info

#### Error Handling
- `cart.fail_close_order` - Close failed order button
- `cart.close_and_clear_cart` - Close and clear cart button
- `cart.closing` - Closing process text
- `cart.error` - Generic error state

### Template Usage Examples

#### Basic Cart Interface
```vue
<template>
  <div class="_c-title">{{ $t('cart.cart') }}</div>
  <div v-if="cart.lines.length === 0">
    {{ t('cart.empty_basket') }}
  </div>
  <div class="_c-subtotal">
    {{ t('cart.subtotal') }}: {{ formatCurrency(subtotal) }}
  </div>
</template>
```

#### Payment States
```vue
<template>
  <!-- Processing state -->
  <codex-button 
    :processing-text="t('cart.processing')"
    :default-text="t('cart.checkout')"
    :processing="loading"
  />
  
  <!-- Success state -->
  <div v-if="success">
    <h3>{{ $t('cart.transaction_successfull') }}</h3>
    <p>{{ $t('cart.transaction_congratulation') }}</p>
  </div>
  
  <!-- Error state -->
  <div v-if="error">
    <h3>{{ $t('cart.payment_failed') }}</h3>
    <p>{{ $t('cart.payment_failed_choose_alt') }}</p>
  </div>
</template>
```

#### Order Status Display
```vue
<template>
  <div class="_c-order-status">
    <h2 v-if="order.status === 'Paid'">
      {{ $t('cart.order_fully_paid') }}
    </h2>
    <h2 v-else-if="order.status === 'Cancelled'">
      {{ $t('cart.order_cancelled_title') }}
    </h2>
    <p v-if="order.status === 'Cancelled'">
      {{ $t('cart.order_cancelled_description') }}
    </p>
  </div>
</template>
```

#### Gift Card Payment
```vue
<template>
  <codex-button 
    v-if="canPayWithGiftCard"
    :default-text="$t('cart.pay_using_gift_card', { name: giftCard.name })"
    @click="payWithGiftCard"
  />
</template>
```

#### Cart Summary Toggle
```vue
<template>
  <button @click="toggleSummary">
    <span v-if="showSummary">{{ $t('cart.hide_cart_summary') }}</span>
    <span v-else>{{ $t('cart.show_cart_summary') }}</span>
  </button>
</template>
```

#### Authentication States
```vue
<template>
  <div v-if="!customer" class="_c-auth-buttons">
    <button>{{ $t('cart.login') }}</button>
    <button>{{ $t('cart.register') }}</button>
  </div>
</template>
```

### Integration with useI18n

The component uses the Vue i18n composition API:

```javascript
import { useI18n } from 'vue-i18n'

const { t } = useI18n()

// Usage in computed properties
const checkoutButtonText = computed(() => 
  loading.value ? t('cart.checking_out') : t('cart.checkout')
)

// Usage in methods
const showSuccessMessage = () => {
  toast.success(t('cart.transaction_successfull'))
}
```

### Parameter Support

Some translation keys support dynamic parameters:

```javascript
// Gift card payment with name parameter
$t('cart.pay_using_gift_card', { name: giftCard.value.name })

// Tax information with amount
$t('cart.including') + ' ' + cart.tax + ' ' + $t('cart.in_taxes')
```

## URL Parameters

The component automatically handles these URL parameters:

| Parameter | Description |
|-----------|-------------|
| `order_id` | Loads specific order by ID |
| `payment_intent` | Loads order from Stripe payment intent |
| `stripe_id` | Triggers customer authentication flow |
| `payment_intent_client_secret` | Handles 3D Secure returns |

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-cart` | Main cart container |
| `codex` | Base component styling |

## Best Practices

### State Management
- Let the component handle state transitions automatically
- Use centralized payment status provider for consistency
- Avoid manual state manipulation outside the component
- Monitor cart events for analytics and user experience improvements

### Stripe Integration
- Configure Stripe appearance globally via `window.codex.stripe`
- Test payment flows in both sandbox and production environments
- Handle 3D Secure authentication gracefully
- Customize colors and fonts to match your brand

### Error Handling
- Implement proper error boundaries around the cart component
- Monitor payment failures and provide clear user feedback
- Log payment errors for debugging and improvement
- Provide fallback options when payment methods fail

### Performance
- Load cart component lazily if not immediately needed
- Optimize Stripe configuration for faster loading
- Monitor cart loading times and optimize as needed
- Use proper loading states during transitions

### Accessibility
- Ensure cart announcements are accessible to screen readers
- Provide keyboard navigation for all cart interactions
- Use semantic HTML structure within slot content
- Test with assistive technologies

### Mobile Experience
- Configure mobile-friendly Stripe layouts
- Optimize touch interactions for cart management
- Consider mobile-specific payment methods
- Test thoroughly on various mobile devices

### Security
- Validate all cart operations server-side
- Use HTTPS for all payment-related operations
- Implement proper CSRF protection
- Monitor for suspicious cart activity

## Component Registration
```javascript
// Global registration
app.component('CodexCart', Cart)

// Local registration  
import Cart from '@/components/Cart/Cart.vue'

export default {
  components: {
    CodexCart: Cart
  }
}
```

## Internationalization

The Cart component itself has minimal translation requirements as it primarily orchestrates other components. However, it may use the following translation keys for state management and user messaging:

### State Management Messages
| Key | Usage |
|-----|-------|
| `cart.loading` | Loading state message |
| `cart.processing` | Processing payment message |
| `cart.error` | General error message |
| `cart.success` | Success state message |

### Payment Status Messages  
| Key | Usage |
|-----|-------|
| `payment.processing` | Payment processing indicator |
| `payment.authenticating` | 3D Secure authentication message |
| `payment.failed` | Payment failed message |
| `payment.succeeded` | Payment successful message |

### Error Messages
| Key | Usage |
|-----|-------|
| `error.connection_failed` | Network connection error |
| `error.payment_intent_failed` | Payment intent creation error |
| `error.unexpected_error` | Generic unexpected error |

### Translation Usage Examples
```vue
<!-- The Cart component primarily relies on child components for translations -->
<!-- Child components handle their own internationalization: -->

<!-- CartContents handles cart-specific translations -->
<codex-cart-contents 
  :cart="cart"
  @cartUpdated="cartUpdated"
/>

<!-- Checkout handles checkout-specific translations -->
<codex-checkout 
  @backToCart="checkout = false"
  @cartUpdated="cartUpdated"
/>

<!-- Order handles order-specific translations -->
<codex-order 
  :order="order"
  :cart="cart"
/>
```

### Stripe Integration Translation Notes
The Cart component configures Stripe with localization support:
- Stripe locale is automatically set from `window.codex.locale` or defaults to 'auto'
- Payment method names are localized by Stripe based on the locale setting
- Currency formatting follows the locale configuration 