# StripePaymentElement Component

## Overview
The StripePaymentElement component provides a comprehensive Stripe payment interface with both standard and express checkout capabilities. It features advanced payment form handling, setup intent management, express checkout integration (Apple Pay, Google Pay, etc.), and robust error handling. The component supports both payment confirmation and payment method setup flows with automatic retry mechanisms and customizable appearance options.

## Basic Usage
```vue
<template>
  <div class="payment-element-container">
    <codex-stripe-payment-element
      :actingOn="'cart'"
      :addingNew="true"
      :saveButtonText="'Save Payment Method'"
      :cancelButtonText="'Cancel'"
      @confirmPayment="handlePaymentConfirm"
      @confirmSetup="handleSetupConfirm"
      @cancelPayment="handleCancel"
      @paymentError="handleError"
    />
  </div>
</template>

<script setup>
const handlePaymentConfirm = () => {
  console.log('Payment confirmed successfully')
}

const handleSetupConfirm = () => {
  console.log('Payment method setup confirmed')
}

const handleCancel = () => {
  console.log('Payment cancelled')
}

const handleError = (error) => {
  console.error('Payment error:', error)
}
</script>
```

## Key Features
- Comprehensive Stripe Elements integration
- Express checkout support (Apple Pay, Google Pay, etc.)
- Setup intent handling for saving payment methods
- Payment intent confirmation for processing payments
- Customizable button text and appearance
- Automatic element retry and error recovery
- Font loading and custom styling support
- Multi-language locale support

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `actingOn` | `String` | `'cart'` | Payment context - either 'cart' or 'order' |
| `addingNew` | `Boolean` | `false` | Whether adding a new payment method |
| `saveButtonText` | `String/Boolean` | `false` | Custom text for save button |
| `cancelButtonText` | `String/Boolean` | `false` | Custom text for cancel button |
| `stripeReturnUrl` | `String` | `undefined` | Return URL after payment completion |
| `stripeReturnUrlParams` | `Object` | `{ show_cart: true }` | Parameters for return URL |
| `options` | `Object` | `{}` | Additional Stripe options |

## Common Props

| Prop Name | Usage |
|-----------|-------|
| `commonProps` | Standard component props for consistency |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `confirmPayment` | `none` | Emitted when payment is confirmed |
| `confirmSetup` | `none` | Emitted when payment method setup is confirmed |
| `cancelPayment` | `none` | Emitted when payment is cancelled |
| `paymentError` | `errorObject` | Emitted when payment errors occur |
| `paymentSubmitted` | `paymentData` | Emitted when payment is submitted |

## Stripe Configuration

### Appearance Options
```javascript
const stripeAppearance = {
  theme: 'stripe', // or 'night', 'flat'
  variables: {
    fontFamily: '"Gill Sans", sans-serif',
    fontLineHeight: '1.5',
    borderRadius: '10px',
    colorPrimary: '#0570de',
    colorBackground: '#ffffff',
    colorText: '#30313d',
    colorDanger: '#df1b41'
  },
  rules: {
    '.Block': {
      boxShadow: 'none',
      padding: '12px'
    },
    '.Input': {
      padding: '12px'
    },
    '.Tab': {
      padding: '10px 12px 8px 12px',
      border: 'none'
    }
  }
}
```

### Font Configuration
```javascript
const fontConfig = {
  fontFamily: 'Inter',
  fontUrl: 'https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap'
}
```

## Payment Flow Types

### Payment Intent Flow
For processing immediate payments:
1. Component receives payment intent secret
2. User fills out payment form
3. Payment is confirmed via Stripe
4. Success/failure handled by parent

### Setup Intent Flow  
For saving payment methods:
1. Component receives setup intent secret
2. User fills out payment form
3. Payment method is saved via Stripe
4. Setup confirmation handled by parent

## Internationalization Keys

| Key | Default | Description |
|-----|---------|-------------|
| `cart.processing` | "Processing..." | Payment processing text |
| `cart.save_and_checkout` | "Save and Checkout" | Default save button text |
| `button.cancel` | "Cancel" | Default cancel button text |
| `button.cancelling` | "Cancelling..." | Cancel processing text |
| `cart.express_checkout` | "Express Checkout" | Express checkout label |

## Examples

### E-commerce Payment Form
```vue
<template>
  <div class="ecommerce-payment">
    <div class="payment-header">
      <h3>Payment Information</h3>
      <div class="security-badges">
        <span class="security-badge">
          <i class="ri-shield-check-line"></i>
          SSL Secure
        </span>
        <span class="security-badge">
          <i class="ri-lock-line"></i>
          256-bit Encryption
        </span>
      </div>
    </div>
    
    <codex-stripe-payment-element
      :actingOn="'cart'"
      :addingNew="true"
      :saveButtonText="'Complete Purchase - ' + orderTotal"
      :cancelButtonText="'Back to Cart'"
      :options="paymentOptions"
      @confirmPayment="completeOrder"
      @cancelPayment="returnToCart"
      @paymentError="handlePaymentError"
    />
    
    <div class="payment-footer">
      <div class="accepted-cards">
        <span>We accept:</span>
        <div class="card-icons">
          <i class="ri-visa-line"></i>
          <i class="ri-mastercard-line"></i>
          <i class="ri-paypal-line"></i>
          <i class="ri-apple-pay-line"></i>
          <i class="ri-google-pay-line"></i>
        </div>
      </div>
      
      <div class="money-back-guarantee">
        <i class="ri-shield-star-line"></i>
        <span>30-day money-back guarantee</span>
      </div>
    </div>
  </div>
</template>

<script setup>
const orderTotal = ref('$149.99')

const paymentOptions = {
  layout: {
    type: 'tabs',
    defaultCollapsed: false,
    radios: false,
    spacedAccordionItems: true
  }
}

const completeOrder = async () => {
  try {
    // Order completion logic
    console.log('Order completed successfully')
    
    // Track conversion
    analytics.track('purchase_completed', {
      total: orderTotal.value,
      currency: 'USD',
      timestamp: new Date()
    })
    
    // Redirect to success page
    router.push('/order-success')
  } catch (error) {
    console.error('Order completion failed:', error)
  }
}

const returnToCart = () => {
  router.push('/cart')
}

const handlePaymentError = (error) => {
  console.error('Payment failed:', error)
  
  // Show user-friendly error message
  toast.error('Payment failed. Please check your card details and try again.')
  
  // Track failed payment
  analytics.track('payment_failed', {
    error: error.message,
    total: orderTotal.value
  })
}
</script>
```

### Subscription Payment Setup
```vue
<template>
  <div class="subscription-payment">
    <div class="subscription-summary">
      <h3>Set up your subscription</h3>
      <div class="plan-details">
        <div class="plan-name">{{ subscriptionPlan.name }}</div>
        <div class="plan-price">{{ subscriptionPlan.price }}/{{ subscriptionPlan.period }}</div>
        <div v-if="subscriptionPlan.trial" class="trial-notice">
          <i class="ri-gift-line"></i>
          <span>{{ subscriptionPlan.trial }} free trial - no charge today</span>
        </div>
      </div>
    </div>
    
    <codex-stripe-payment-element
      :actingOn="'cart'"
      :addingNew="true"
      :saveButtonText="subscriptionPlan.trial ? 'Start Free Trial' : 'Subscribe Now'"
      :cancelButtonText="'Choose Different Plan'"
      @confirmSetup="setupSubscription"
      @confirmPayment="processSubscriptionPayment"
      @cancelPayment="choosePlan"
      @paymentError="handleSubscriptionError"
    />
    
    <div class="subscription-terms">
      <div class="billing-info">
        <h4>Billing Information</h4>
        <ul>
          <li v-if="subscriptionPlan.trial">
            Free trial for {{ subscriptionPlan.trial }}
          </li>
          <li>
            After trial: {{ subscriptionPlan.price }} charged {{ subscriptionPlan.period }}
          </li>
          <li>Cancel anytime - no long-term contracts</li>
          <li>All features included with your subscription</li>
        </ul>
      </div>
      
      <div class="terms-agreement">
        <label class="checkbox-label">
          <input type="checkbox" v-model="agreesToTerms" required>
          <span>
            I agree to the <a href="/terms" target="_blank">Terms of Service</a> 
            and <a href="/privacy" target="_blank">Privacy Policy</a>
          </span>
        </label>
      </div>
    </div>
  </div>
</template>

<script setup>
const subscriptionPlan = ref({
  name: 'Premium Plan',
  price: '$29.99',
  period: 'monthly',
  trial: '14 days'
})

const agreesToTerms = ref(false)

const setupSubscription = async () => {
  if (!agreesToTerms.value) {
    toast.error('Please accept the terms and conditions')
    return
  }
  
  try {
    console.log('Setting up subscription...')
    
    // Subscription setup logic
    await setupUserSubscription()
    
    // Track subscription start
    analytics.track('subscription_started', {
      plan: subscriptionPlan.value.name,
      trial: subscriptionPlan.value.trial
    })
    
    // Redirect to welcome page
    router.push('/subscription-welcome')
  } catch (error) {
    console.error('Subscription setup failed:', error)
  }
}

const processSubscriptionPayment = async () => {
  // Handle immediate subscription payment (no trial)
  try {
    await processPayment()
    router.push('/subscription-active')
  } catch (error) {
    console.error('Subscription payment failed:', error)
  }
}

const choosePlan = () => {
  router.push('/pricing')
}

const handleSubscriptionError = (error) => {
  console.error('Subscription error:', error)
  toast.error('Unable to set up subscription. Please try again.')
}
</script>
```

### Mobile Payment Interface
```vue
<template>
  <div class="mobile-payment">
    <div class="mobile-header">
      <button @click="goBack" class="back-button">
        <i class="ri-arrow-left-line"></i>
      </button>
      <h2>Payment</h2>
      <div class="step-indicator">2 of 3</div>
    </div>
    
    <div class="mobile-content">
      <div class="order-summary-mobile">
        <div class="summary-toggle" @click="toggleSummary">
          <span>Order Summary</span>
          <span class="total">{{ orderTotal }}</span>
          <i :class="summaryExpanded ? 'ri-arrow-up-s-line' : 'ri-arrow-down-s-line'"></i>
        </div>
        
        <div v-if="summaryExpanded" class="summary-details">
          <div v-for="item in orderItems" :key="item.id" class="summary-item">
            <span>{{ item.name }}</span>
            <span>{{ item.price }}</span>
          </div>
        </div>
      </div>
      
      <codex-stripe-payment-element
        :actingOn="'cart'"
        :addingNew="true"
        :saveButtonText="'Pay ' + orderTotal"
        :cancelButtonText="'Back'"
        class="mobile-payment-element"
        @confirmPayment="completeMobilePayment"
        @cancelPayment="goBack"
        @paymentError="handleMobileError"
      />
    </div>
    
    <div class="mobile-footer">
      <div class="security-notice">
        <i class="ri-shield-check-line"></i>
        <span>Your payment is secure and encrypted</span>
      </div>
    </div>
  </div>
</template>

<script setup>
const orderTotal = ref('$89.99')
const summaryExpanded = ref(false)

const orderItems = ref([
  { id: 1, name: 'Wireless Headphones', price: '$79.99' },
  { id: 2, name: 'Shipping', price: '$10.00' }
])

const toggleSummary = () => {
  summaryExpanded.value = !summaryExpanded.value
}

const completeMobilePayment = async () => {
  try {
    // Mobile payment completion
    console.log('Mobile payment completed')
    
    // Show success animation
    showMobileSuccessAnimation()
    
    // Haptic feedback
    if (navigator.vibrate) {
      navigator.vibrate([200, 100, 200])
    }
    
    // Navigate to success page
    setTimeout(() => {
      router.push('/mobile/success')
    }, 1500)
  } catch (error) {
    console.error('Mobile payment failed:', error)
  }
}

const goBack = () => {
  router.go(-1)
}

const handleMobileError = (error) => {
  // Mobile-specific error handling
  showMobileErrorSheet(error.message)
}

const showMobileSuccessAnimation = () => {
  // Mobile success animation logic
  console.log('Showing mobile success animation')
}

const showMobileErrorSheet = (message) => {
  // Mobile error sheet display
  console.log('Showing mobile error:', message)
}
</script>

<style scoped>
.mobile-payment {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: #f8f9fa;
}

.mobile-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px;
  background: white;
  border-bottom: 1px solid #e9ecef;
}

.back-button {
  background: none;
  border: none;
  font-size: 20px;
  padding: 8px;
}

.step-indicator {
  font-size: 14px;
  color: #6c757d;
}

.mobile-content {
  flex: 1;
  padding: 16px;
}

.order-summary-mobile {
  background: white;
  border-radius: 8px;
  margin-bottom: 16px;
  overflow: hidden;
}

.summary-toggle {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px;
  cursor: pointer;
}

.total {
  font-weight: bold;
}

.summary-details {
  border-top: 1px solid #e9ecef;
  padding: 16px;
}

.summary-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
}

.mobile-payment-element {
  background: white;
  border-radius: 8px;
  padding: 16px;
}

.mobile-footer {
  padding: 16px;
  background: white;
  border-top: 1px solid #e9ecef;
}

.security-notice {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  color: #6c757d;
  font-size: 14px;
}
</style>
```

### Express Checkout Integration
```vue
<template>
  <div class="express-checkout">
    <div class="checkout-options">
      <h3>Quick Checkout</h3>
      <p>Use your preferred payment method for faster checkout</p>
    </div>
    
    <codex-stripe-payment-element
      :actingOn="'cart'"
      :addingNew="false"
      @confirmPayment="handleExpressPayment"
      @paymentError="handleExpressError"
    />
    
    <div class="checkout-divider">
      <span>OR</span>
    </div>
    
    <div class="standard-checkout">
      <h4>Standard Checkout</h4>
      <codex-stripe-payment-element
        :actingOn="'cart'"
        :addingNew="true"
        :saveButtonText="'Complete Order'"
        @confirmPayment="handleStandardPayment"
        @paymentError="handleStandardError"
      />
    </div>
  </div>
</template>

<script setup>
const handleExpressPayment = async () => {
  try {
    console.log('Express payment completed')
    
    // Track express checkout usage
    analytics.track('express_checkout_used', {
      method: 'apple_pay_or_google_pay',
      timestamp: new Date()
    })
    
    // Faster redirect for express checkout
    router.push('/express-success')
  } catch (error) {
    console.error('Express payment failed:', error)
  }
}

const handleStandardPayment = async () => {
  try {
    console.log('Standard payment completed')
    router.push('/order-confirmation')
  } catch (error) {
    console.error('Standard payment failed:', error)
  }
}

const handleExpressError = (error) => {
  console.error('Express checkout error:', error)
  // Fallback to standard checkout
  toast.info('Express checkout unavailable. Please use standard checkout below.')
}

const handleStandardError = (error) => {
  console.error('Standard checkout error:', error)
  toast.error('Payment failed. Please check your information and try again.')
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-stripe-payment-container` | Main container |
| `_c-content-column` | Column layout container |
| `_c-stripe-payment` | Payment form container |
| `_c-stripe-wrapper` | Stripe elements wrapper |
| `_c-mb-sm` | Small margin bottom |
| `_c-divider` | Visual divider |
| `_c-btn-container` | Button container |
| `_c-fill` | Fill available space |
| `_c-primary-btn` | Primary button styling |
| `_c-secondary-btn` | Secondary button styling |
| `_c-desc` | Description text |
| `_c-text-center` | Center aligned text |

## Best Practices

### Payment Form Setup
- Initialize Stripe elements with proper locale settings
- Configure appearance to match your brand design
- Handle font loading for consistent typography
- Implement proper error boundaries for Stripe failures

### User Experience
- Show clear progress indicators during payment processing
- Provide immediate feedback for form validation
- Use appropriate loading states and skeleton screens
- Implement retry mechanisms for failed payments

### Error Handling
- Display user-friendly error messages
- Log detailed errors for debugging purposes
- Provide fallback options when payment fails
- Handle network connectivity issues gracefully

### Security
- Never log sensitive payment information
- Use Stripe's secure elements for all payment data
- Implement proper CSP headers for Stripe domains
- Follow PCI compliance guidelines

### Mobile Optimization
- Use responsive design for payment forms
- Optimize touch targets for mobile interaction
- Implement proper keyboard handling
- Consider mobile-specific payment methods

### Performance
- Load Stripe library asynchronously
- Cache Stripe instance when possible
- Minimize re-renders during payment processing
- Implement progressive loading for slow connections

### Accessibility
- Provide clear labels for all form fields
- Ensure keyboard navigation works properly
- Use appropriate ARIA attributes
- Test with screen readers and assistive technologies

## Component Registration
```javascript
// Global registration
app.component('CodexStripePaymentElement', StripePaymentElement)

// Local registration  
import StripePaymentElement from '@/components/Cart/StripePaymentElement.vue'

export default {
  components: {
    CodexStripePaymentElement: StripePaymentElement
  }
}
``` 