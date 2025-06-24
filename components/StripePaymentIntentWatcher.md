# StripePaymentIntentWatcher Component

## Overview
The StripePaymentIntentWatcher component provides automated monitoring of Stripe payment intent status changes with intelligent polling and event emission. This invisible component continuously tracks payment intent states, handles different payment statuses appropriately, and provides real-time feedback for payment processing workflows. It features automatic retry logic, error handling, and proper cleanup for failed or successful payments.

## Basic Usage
```vue
<template>
  <div class="payment-monitoring">
    <!-- This component has no visual template - it only watches -->
    <codex-stripe-payment-intent-watcher
      :payment-intent-id="paymentIntentId"
      @stripeStatus="handleStatusChange"
      @paymentFailure="handlePaymentFailure"
    />
    
    <!-- Display status based on watcher events -->
    <div class="payment-status-display">
      <div v-if="currentStatus === 'processing'" class="status-processing">
        <i class="ri-loader-4-line spinning"></i>
        <span>Processing your payment...</span>
      </div>
      
      <div v-else-if="currentStatus === 'succeeded'" class="status-success">
        <i class="ri-check-line"></i>
        <span>Payment successful!</span>
      </div>
      
      <div v-else-if="currentStatus === 'requires_action'" class="status-action">
        <i class="ri-shield-check-line"></i>
        <span>Additional authentication required</span>
      </div>
      
      <div v-else-if="paymentError" class="status-error">
        <i class="ri-error-warning-line"></i>
        <span>{{ paymentError }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
const paymentIntentId = ref('pi_1234567890abcdef')
const currentStatus = ref('processing')
const paymentError = ref('')

const handleStatusChange = (status) => {
  currentStatus.value = status
  console.log('Payment status changed:', status)
}

const handlePaymentFailure = (failure) => {
  paymentError.value = failure.message
  console.error('Payment failed:', failure)
}
</script>
```

## Key Features
- Automated payment intent status polling
- Real-time status change detection and emission
- Intelligent retry logic for transient states
- Automatic polling termination for final states
- Error handling for failed payment methods
- Integration with Stripe JavaScript SDK
- Composable integration with cart system

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `paymentIntentId` | `String` | `required` | Stripe payment intent ID to monitor |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `stripeStatus` | `status` | Emitted when payment intent status changes |
| `paymentFailure` | `failureObject` | Emitted when payment requires new payment method |

## Payment Intent Status Types

The component handles all Stripe payment intent statuses with appropriate polling behavior:

| Status | Polling Behavior | Description |
|--------|------------------|-------------|
| `requires_confirmation` | Continue polling | Payment needs confirmation |
| `requires_action` | Continue polling | Additional authentication required (3D Secure, etc.) |
| `processing` | Continue polling | Payment is being processed |
| `succeeded` | Stop polling | Payment completed successfully |
| `cancelled` | Stop polling | Payment was cancelled |
| `requires_payment_method` | Stop polling, emit failure | Payment failed, new method needed |

## Payment Intent Data Structure

### Basic Payment Intent
```javascript
{
  id: 'pi_1234567890abcdef',
  status: 'processing',
  amount: 2999,
  currency: 'usd',
  client_secret: 'pi_1234567890abcdef_secret_xyz',
  last_payment_error: null
}
```

### Failed Payment Intent
```javascript
{
  id: 'pi_1234567890abcdef',
  status: 'requires_payment_method',
  amount: 2999,
  currency: 'usd',
  last_payment_error: {
    message: 'Your card was declined.',
    code: 'card_declined',
    decline_code: 'generic_decline'
  }
}
```

## Examples

### E-commerce Payment Monitoring
```vue
<template>
  <div class="ecommerce-payment-monitor">
    <codex-stripe-payment-intent-watcher
      :payment-intent-id="orderPaymentIntent"
      @stripeStatus="updatePaymentStatus"
      @paymentFailure="handleOrderPaymentFailure"
    />
    
    <div class="order-payment-status">
      <div class="status-header">
        <h3>Order #{{ orderNumber }}</h3>
        <div class="payment-amount">{{ formatCurrency(orderAmount) }}</div>
      </div>
      
      <div class="status-timeline">
        <div class="timeline-step" :class="{ 'completed': isStatusReached('requires_confirmation') }">
          <div class="step-icon">
            <i class="ri-credit-card-line"></i>
          </div>
          <div class="step-content">
            <h4>Payment Initiated</h4>
            <p>Processing your payment information</p>
          </div>
        </div>
        
        <div class="timeline-step" :class="{ 'completed': isStatusReached('processing'), 'active': currentStatus === 'processing' }">
          <div class="step-icon">
            <i class="ri-refresh-line" :class="{ 'spinning': currentStatus === 'processing' }"></i>
          </div>
          <div class="step-content">
            <h4>Processing Payment</h4>
            <p>Your payment is being processed</p>
          </div>
        </div>
        
        <div class="timeline-step" :class="{ 'completed': isStatusReached('requires_action'), 'active': currentStatus === 'requires_action' }">
          <div class="step-icon">
            <i class="ri-shield-check-line"></i>
          </div>
          <div class="step-content">
            <h4>Authentication</h4>
            <p v-if="currentStatus === 'requires_action'">Please complete additional authentication</p>
            <p v-else>Authentication step</p>
          </div>
        </div>
        
        <div class="timeline-step" :class="{ 'completed': currentStatus === 'succeeded' }">
          <div class="step-icon">
            <i class="ri-check-line"></i>
          </div>
          <div class="step-content">
            <h4>Payment Complete</h4>
            <p>Your order has been confirmed</p>
          </div>
        </div>
      </div>
      
      <div v-if="paymentFailure" class="payment-failure">
        <div class="failure-icon">
          <i class="ri-error-warning-line"></i>
        </div>
        <div class="failure-content">
          <h4>Payment Failed</h4>
          <p>{{ paymentFailure.message }}</p>
          <div class="failure-actions">
            <button @click="retryPayment" class="retry-btn">
              Try Different Payment Method
            </button>
            <button @click="contactSupport" class="support-btn">
              Contact Support
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const orderPaymentIntent = ref('pi_order_2024_001')
const orderNumber = ref('ORD-2024-001')
const orderAmount = ref(14999) // $149.99 in cents
const currentStatus = ref('requires_confirmation')
const paymentFailure = ref(null)

const statusOrder = ['requires_confirmation', 'processing', 'requires_action', 'succeeded']

const updatePaymentStatus = (status) => {
  currentStatus.value = status
  
  // Track payment progress
  analytics.track('payment_status_changed', {
    order_id: orderNumber.value,
    payment_intent_id: orderPaymentIntent.value,
    status: status,
    timestamp: new Date()
  })
  
  // Handle successful payment
  if (status === 'succeeded') {
    setTimeout(() => {
      router.push('/order-confirmation')
    }, 2000)
  }
}

const handleOrderPaymentFailure = (failure) => {
  paymentFailure.value = failure
  
  // Track payment failure
  analytics.track('payment_failed', {
    order_id: orderNumber.value,
    payment_intent_id: orderPaymentIntent.value,
    error_message: failure.message,
    error_code: failure.code
  })
}

const isStatusReached = (status) => {
  const currentIndex = statusOrder.indexOf(currentStatus.value)
  const targetIndex = statusOrder.indexOf(status)
  return currentIndex >= targetIndex && currentStatus.value !== 'requires_payment_method'
}

const formatCurrency = (amount) => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(amount / 100)
}

const retryPayment = () => {
  // Reset payment and show payment method selection
  router.push('/checkout/payment')
}

const contactSupport = () => {
  router.push('/support')
}
</script>
```

### Subscription Payment Monitoring
```vue
<template>
  <div class="subscription-payment-monitor">
    <codex-stripe-payment-intent-watcher
      :payment-intent-id="subscriptionPaymentIntent"
      @stripeStatus="updateSubscriptionPayment"
      @paymentFailure="handleSubscriptionFailure"
    />
    
    <div class="subscription-payment-status">
      <div class="subscription-header">
        <h3>Setting up your subscription</h3>
        <div class="plan-info">
          <span>{{ subscriptionPlan.name }}</span>
          <span>{{ subscriptionPlan.price }}/{{ subscriptionPlan.period }}</span>
        </div>
      </div>
      
      <div class="payment-progress">
        <div class="progress-circle" :class="getProgressClass()">
          <div class="progress-inner">
            <i v-if="currentStatus === 'succeeded'" class="ri-check-line success-icon"></i>
            <i v-else-if="paymentFailure" class="ri-close-line error-icon"></i>
            <i v-else class="ri-refresh-line spinning-icon"></i>
          </div>
        </div>
        
        <div class="progress-text">
          <h4>{{ getStatusTitle() }}</h4>
          <p>{{ getStatusDescription() }}</p>
        </div>
      </div>
      
      <div v-if="currentStatus === 'succeeded'" class="subscription-success">
        <div class="success-actions">
          <button @click="accessAccount" class="primary-btn">
            Access Your Account
          </button>
          <button @click="downloadApp" class="secondary-btn">
            Download Mobile App
          </button>
        </div>
        
        <div class="next-billing">
          <i class="ri-calendar-line"></i>
          <span>Next billing: {{ nextBillingDate }}</span>
        </div>
      </div>
      
      <div v-if="paymentFailure" class="subscription-failure">
        <h4>Subscription Setup Failed</h4>
        <p>{{ paymentFailure.message }}</p>
        <div class="failure-options">
          <button @click="updatePaymentMethod" class="update-payment-btn">
            Update Payment Method
          </button>
          <button @click="chooseDifferentPlan" class="change-plan-btn">
            Choose Different Plan
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const subscriptionPaymentIntent = ref('pi_subscription_2024_001')
const subscriptionPlan = ref({
  name: 'Premium Plan',
  price: '$29.99',
  period: 'monthly'
})
const currentStatus = ref('processing')
const paymentFailure = ref(null)
const nextBillingDate = ref('February 15, 2024')

const updateSubscriptionPayment = (status) => {
  currentStatus.value = status
  
  if (status === 'succeeded') {
    // Subscription is now active
    console.log('Subscription payment successful')
    
    // Track subscription activation
    analytics.track('subscription_activated', {
      plan: subscriptionPlan.value.name,
      price: subscriptionPlan.value.price,
      payment_intent_id: subscriptionPaymentIntent.value
    })
  }
}

const handleSubscriptionFailure = (failure) => {
  paymentFailure.value = failure
  
  // Track subscription setup failure
  analytics.track('subscription_setup_failed', {
    plan: subscriptionPlan.value.name,
    error: failure.message
  })
}

const getProgressClass = () => {
  if (currentStatus.value === 'succeeded') return 'success'
  if (paymentFailure.value) return 'error'
  return 'processing'
}

const getStatusTitle = () => {
  if (currentStatus.value === 'succeeded') return 'Subscription Active!'
  if (paymentFailure.value) return 'Setup Failed'
  return 'Setting Up Subscription...'
}

const getStatusDescription = () => {
  if (currentStatus.value === 'succeeded') {
    return 'Your subscription is now active and ready to use'
  }
  if (paymentFailure.value) {
    return 'There was an issue setting up your subscription'
  }
  return 'Please wait while we process your payment'
}

const accessAccount = () => {
  router.push('/account/dashboard')
}

const downloadApp = () => {
  window.open('/mobile-app', '_blank')
}

const updatePaymentMethod = () => {
  router.push('/account/payment-methods')
}

const chooseDifferentPlan = () => {
  router.push('/pricing')
}
</script>
```

### Mobile Payment Status Tracking
```vue
<template>
  <div class="mobile-payment-tracking">
    <codex-stripe-payment-intent-watcher
      :payment-intent-id="mobilePaymentIntent"
      @stripeStatus="handleMobileStatus"
      @paymentFailure="handleMobileFailure"
    />
    
    <div class="mobile-status-screen">
      <div class="mobile-header">
        <h2>Payment Status</h2>
      </div>
      
      <div class="status-animation">
        <div class="status-circle" :class="getMobileStatusClass()">
          <i :class="getMobileStatusIcon()"></i>
        </div>
      </div>
      
      <div class="status-message">
        <h3>{{ getMobileStatusTitle() }}</h3>
        <p>{{ getMobileStatusMessage() }}</p>
      </div>
      
      <div class="status-progress">
        <div class="progress-bar">
          <div class="progress-fill" :style="{ width: getProgressPercentage() + '%' }"></div>
        </div>
        <div class="progress-percentage">{{ getProgressPercentage() }}%</div>
      </div>
      
      <div v-if="currentStatus === 'succeeded'" class="success-actions">
        <button @click="viewOrder" class="view-order-btn">
          View Order Details
        </button>
        <button @click="continueShopping" class="continue-shopping-btn">
          Continue Shopping
        </button>
      </div>
      
      <div v-if="paymentFailure" class="mobile-failure">
        <button @click="retryMobilePayment" class="retry-mobile-btn">
          Try Again
        </button>
        <button @click="getMobileSupport" class="mobile-support-btn">
          Get Help
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
const mobilePaymentIntent = ref('pi_mobile_2024_001')
const currentStatus = ref('processing')
const paymentFailure = ref(null)

const handleMobileStatus = (status) => {
  currentStatus.value = status
  
  // Mobile-specific status handling
  if (status === 'succeeded') {
    // Haptic feedback for success
    if (navigator.vibrate) {
      navigator.vibrate([200, 100, 200])
    }
    
    // Show success notification
    showMobileNotification('Payment Successful! 🎉')
  } else if (status === 'requires_action') {
    // Alert for additional authentication
    showMobileAlert('Additional authentication required')
  }
}

const handleMobileFailure = (failure) => {
  paymentFailure.value = failure
  
  // Haptic feedback for failure
  if (navigator.vibrate) {
    navigator.vibrate([500])
  }
  
  // Show failure notification
  showMobileNotification('Payment failed. Please try again.')
}

const getMobileStatusClass = () => {
  const classes = {
    'processing': 'status-processing',
    'requires_action': 'status-action',
    'succeeded': 'status-success',
    'failed': 'status-error'
  }
  return classes[paymentFailure.value ? 'failed' : currentStatus.value] || 'status-processing'
}

const getMobileStatusIcon = () => {
  if (currentStatus.value === 'succeeded') return 'ri-check-line'
  if (paymentFailure.value) return 'ri-close-line'
  if (currentStatus.value === 'requires_action') return 'ri-shield-check-line'
  return 'ri-refresh-line spinning'
}

const getMobileStatusTitle = () => {
  if (currentStatus.value === 'succeeded') return 'Payment Complete!'
  if (paymentFailure.value) return 'Payment Failed'
  if (currentStatus.value === 'requires_action') return 'Authentication Required'
  return 'Processing Payment...'
}

const getMobileStatusMessage = () => {
  if (currentStatus.value === 'succeeded') {
    return 'Your order has been confirmed and will be processed shortly.'
  }
  if (paymentFailure.value) {
    return paymentFailure.value.message || 'Please try a different payment method.'
  }
  if (currentStatus.value === 'requires_action') {
    return 'Please complete the additional security verification.'
  }
  return 'Please wait while we process your payment securely.'
}

const getProgressPercentage = () => {
  const progressMap = {
    'requires_confirmation': 25,
    'processing': 50,
    'requires_action': 75,
    'succeeded': 100
  }
  return progressMap[currentStatus.value] || 0
}

const viewOrder = () => {
  router.push('/mobile/order-confirmation')
}

const continueShopping = () => {
  router.push('/mobile/shop')
}

const retryMobilePayment = () => {
  router.push('/mobile/checkout/payment')
}

const getMobileSupport = () => {
  router.push('/mobile/support')
}

const showMobileNotification = (message) => {
  // Mobile toast notification
  toast.success(message, {
    position: 'top-center',
    duration: 3000
  })
}

const showMobileAlert = (message) => {
  // Mobile alert
  alert(message)
}
</script>

<style scoped>
.mobile-status-screen {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 24px;
  text-align: center;
}

.status-circle {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32px;
  margin-bottom: 24px;
}

.status-processing {
  background: #e3f2fd;
  color: #1976d2;
}

.status-success {
  background: #e8f5e8;
  color: #4caf50;
}

.status-error {
  background: #ffebee;
  color: #f44336;
}

.status-action {
  background: #fff3e0;
  color: #ff9800;
}

.progress-bar {
  width: 100%;
  height: 4px;
  background: #e0e0e0;
  border-radius: 2px;
  margin: 16px 0 8px 0;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: #4caf50;
  transition: width 0.5s ease;
}

.spinning {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
```

## Error Handling

### Payment Method Required
```javascript
// When status becomes 'requires_payment_method'
const handlePaymentMethodRequired = (intent) => {
  emit('paymentFailure', {
    message: intent.last_payment_error?.message || 'Payment method declined',
    code: intent.last_payment_error?.code,
    decline_code: intent.last_payment_error?.decline_code
  })
}
```

### Common Error Scenarios
- Card declined: Show user-friendly message and retry option
- Insufficient funds: Suggest alternative payment method
- Authentication failure: Guide user through authentication process
- Network error: Implement retry mechanism with exponential backoff

## Best Practices

### Implementation
- Always provide visual feedback for payment status changes
- Handle all possible payment intent statuses appropriately
- Implement proper loading states during status transitions
- Use the component in conjunction with payment forms

### Error Handling
- Display specific error messages for different failure types
- Provide clear next steps for resolving payment issues
- Log errors for debugging and analytics purposes
- Implement retry mechanisms for transient failures

### User Experience
- Show progress indicators for long-running payments
- Provide clear messaging for required actions
- Handle authentication flows gracefully
- Use appropriate animations and feedback

### Performance
- Component automatically manages its lifecycle
- Polling stops when final states are reached
- Minimal impact on application performance
- Efficient status checking intervals

### Mobile Optimization
- Use haptic feedback for status changes on mobile
- Implement mobile-optimized status displays
- Consider push notifications for background payments
- Use progressive web app features when available

### Accessibility
- Announce status changes to screen readers
- Provide clear descriptions of payment states
- Use appropriate ARIA labels for status indicators
- Test with assistive technologies

## Component Registration
```javascript
// Global registration
app.component('CodexStripePaymentIntentWatcher', StripePaymentIntentWatcher)

// Local registration  
import StripePaymentIntentWatcher from '@/components/Cart/StripePaymentIntentWatcher.vue'

export default {
  components: {
    CodexStripePaymentIntentWatcher: StripePaymentIntentWatcher
  }
} 