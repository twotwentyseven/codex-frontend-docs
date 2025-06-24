# Checkout Component

## Overview
The Checkout component orchestrates the complete checkout process including cart review, payment method selection, and order finalization. It features collapsible cart summaries, integrated payment handling via Stripe, support for gift cards and free checkout scenarios, customer authentication integration, and comprehensive pricing displays with discounts and ongoing charges. The component manages the entire checkout flow from cart review to payment processing.

## Basic Usage
```vue
<template>
  <div class="checkout-process">
    <codex-checkout
      :cart="cartData"
      @cartUpdated="handleCartUpdate"
      @backToCart="returnToCart"
      @cartFinalized="handleOrderComplete"
    >
      <template #header>
        <div class="custom-checkout-header">
          <h2>Secure Checkout</h2>
          <div class="security-badges">
            <span>🔒 SSL Encrypted</span>
            <span>✅ PCI Compliant</span>
          </div>
        </div>
      </template>
      
      <template #footer>
        <div class="checkout-footer">
          <p>Need help? <a href="/contact">Contact support</a></p>
        </div>
      </template>
    </codex-checkout>
  </div>
</template>

<script setup>
const cartData = ref({
  lines: [
    {
      hash: 'line_123',
      name: 'Premium Plan',
      quantity: 1,
      line_price: '$29.99',
      line_ongoing_price: '$29.99/month',
      buyable_type: 'Plan'
    }
  ],
  price_raw: 2999,
  base_price_raw: 3999,
  discount_raw: 1000,
  tax: '$2.40',
  voucher: {
    code: 'SAVE10',
    amount: '$10.00'
  }
})

const handleCartUpdate = () => {
  console.log('Cart was updated during checkout')
}

const returnToCart = () => {
  console.log('User wants to return to cart')
  // Navigate back to cart view
}

const handleOrderComplete = () => {
  console.log('Order completed successfully')
  // Navigate to success page
}
</script>
```

## Key Features
- Collapsible cart summary with detailed pricing breakdown
- Integrated Stripe payment method selection and processing
- Support for gift card payments and store credit
- Free checkout handling for zero-cost orders
- Customer authentication state management
- Real-time pricing calculations with discounts
- Ongoing/subscription payment display
- Comprehensive error handling and user feedback
- Mobile-optimized accordion interface
- Cart editing capabilities during checkout

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `cart` | `Object` | `undefined` | Cart data object (can be injected from payment provider) |

## Common Props

| Prop Name | Usage |
|-----------|-------|
| `formatCurrency` | Formats all monetary values according to locale |
| `commonProps` | Standard component props for consistency |

## Common Functions

| Function | Usage |
|----------|-------|
| `formatCurrency` | Used to display all price values in correct currency format |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `cartUpdated` | `none` | Emitted when cart contents change during checkout |
| `backToCart` | `none` | Emitted when user wants to return to cart view |
| `cartFinalized` | `orderData` | Emitted when checkout is successfully completed |

## Slots

| Slot | Description |
|------|-------------|
| `header` | Custom header content above checkout form |
| `content` | Complete custom content to replace entire checkout flow |
| `footer` | Custom footer content below checkout form |

## States

### Default Checkout State
```vue
<codex-checkout :cart="cart" />
```

### Authenticated User Checkout
```vue
<codex-checkout 
  :cart="cart"
  @cartFinalized="handleSuccess"
/>
```

### Guest User Checkout
```vue
<!-- Shows login/register buttons in footer -->
<codex-checkout :cart="cart" />
```

### Free Checkout State
```vue
<!-- For zero-cost orders -->
<codex-checkout 
  :cart="freeCart"
  @cartFinalized="handleFreeOrder"
/>
```

### Gift Card Checkout
```vue
<!-- When gift cards cover full amount -->
<codex-checkout 
  :cart="giftCardCart"
  @cartFinalized="handleGiftCardOrder"
/>
```

## Cart Data Structure

### Basic Cart
```javascript
{
  lines: [
    {
      hash: 'line_abc123',
      name: 'Product Name',
      quantity: 1,
      line_price: '$29.99',
      line_original_price: '$39.99',
      buyable_type: 'Product'
    }
  ],
  price_raw: 2999,
  base_price_raw: 3999,
  discount_raw: 1000,
  tax: '$2.40'
}
```

### Subscription Cart
```javascript
{
  lines: [
    {
      hash: 'line_sub456',
      name: 'Premium Plan',
      line_price: '$0.00',
      line_ongoing_price: '$29.99/month',
      line_ongoing_description: 'per month',
      buyable_type: 'Plan',
      buyable: {
        trial_days: 14
      }
    }
  ],
  price_raw: 0,
  ongoing_price_raw: 2999
}
```

### Cart with Voucher and Gift Card
```javascript
{
  lines: [...],
  price_raw: 2999,
  balance_raw: 999, // Remaining after gift card
  voucher: {
    code: 'SAVE20',
    amount: '$15.00'
  },
  payment_methods: {
    paymentMethodGiftCard0: {
      name: 'Gift Card',
      paymentSource: {
        balance: '$20.00',
        last4: '1234'
      }
    }
  }
}
```

## Pricing Calculations

The component handles complex pricing scenarios:

### Combined Join Fees
```javascript
// When window.codex.combine_join_fees is enabled
const combinedTotal = computed(() => {
  if (window.codex?.combine_join_fees) {
    return cart.value?.lines?.reduce((total, line) => {
      if (!hasTrialOrDelayedStart(line)) {
        return total + line.line_ongoing_price_raw
      }
      return total
    }, cart.value.price_raw) || 0
  } else {
    return cart.value?.price_raw || 0
  }
})
```

### Discount Calculations
```javascript
const combinedDiscountTotal = computed(() => {
  if (window.codex?.combine_join_fees) {
    return cart.value?.lines?.reduce((total, line) => {
      if (!hasTrialOrDelayedStart(line)) {
        return total + (line.line_original_ongoing_price_raw - line.line_ongoing_price_raw)
      }
      return total
    }, cart.value.discount_raw) || 0
  } else {
    return cart.value?.discount_raw || 0
  }
})
```

## Translation Support

The Checkout component supports comprehensive internationalization through Vue i18n integration. All checkout flow text, payment states, and error messages are fully translatable.

### Translation Keys

#### Checkout Interface
- `cart.checkout` - Main checkout header/title
- `cart.hide_cart_summary` - Hide cart summary button
- `cart.show_cart_summary` - Show cart summary button

#### Pricing & Cart Summary
- `cart.subtotal` - Subtotal label
- `cart.discount` - Discount label
- `cart.total` - Total label
- `cart.including` - Tax inclusion prefix ("Including")
- `cart.in_taxes` - Tax inclusion suffix ("in taxes")

#### Payment Processing
- `cart.processing` - Generic processing state
- `cart.error` - Generic error state
- `cart.payment_failed` - Payment failure title
- `cart.payment_failed_choose_alt` - Payment failure instruction
- `cart.nothing_to_pay` - Free checkout button
- `cart.pay_using_gift_card` - Gift card payment button (with parameter)

#### Payment Methods
- `cart.hide_payment_methods` - Hide payment methods accordion
- `cart.payment_methods` - Show payment methods label
- `cart.expiry` - Credit card expiry label
- `cart.pay_now` - Pay now button prefix
- `card.add_new_payment_method` - Add new payment method button
- `card.purchase_with_card_on_file` - Accessibility label for saved cards

#### Authentication
- `cart.login` - Login button
- `cart.register` - Register button

### Template Usage Examples

#### Checkout Header & Navigation
```vue
<template>
  <div class="_c-checkout-header">
    <button @click="goToCart">
      <i class="ri-arrow-left-s-line"></i>
    </button>
    {{ $t('cart.checkout') }}
  </div>
</template>
```

#### Cart Summary Toggle
```vue
<template>
  <button @click="toggleCartSummary">
    <span v-if="showCartSummary">
      {{ $t('cart.hide_cart_summary') }}
    </span>
    <span v-else>
      {{ $t('cart.show_cart_summary') }}
    </span>
  </button>
</template>
```

#### Pricing Display
```vue
<template>
  <div class="_c-checkout-pricing">
    <!-- Subtotal -->
    <div class="_c-subtotal-row">
      <span>{{ t('cart.subtotal') }}</span>
      <span>{{ formatCurrency(subtotal) }}</span>
    </div>
    
    <!-- Discount -->
    <div v-if="hasDiscount" class="_c-discount-row">
      <span>{{ t('cart.discount') }}</span>
      <span>-{{ formatCurrency(discountAmount) }}</span>
    </div>
    
    <!-- Total -->
    <div class="_c-total-row">
      <span>{{ t('cart.total') }}</span>
      <span>{{ formatCurrency(total) }}</span>
    </div>
    
    <!-- Tax Information -->
    <div class="_c-tax-info">
      {{ t('cart.including') }} {{ formatCurrency(taxAmount) }} {{ t('cart.in_taxes') }}
    </div>
  </div>
</template>
```

#### Payment Method Selection
```vue
<template>
  <div class="_c-payment-methods">
    <!-- Saved payment methods toggle -->
    <button @click="togglePaymentMethods">
      <span v-if="addingNew">
        {{ $t('cart.hide_payment_methods') }}
      </span>
      <span v-else>
        {{ $t('cart.payment_methods') }}
      </span>
    </button>
    
    <!-- Payment method list -->
    <div v-for="method in savedMethods" :key="method.id">
      <span>{{ method.brand }} - {{ method.last4 }}</span>
      <span>{{ $t('cart.expiry') }} - {{ method.exp_month }}/{{ method.exp_year }}</span>
    </div>
    
    <!-- Add new method -->
    <button @click="showNewMethodForm">
      {{ $t('card.add_new_payment_method') }}
    </button>
  </div>
</template>
```

#### Payment Buttons
```vue
<template>
  <div class="_c-payment-buttons">
    <!-- Free checkout -->
    <codex-button 
      v-if="isFreeCheckout"
      :processing="processing"
      :processing-text="t('cart.processing')"
      :error-text="t('cart.error')"
      :default-text="$t('cart.nothing_to_pay')"
      @click="handleFreePay"
    />
    
    <!-- Gift card payment -->
    <codex-button 
      v-else-if="canPayWithGiftCard"
      :processing="processing"
      :processing-text="t('cart.processing')"
      :error-text="t('cart.error')"
      :default-text="$t('cart.pay_using_gift_card', { name: giftCard.name })"
      @click="payWithGiftCard"
    />
    
    <!-- Regular payment -->
    <codex-button 
      v-else
      :processing="processing"
      :processing-text="t('cart.processing')"
      :default-text="t('cart.pay_now') + ' - ' + finalPrice"
      :disabled-text="t('cart.pay_now')"
      :error-text="t('cart.error')"
      @click="processPayment"
    />
  </div>
</template>
```

#### Error Handling
```vue
<template>
  <div v-if="paymentError" class="_c-payment-error">
    <h3>{{ $t('cart.payment_failed') }}</h3>
    <p>{{ $t('cart.payment_failed_choose_alt') }}</p>
    <div class="_c-error-details">{{ genericErrors }}</div>
  </div>
</template>
```

#### Authentication State
```vue
<template>
  <div v-if="!customer" class="_c-checkout-auth">
    <button data-modal-toggle="codex-login">
      {{ $t('cart.login') }}
    </button>
    <button data-modal-toggle="codex-register">
      {{ $t('cart.register') }}
    </button>
  </div>
</template>
```

### Integration with useI18n

The component uses Vue i18n composition API throughout:

```javascript
import { useI18n } from 'vue-i18n'

const { t } = useI18n()

// Payment button text computation
const paymentButtonText = computed(() => {
  if (processing.value) return t('cart.processing')
  if (error.value) return t('cart.error')
  if (canPayWithGiftCard.value) {
    return t('cart.pay_using_gift_card', { name: giftCard.value.name })
  }
  return t('cart.pay_now') + ' - ' + formatCurrency(finalAmount.value)
})

// Error message handling
const handlePaymentError = (error) => {
  const errorMessage = error.decline_code 
    ? t(`payment.decline_code.${error.decline_code}`)
    : t('cart.payment_failed')
  
  showError(errorMessage)
}
```

### Parameter Support

Translation keys that support dynamic parameters:

```javascript
// Gift card payment with card name
$t('cart.pay_using_gift_card', { name: giftCard.value.name })

// Tax information display
`${t('cart.including')} ${formatCurrency(tax)} ${t('cart.in_taxes')}`

// Payment amount with total
`${t('cart.pay_now')} - ${formatCurrency(total)}`
```

### Accessibility Considerations

The component includes accessibility-focused translations:

```vue
<template>
  <button 
    :aria-label="t('card.purchase_with_card_on_file')"
    :data-last-four="method.last4"
    :data-card-brand="method.brand"
  >
    {{ t('cart.pay_now') }}
  </button>
</template>
```

The Checkout component's comprehensive translation system ensures a fully localized payment experience across all payment methods, states, and user interactions.

## Internationalization

The component uses the following translation keys:

### Core Checkout Interface
| Key | Usage |
|-----|-------|
| `cart.checkout` | Main checkout header title |
| `cart.show_cart_summary` | Show cart summary button text |
| `cart.hide_cart_summary` | Hide cart summary button text |

### Cart Summary and Pricing
| Key | Usage |
|-----|-------|
| `cart.subtotal` | Subtotal line label |
| `cart.discount` | Discount line label |
| `cart.total` | Total price label |
| `cart.including` | Tax inclusion prefix ("Including") |
| `cart.in_taxes` | Tax suffix ("in taxes") |

### Payment Processing
| Key | Usage |
|-----|-------|
| `cart.processing` | Payment processing state |
| `cart.error` | Payment error state |
| `cart.payment_failed` | Payment failed title |
| `cart.payment_failed_choose_alt` | Payment failed instruction text |

### Payment Options
| Key | Usage |
|-----|-------|
| `cart.nothing_to_pay` | Free checkout button text |
| `cart.pay_using_gift_card` | Gift card payment button text |

### Authentication Prompts
| Key | Usage |
|-----|-------|
| `cart.login` | Login button text |
| `cart.register` | Register button text |

### Translation Usage Examples
```vue
<!-- Checkout header with back button -->
<div class="checkout-title">
  <button class="back-button" @click="goToCart">
    <i class="ri-arrow-left-s-line"></i>
  </button>
  {{ $t('cart.checkout') }}
</div>

<!-- Collapsible cart summary -->
<button 
  class="cart-summary-toggle"
  @click="toggleCartSummary"
>
  <span>
    <template v-if="showCartSummary">
      {{ $t('cart.hide_cart_summary') }}
    </template>
    <template v-else>
      {{ $t('cart.show_cart_summary') }}
    </template>
  </span>
</button>

<!-- Payment failed error display -->
<div v-if="genericErrors" class="error-container">
  <codex-title :content="$t('cart.payment_failed')" />
  <codex-paragraph :content="$t('cart.payment_failed_choose_alt')" />
  <div class="error-details">{{ genericErrors }}</div>
</div>

<!-- Pricing summary (same as CartContents) -->
<div class="subtotal">
  <div>{{ t('cart.subtotal') }}</div>
  <div>{{ formatCurrency(combinedOriginalTotal) }}</div>
</div>

<div class="total">
  <div>{{ t('cart.total') }}</div>
  <div>{{ formatCurrency(combinedTotal) }}</div>
</div>

<div class="taxes">
  {{ t('cart.including') }} {{ cart.tax }} {{ t('cart.in_taxes') }}
</div>

<!-- Free checkout button -->
<codex-button 
  v-if="canCheckoutWithoutPaymentDetails"
  @click="handleFreePay"
  :processingText="t('cart.processing')"
  :errorText="t('cart.error')"
  :defaultText="$t('cart.nothing_to_pay')"
/>

<!-- Gift card checkout button -->
<codex-button 
  v-else-if="canFullyPayWithGiftCard"
  @click="handleFreePay"
  :processingText="t('cart.processing')"
  :errorText="t('cart.error')"
  :defaultText="$t('cart.pay_using_gift_card', { name: giftCard.name })"
/>

<!-- Authentication prompts for guests -->
<div v-if="!customer" class="auth-buttons">
  <button data-codex-modal-toggle="codex-login">
    {{ $t('cart.login') }}
  </button>
  <button data-codex-modal-toggle="codex-register">
    {{ $t('cart.register') }}
  </button>
</div>
```

### Payment Integration Notes
- The component integrates with Stripe Elements which handles payment method localization
- Error messages from payment providers are typically in English but can be supplemented with translated explanations
- Gift card names are displayed using interpolation: `$t('cart.pay_using_gift_card', { name: giftCard.name })`

## Examples

### E-commerce Checkout
```vue
<template>
  <div class="ecommerce-checkout">
    <codex-checkout
      :cart="cart"
      @cartUpdated="trackCheckoutProgress"
      @backToCart="returnToCart"
      @cartFinalized="handleOrderSuccess"
    >
      <template #header>
        <div class="checkout-header">
          <h2>Secure Checkout</h2>
          <div class="progress-indicator">
            <div class="step active">Cart</div>
            <div class="step active">Checkout</div>
            <div class="step">Complete</div>
          </div>
          <div class="security-badges">
            <img src="/images/ssl-badge.png" alt="SSL Secured">
            <img src="/images/pci-badge.png" alt="PCI Compliant">
          </div>
        </div>
      </template>
      
      <template #footer>
        <div class="checkout-footer">
          <div class="support-info">
            <p>Need help? Call <a href="tel:1-800-555-0123">1-800-555-0123</a></p>
          </div>
          <div class="legal-links">
            <a href="/terms">Terms</a>
            <a href="/privacy">Privacy</a>
            <a href="/returns">Returns</a>
          </div>
        </div>
      </template>
    </codex-checkout>
  </div>
</template>

<script setup>
const cart = ref({
  lines: [
    {
      hash: 'product_1',
      name: 'Wireless Headphones',
      quantity: 1,
      line_price: '$199.99',
      line_original_price: '$249.99',
      buyable_type: 'Product'
    }
  ],
  price_raw: 19999,
  base_price_raw: 24999,
  discount_raw: 5000,
  tax: '$16.00'
})

const trackCheckoutProgress = () => {
  // Analytics tracking
  gtag('event', 'checkout_progress', {
    event_category: 'ecommerce',
    event_label: 'payment_selection'
  })
}

const returnToCart = () => {
  router.push('/cart')
}

const handleOrderSuccess = (orderData) => {
  // Track conversion
  gtag('event', 'purchase', {
    transaction_id: orderData.id,
    value: orderData.total,
    currency: 'USD'
  })
  
  // Navigate to success page
  router.push(`/order-success/${orderData.id}`)
}
</script>
```

### Subscription Checkout Flow
```vue
<template>
  <div class="subscription-checkout">
    <codex-checkout
      :cart="subscriptionCart"
      @cartFinalized="handleSubscriptionSuccess"
      @backToCart="returnToPlanSelection"
    >
      <template #header>
        <div class="subscription-header">
          <h2>Complete Your Subscription</h2>
          <div class="plan-summary">
            <div class="selected-plan">
              <h3>{{ selectedPlan.name }}</h3>
              <div class="plan-features">
                <div v-for="feature in selectedPlan.features" :key="feature" class="feature">
                  <i class="ri-check-line"></i>
                  <span>{{ feature }}</span>
                </div>
              </div>
            </div>
            <div class="billing-info">
              <div class="trial-notice" v-if="subscriptionCart.lines[0]?.buyable?.trial_days">
                <i class="ri-gift-line"></i>
                <span>{{ subscriptionCart.lines[0].buyable.trial_days }} day free trial</span>
              </div>
              <div class="billing-cycle">
                <span>Bills {{ subscriptionCart.lines[0]?.line_ongoing_description }}</span>
              </div>
              <div class="cancellation-policy">
                <span>Cancel anytime</span>
              </div>
            </div>
          </div>
        </div>
      </template>
      
      <template #footer>
        <div class="subscription-footer">
          <div class="guarantees">
            <div class="guarantee">
              <i class="ri-shield-check-line"></i>
              <span>30-day money-back guarantee</span>
            </div>
            <div class="guarantee">
              <i class="ri-loop-left-line"></i>
              <span>Cancel anytime</span>
            </div>
          </div>
        </div>
      </template>
    </codex-checkout>
  </div>
</template>

<script setup>
const subscriptionCart = ref({
  lines: [
    {
      hash: 'sub_premium',
      name: 'Premium Plan',
      line_price: '$0.00',
      line_ongoing_price: '$29.99/month',
      line_ongoing_description: 'monthly',
      buyable_type: 'Plan',
      buyable: {
        trial_days: 14
      }
    }
  ],
  price_raw: 0,
  ongoing_price_raw: 2999
})

const selectedPlan = ref({
  name: 'Premium Plan',
  features: [
    'Unlimited access to all content',
    'Priority customer support',
    'Advanced analytics',
    'Custom integrations'
  ]
})

const handleSubscriptionSuccess = (orderData) => {
  // Track subscription conversion
  analytics.track('subscription_started', {
    plan: selectedPlan.value.name,
    amount: orderData.ongoing_total,
    trial_days: subscriptionCart.value.lines[0]?.buyable?.trial_days
  })
  
  // Navigate to welcome flow
  router.push('/welcome')
}

const returnToPlanSelection = () => {
  router.push('/pricing')
}
</script>
```

### Gift Card Checkout
```vue
<template>
  <div class="gift-card-checkout">
    <codex-checkout
      :cart="giftCardCart"
      @cartFinalized="handleGiftCardOrder"
    >
      <template #header>
        <div class="gift-card-header">
          <h2>Complete Your Order</h2>
          <div class="gift-card-summary">
            <div class="applied-gift-cards">
              <h4>Applied Gift Cards</h4>
              <div v-for="(method, key) in giftCardCart.payment_methods" :key="key" class="gift-card-item">
                <i class="ri-gift-line"></i>
                <span>Ending in {{ method.paymentSource.last4 }}</span>
                <span class="balance">{{ method.paymentSource.balance }}</span>
              </div>
            </div>
            <div class="remaining-balance" v-if="giftCardCart.balance_raw > 0">
              <span>Remaining to pay: {{ formatCurrency(giftCardCart.balance_raw) }}</span>
            </div>
          </div>
        </div>
      </template>
    </codex-checkout>
  </div>
</template>

<script setup>
const giftCardCart = ref({
  lines: [
    {
      hash: 'product_1',
      name: 'Premium Widget',
      line_price: '$99.99',
      buyable_type: 'Product'
    }
  ],
  price_raw: 9999,
  balance_raw: 2999, // Remaining after gift card
  payment_methods: {
    paymentMethodGiftCard0: {
      name: 'Gift Card',
      paymentSource: {
        balance: '$70.00',
        last4: '1234'
      }
    }
  }
})

const handleGiftCardOrder = (orderData) => {
  // Track gift card usage
  analytics.track('gift_card_used', {
    order_id: orderData.id,
    gift_card_amount: 7000,
    remaining_balance: 2999
  })
  
  router.push(`/order-confirmation/${orderData.id}`)
}
</script>
```

### Mobile-Optimized Checkout
```vue
<template>
  <div class="mobile-checkout">
    <codex-checkout
      :cart="cart"
      class="mobile-checkout-component"
      @cartFinalized="handleMobileSuccess"
      @backToCart="closeMobileCheckout"
    >
      <template #header>
        <div class="mobile-header">
          <button @click="closeMobileCheckout" class="back-btn">
            <i class="ri-arrow-left-line"></i>
          </button>
          <h2>Checkout</h2>
          <div class="cart-total">{{ formatCurrency(cart.price_raw) }}</div>
        </div>
      </template>
      
      <template #footer>
        <div class="mobile-footer">
          <div class="security-info">
            <i class="ri-shield-check-line"></i>
            <span>Secure payment powered by Stripe</span>
          </div>
        </div>
      </template>
    </codex-checkout>

    <!-- Mobile-specific cart summary toggle -->
    <div class="mobile-cart-summary-fab" @click="toggleMobileCartSummary">
      <i class="ri-shopping-cart-line"></i>
      <span class="item-count">{{ cart.lines.length }}</span>
    </div>
  </div>
</template>

<script setup>
const cart = ref({
  lines: [
    {
      hash: 'mobile_product',
      name: 'Mobile Accessory',
      line_price: '$49.99'
    }
  ],
  price_raw: 4999
})

const showMobileCartSummary = ref(false)

const handleMobileSuccess = (orderData) => {
  // Mobile-specific success handling
  if (navigator.vibrate) {
    navigator.vibrate([200, 100, 200]) // Success pattern
  }
  
  // Show mobile success screen
  showMobileSuccessScreen(orderData)
}

const closeMobileCheckout = () => {
  // Handle mobile navigation
  window.history.back()
}

const toggleMobileCartSummary = () => {
  showMobileCartSummary.value = !showMobileCartSummary.value
}
</script>

<style scoped>
.mobile-checkout-component {
  min-height: 100vh;
  padding-bottom: 80px; /* Space for mobile elements */
}

.mobile-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  background: white;
  border-bottom: 1px solid #eee;
  position: sticky;
  top: 0;
  z-index: 100;
}

.back-btn {
  background: none;
  border: none;
  font-size: 20px;
  padding: 8px;
}

.cart-total {
  font-weight: bold;
  color: #007bff;
}

.mobile-cart-summary-fab {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: #007bff;
  color: white;
  border-radius: 50%;
  width: 56px;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 12px rgba(0,123,255,0.3);
  cursor: pointer;
  z-index: 200;
}

.item-count {
  position: absolute;
  top: -4px;
  right: -4px;
  background: #ff4444;
  color: white;
  border-radius: 10px;
  padding: 2px 6px;
  font-size: 10px;
  font-weight: bold;
  min-width: 16px;
  text-align: center;
}

.mobile-footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: white;
  border-top: 1px solid #eee;
  padding: 12px 16px;
  text-align: center;
}

.security-info {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 12px;
  color: #666;
}
</style>
```

### International Checkout
```vue
<template>
  <div class="international-checkout">
    <codex-checkout
      :cart="internationalCart"
      @cartFinalized="handleInternationalOrder"
    >
      <template #header>
        <div class="international-header">
          <h2>{{ $t('checkout.secure_checkout') }}</h2>
          <div class="region-info">
            <div class="shipping-country">
              <i class="ri-map-pin-line"></i>
              <span>{{ $t('checkout.shipping_to') }} {{ shippingCountry }}</span>
            </div>
            <div class="currency-display">
              <i class="ri-exchange-line"></i>
              <span>{{ $t('checkout.prices_in') }} {{ currentCurrency }}</span>
            </div>
            <div class="estimated-delivery">
              <i class="ri-truck-line"></i>
              <span>{{ $t('checkout.estimated_delivery') }} {{ estimatedDelivery }}</span>
            </div>
          </div>
        </div>
      </template>
      
      <template #footer>
        <div class="international-footer">
          <div class="international-support">
            <p>{{ $t('checkout.international_support') }}</p>
            <div class="support-options">
              <a :href="`mailto:${supportEmail}`">{{ supportEmail }}</a>
              <a :href="`tel:${supportPhone}`">{{ supportPhone }}</a>
            </div>
          </div>
          <div class="legal-compliance">
            <span>{{ $t('checkout.vat_included') }}</span>
            <a href="/international-terms">{{ $t('checkout.international_terms') }}</a>
          </div>
        </div>
      </template>
    </codex-checkout>
  </div>
</template>

<script setup>
const internationalCart = ref({
  lines: [
    {
      hash: 'intl_product',
      name: 'International Product',
      line_price: '€89.99',
      buyable_type: 'Product'
    }
  ],
  price_raw: 8999,
  tax: '€14.40', // VAT included
  currency: 'EUR'
})

const shippingCountry = ref('Germany')
const currentCurrency = ref('EUR')
const estimatedDelivery = ref('3-5 business days')
const supportEmail = ref('international@company.com')
const supportPhone = ref('+49-30-12345678')

const handleInternationalOrder = (orderData) => {
  // Track international conversion
  analytics.track('international_purchase', {
    country: shippingCountry.value,
    currency: currentCurrency.value,
    order_value: orderData.total,
    shipping_method: orderData.shipping_method
  })
  
  // Navigate to localized success page
  router.push(`/${locale.value}/order-success/${orderData.id}`)
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-checkout` | Main checkout container |
| `codex` | Base component styling |
| `_c-header` | Header section |
| `_c-title` | Title styling |
| `_c-divider` | Divider line styling |
| `_c-btn-back` | Back button styling |
| `_c-content` | Main content area |
| `_c-cart-summary-toggle` | Cart summary toggle button |
| `_c-accordion-toggle` | Accordion toggle styling |
| `_c-collapsed` | Collapsed state styling |
| `_c-cart-summary` | Cart summary container |
| `_c-accordion` | Accordion wrapper |
| `_c-accordion-inner` | Accordion inner content |
| `_c-cart-card` | Cart display card |
| `_c-line-items` | Cart items container |
| `_c-sub-total` | Subtotal section |
| `_c-price-container` | Price display container |
| `_c-discount-container` | Discount display |
| `_c-voucher` | Voucher display |
| `_c-total` | Total price section |
| `_c-giftcard-container` | Gift card display |
| `_c-taxes` | Tax information |
| `_c-price-ongoing` | Recurring price display |
| `_c-error-container` | Error state container |
| `_c-payment-wrapper` | Payment methods wrapper |
| `_c-disabled-payment` | Disabled payment state |
| `_c-btn-container` | Button container |
| `_c-footer` | Footer section |

## Best Practices

### Checkout Flow Management
- Always provide clear navigation back to cart
- Show detailed pricing breakdown with all fees
- Handle authentication state gracefully
- Provide progress indicators for multi-step flows

### Payment Integration
- Use centralized payment status management
- Handle all payment states (free, gift card, stripe)
- Provide clear error messages for payment failures
- Implement proper loading states during processing

### Cart Summary Management
- Make cart summary collapsible on mobile
- Show all pricing components (subtotal, discounts, taxes)
- Display ongoing/subscription charges clearly
- Allow cart editing during checkout when appropriate

### Error Handling
- Display specific error messages for different failure types
- Provide recovery options for payment failures
- Handle network failures gracefully
- Log checkout errors for debugging

### Mobile Optimization
- Use collapsible interfaces to save space
- Implement touch-friendly controls
- Optimize for one-handed operation
- Consider floating action buttons for cart access

### Security
- Never expose sensitive payment information
- Use secure communication for all operations
- Implement proper CSRF protection
- Follow PCI compliance guidelines

### Accessibility
- Provide screen reader announcements for price changes
- Use appropriate ARIA labels for interactive elements
- Ensure keyboard navigation works properly
- Test with assistive technologies

### Performance
- Optimize for fast checkout completion
- Minimize re-renders during price updates
- Load payment methods efficiently
- Monitor checkout abandonment rates

## Component Registration
```javascript
// Global registration
app.component('CodexCheckout', Checkout)

// Local registration  
import Checkout from '@/components/Cart/Checkout.vue'

export default {
  components: {
    CodexCheckout: Checkout
  }
}
``` 