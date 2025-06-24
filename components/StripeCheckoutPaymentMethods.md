# StripeCheckoutPaymentMethods Component

## Overview
The StripeCheckoutPaymentMethods component provides a comprehensive payment method selection interface for Stripe-powered checkouts. It combines saved payment method management with new payment method entry, featuring collapsible interfaces, brand icon recognition, and seamless integration with Stripe Elements. The component supports both cart and order payment flows with automatic payment method selection, error handling, and retry mechanisms.

## Basic Usage
```vue
<template>
  <div class="checkout-payment">
    <codex-stripe-checkout-payment-methods
      :actingOn="'cart'"
      :notForceReload="true"
      @confirmPayment="handlePaymentConfirm"
      @confirmSetup="handleSetupConfirm" 
      @paymentError="handlePaymentError"
      @hideNewMethod="handleHideNewMethod"
      @hideSummary="handleHideSummary"
    />
  </div>
</template>

<script setup>
const handlePaymentConfirm = () => {
  console.log('Payment confirmed')
}

const handleSetupConfirm = () => {
  console.log('Payment method setup confirmed')
}

const handlePaymentError = (error) => {
  console.error('Payment error:', error)
}

const handleHideNewMethod = () => {
  console.log('Hide new payment method')
}

const handleHideSummary = () => {
  console.log('Hide cart summary')
}
</script>
```

## Key Features
- Saved payment method selection with brand recognition
- Collapsible interface for adding new payment methods
- Integration with StripePaymentElement component
- Automatic payment processing with saved methods
- Card brand icon display for visual recognition
- Error handling and retry mechanisms
- Support for both cart and order payment contexts
- Loading states and skeleton placeholders

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `actingOn` | `String` | `'cart'` | Payment context - either 'cart' or 'order' |
| `notForceReload` | `Boolean` | `required` | Controls whether to force reload Stripe elements |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `confirmPayment` | `none` | Emitted when payment is confirmed |
| `confirmSetup` | `none` | Emitted when payment method setup is confirmed |
| `paymentError` | `errorObject` | Emitted when payment errors occur |
| `hideNewMethod` | `none` | Emitted when new method interface should be hidden |
| `hideSummary` | `none` | Emitted when cart summary should be hidden |

## Payment Method Data Structure

### Saved Payment Method
```javascript
{
  id: 'pm_1234567890',
  brand: 'visa',
  last4: '4242',
  exp_month: 12,
  exp_year: 2025,
  default: true
}
```

### Card Brand Mapping
The component supports extensive card brand recognition:
- Visa, Mastercard, American Express
- Discover, JCB, Diners Club
- Alipay, PayPal, UnionPay
- Regional cards: Elo, Hiper, Maestro, Mir

## Internationalization Keys

| Key | Default | Description |
|-----|---------|-------------|
| `cart.hide_payment_methods` | "Hide payment methods" | Collapse payment methods button |
| `cart.payment_methods` | "Payment methods" | Expand payment methods button |
| `cart.expiry` | "Expiry" | Card expiry label |
| `card.add_new_payment_method` | "Add new payment method" | Add new method button |
| `cart.processing` | "Processing..." | Payment processing text |
| `cart.pay_now` | "Pay Now" | Payment button text |
| `cart.error` | "Error" | Error state text |
| `card.purchase_with_card_on_file` | "Purchase with saved card" | Accessibility label |

## Examples

### E-commerce Checkout Payment
```vue
<template>
  <div class="ecommerce-checkout">
    <div class="checkout-section">
      <h3>Payment Method</h3>
      <codex-stripe-checkout-payment-methods
        :actingOn="'cart'"
        :notForceReload="!forceReload"
        @confirmPayment="processPayment"
        @confirmSetup="savePaymentMethod"
        @paymentError="handleError"
        @hideSummary="collapseSummary"
      />
    </div>
    
    <div v-if="paymentProcessing" class="payment-status">
      <div class="processing-indicator">
        <i class="ri-loader-4-line spinning"></i>
        <span>Processing your payment...</span>
      </div>
    </div>
    
    <div v-if="paymentError" class="error-message">
      <i class="ri-error-warning-line"></i>
      <span>{{ paymentError }}</span>
      <button @click="retryPayment" class="retry-btn">Try Again</button>
    </div>
  </div>
</template>

<script setup>
const forceReload = ref(false)
const paymentProcessing = ref(false)
const paymentError = ref('')

const processPayment = async () => {
  paymentProcessing.value = true
  paymentError.value = ''
  
  try {
    // Payment will be handled by the component
    // This is called after successful payment
    console.log('Payment processed successfully')
    router.push('/order-confirmation')
  } catch (error) {
    console.error('Payment processing failed:', error)
  } finally {
    paymentProcessing.value = false
  }
}

const savePaymentMethod = () => {
  console.log('Payment method saved successfully')
  // Refresh payment methods list
  refreshPaymentMethods()
}

const handleError = (error) => {
  paymentError.value = error.message || 'Payment failed. Please try again.'
  paymentProcessing.value = false
}

const retryPayment = () => {
  paymentError.value = ''
  forceReload.value = true
  // Reset after forcing reload
  nextTick(() => {
    forceReload.value = false
  })
}

const collapseSummary = () => {
  // Hide cart summary when payment method UI expands
  cartSummaryExpanded.value = false
}
</script>
```

### Subscription Payment Setup
```vue
<template>
  <div class="subscription-payment">
    <div class="subscription-plan-summary">
      <h3>{{ selectedPlan.name }}</h3>
      <div class="plan-price">
        <span class="amount">{{ selectedPlan.price }}</span>
        <span class="period">{{ selectedPlan.period }}</span>
      </div>
      <div v-if="selectedPlan.trial_days" class="trial-info">
        <i class="ri-gift-line"></i>
        <span>{{ selectedPlan.trial_days }} day free trial</span>
      </div>
    </div>
    
    <div class="payment-section">
      <h4>Payment Method</h4>
      <codex-stripe-checkout-payment-methods
        :actingOn="'cart'"
        :notForceReload="true"
        @confirmPayment="startSubscription"
        @confirmSetup="setupPaymentMethod"
        @paymentError="handleSubscriptionError"
      />
    </div>
    
    <div class="subscription-terms">
      <div class="terms-checkbox">
        <input type="checkbox" id="terms" v-model="acceptedTerms">
        <label for="terms">
          I agree to the <a href="/terms" target="_blank">Terms of Service</a>
          and <a href="/privacy" target="_blank">Privacy Policy</a>
        </label>
      </div>
      
      <div class="billing-agreement">
        <p>
          <i class="ri-information-line"></i>
          You will be charged {{ selectedPlan.price }} {{ selectedPlan.period }}
          after your trial period ends. Cancel anytime.
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
const selectedPlan = ref({
  name: 'Premium Plan',
  price: '$29.99',
  period: 'monthly',
  trial_days: 14
})

const acceptedTerms = ref(false)

const startSubscription = async () => {
  if (!acceptedTerms.value) {
    alert('Please accept the terms of service to continue')
    return
  }
  
  try {
    console.log('Starting subscription with payment')
    // Subscription activation logic handled by payment system
    router.push('/subscription-welcome')
  } catch (error) {
    console.error('Subscription setup failed:', error)
  }
}

const setupPaymentMethod = () => {
  console.log('Payment method setup for subscription')
}

const handleSubscriptionError = (error) => {
  console.error('Subscription payment error:', error)
  // Show subscription-specific error handling
  showNotification('error', 'Unable to set up subscription. Please try again.')
}
</script>
```

### Mobile-Optimized Payment
```vue
<template>
  <div class="mobile-payment">
    <div class="mobile-header">
      <button @click="goBack" class="back-btn">
        <i class="ri-arrow-left-line"></i>
      </button>
      <h2>Payment</h2>
    </div>
    
    <div class="mobile-payment-container">
      <codex-stripe-checkout-payment-methods
        :actingOn="'cart'"
        :notForceReload="true"
        class="mobile-payment-methods"
        @confirmPayment="handleMobilePayment"
        @confirmSetup="handleMobileSetup"
        @paymentError="handleMobileError"
        @hideSummary="hideMobileSummary"
      />
    </div>
    
    <div class="mobile-footer">
      <div class="security-info">
        <i class="ri-shield-check-line"></i>
        <span>Your payment is secured with SSL encryption</span>
      </div>
    </div>
  </div>
</template>

<script setup>
const handleMobilePayment = () => {
  // Show mobile success animation
  showMobileSuccess()
  
  // Navigate to mobile confirmation
  setTimeout(() => {
    router.push('/mobile/order-success')
  }, 1500)
}

const handleMobileSetup = () => {
  console.log('Mobile payment method setup')
  showMobileNotification('Payment method saved successfully')
}

const handleMobileError = (error) => {
  // Mobile-specific error handling
  showMobileErrorDialog(error.message)
}

const hideMobileSummary = () => {
  // Adjust mobile layout when payment UI expands
  adjustMobileLayout()
}

const goBack = () => {
  router.go(-1)
}

const showMobileSuccess = () => {
  // Mobile success animation
  if (navigator.vibrate) {
    navigator.vibrate([200, 100, 200])
  }
}

const showMobileNotification = (message) => {
  // Mobile notification system
  toast.success(message)
}

const showMobileErrorDialog = (message) => {
  // Mobile error dialog
  modal.confirm({
    title: 'Payment Error',
    message: message,
    confirmText: 'Try Again',
    cancelText: 'Cancel'
  })
}
</script>

<style scoped>
.mobile-payment {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.mobile-header {
  display: flex;
  align-items: center;
  padding: 16px;
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
  margin-right: 16px;
}

.mobile-payment-container {
  flex: 1;
  padding: 16px;
}

.mobile-payment-methods {
  margin-bottom: 20px;
}

.mobile-footer {
  padding: 16px;
  background: #f8f9fa;
  border-top: 1px solid #eee;
}

.security-info {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  color: #666;
  font-size: 14px;
}
</style>
```

### One-Click Payment Experience
```vue
<template>
  <div class="one-click-payment">
    <div class="quick-checkout-header">
      <h3>Quick Checkout</h3>
      <div class="order-total">
        <span>Total: {{ orderTotal }}</span>
      </div>
    </div>
    
    <codex-stripe-checkout-payment-methods
      :actingOn="'cart'"
      :notForceReload="true"
      @confirmPayment="completeOneClickPayment"
      @paymentError="handleOneClickError"
    />
    
    <div class="one-click-benefits">
      <div class="benefit-item">
        <i class="ri-flash-line"></i>
        <span>One-click checkout</span>
      </div>
      <div class="benefit-item">
        <i class="ri-shield-check-line"></i>
        <span>Secure payment</span>
      </div>
      <div class="benefit-item">
        <i class="ri-truck-line"></i>
        <span>Fast shipping</span>
      </div>
    </div>
  </div>
</template>

<script setup>
const orderTotal = ref('$89.99')

const completeOneClickPayment = async () => {
  // Track one-click conversion
  analytics.track('one_click_payment_completed', {
    total: orderTotal.value,
    timestamp: new Date()
  })
  
  // Show success state
  showSuccessMessage('Order placed successfully!')
  
  // Redirect to confirmation
  router.push('/order-confirmation')
}

const handleOneClickError = (error) => {
  // Log one-click payment failure
  analytics.track('one_click_payment_failed', {
    error: error.message,
    total: orderTotal.value
  })
  
  // Fallback to regular checkout
  router.push('/checkout')
}

const showSuccessMessage = (message) => {
  toast.success(message, {
    duration: 3000,
    position: 'top-center'
  })
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-stripe-checkout-payment-methods` | Main component container |
| `_c-column` | Column layout |
| `_c-flex` | Flexbox layout |
| `_c-justify-between` | Space between flex items |
| `_c-payment-methods-toggle` | Payment methods toggle button |
| `_c-accordion-toggle` | Accordion toggle styling |
| `_c-collapsed` | Collapsed state |
| `_c-payment-method-item` | Individual payment method item |
| `_c-items-center` | Center aligned items |
| `_c-gap-sm` | Small gap spacing |
| `_c-brand-icon` | Card brand icon |
| `_c-text-bold` | Bold text styling |
| `_c-text-sm` | Small text size |
| `_c-text-md` | Medium text size |
| `_c-text-icon` | Icon text styling |
| `_c-add-new-payment-method` | Add new method button |
| `_c-btn-container` | Button container |
| `_c-divider` | Visual divider |
| `_c-mb-sm` | Small margin bottom |

## Best Practices

### Payment Method Selection
- Pre-select the customer's default payment method when available
- Show clear visual indicators for selected payment methods
- Display card brand icons for easy recognition
- Provide clear expiry date information

### User Experience
- Use collapsible interfaces to manage screen space efficiently
- Show loading states during payment method retrieval
- Provide clear feedback during payment processing
- Handle errors gracefully with retry options

### Error Handling
- Display specific error messages for different failure types
- Provide clear next steps for resolving payment issues
- Log errors for debugging and support purposes
- Implement automatic retry mechanisms where appropriate

### Security
- Never store or display full card numbers
- Use Stripe's secure elements for all payment data
- Implement proper authentication checks
- Follow PCI compliance guidelines

### Mobile Optimization
- Use touch-friendly interface elements
- Optimize for one-handed operation
- Implement haptic feedback for interactions
- Consider reduced data usage patterns

### Performance
- Load payment methods efficiently on component mount
- Cache payment method data when appropriate
- Minimize re-renders during payment processing
- Implement progressive loading for slow connections

### Accessibility
- Provide screen reader announcements for state changes
- Use appropriate ARIA labels for payment method selection
- Ensure keyboard navigation works properly
- Test with assistive technologies

## Component Registration
```javascript
// Global registration
app.component('CodexStripeCheckoutPaymentMethods', StripeCheckoutPaymentMethods)

// Local registration  
import StripeCheckoutPaymentMethods from '@/components/Cart/StripeCheckoutPaymentMethods.vue'

export default {
  components: {
    CodexStripeCheckoutPaymentMethods: StripeCheckoutPaymentMethods
  }
}
``` 