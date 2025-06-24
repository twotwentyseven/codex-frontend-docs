# CartStateManager Component

## Overview
The CartStateManager component manages and displays different payment processing states during cart checkout and order completion. It provides visual feedback for payment status including processing, success, failure, bank confirmations, and cancellations. The component integrates with the payment status provider to show appropriate UI states and handles cart clearing functionality with comprehensive error handling and user feedback.

## Basic Usage
```vue
<template>
  <div class="checkout-process">
    <!-- This component is typically used within the Cart component -->
    <!-- It automatically shows based on processing/completed states -->
    <codex-cart-state-manager>
      <template #header>
        <h2>Processing Your Order</h2>
      </template>
      
      <template #footer>
        <button @click="returnToShopping">Return to Shopping</button>
      </template>
    </codex-cart-state-manager>
  </div>
</template>
```

## Key Features
- Centralized payment status management and display
- Visual feedback for multiple payment states (processing, success, error, cancelled)
- Integration with usePaymentStatusInjector for shared state
- Bank authentication and SCA (Strong Customer Authentication) support
- Error handling with clear user messaging
- Cart clearing functionality with user confirmation
- Internationalized status messages and descriptions
- Responsive loading indicators and animations
- Customizable header and footer slots

## Configuration Props
This component does not accept any configuration props. All state is injected via the payment status provider.

## Injected State

| Property | Type | Description |
|----------|------|-------------|
| `currentCart` | `Object` | Current cart data from payment provider |
| `currentOrder` | `Object` | Current order data from payment provider |
| `intentStatus` | `String` | Stripe payment intent status |
| `error` | `Boolean/String` | Error state from payment processing |
| `genericErrors` | `String` | Generic error messages |
| `clearCart` | `Function` | Function to clear cart and reset state |

## Events
This component does not emit events directly but uses injected functions to modify shared state.

## Slots

| Slot | Description |
|------|-------------|
| `header` | Custom header content above status display |
| `content` | Complete custom content to replace status displays |
| `footer` | Custom footer content below status display |

## States

### Processing State
```vue
<!-- Default state while payment is being processed -->
<codex-cart-state-manager />
```

### Success State
```vue
<!-- Automatically shown when order.status === 'Paid' or intentStatus === 'succeeded' -->
<codex-cart-state-manager />
```

### Error State
```vue
<!-- Shown when payment fails or requires different payment method -->
<codex-cart-state-manager />
```

### Bank Confirmation State
```vue
<!-- Shown when intentStatus === 'requires_action' for SCA -->
<codex-cart-state-manager />
```

### Cancelled Order State
```vue
<!-- Shown when order.status === 'Cancelled' -->
<codex-cart-state-manager />
```

## Payment Intent Status States

The component responds to different Stripe payment intent statuses:

| Status | Display | Description |
|--------|---------|-------------|
| `succeeded` | Success screen | Payment completed successfully |
| `requires_action` | Bank confirmation | SCA/3D Secure authentication needed |
| `requires_payment_method` | Error screen | Payment failed, new method needed |
| `processing` | Loading spinner | Payment being processed |
| `cancelled` | Error screen | Payment was cancelled |

## Internationalization

The `CartStateManager` component uses translation keys for various payment and order status messages:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `cart.checkout` | Checkout page header | Main checkout title |
| `cart.order_cancelled_title` | Cancelled order error title | When order is cancelled |
| `cart.order_cancelled_description` | Cancelled order error description | Explanation of cancellation |
| `cart.fail_close_order` | Close cancelled order button | Action to clear failed order |
| `cart.transaction_successfull` | Payment success title | Successful payment confirmation |
| `cart.transaction_congratulation` | Payment success description | Success message details |
| `cart.bank_requested_confirmation` | SCA authentication title | When bank requires additional verification |
| `cart.bank_requested_confirmation_more_info` | SCA authentication description | Additional SCA information |
| `cart.payment_failed` | Payment failure title | When payment is declined |
| `cart.payment_failed_choose_alt` | Payment failure description | Instructions for alternative payment |
| `cart.payment_processing` | Processing payment title | During payment verification |
| `cart.wait_while_confirm_payment` | Processing payment description | While confirming payment |
| `cart.close_and_clear_cart` | Clear cart button text | Action to close and clear cart |
| `cart.closing` | Processing state for clear action | While clearing cart |

### Payment Status Handling

The component renders different content based on order and payment intent status:

```vue
<!-- Cancelled Order -->
<template v-if="order?.status == 'Cancelled'">
    <codex-title :content="$t('cart.order_cancelled_title')" />
    <codex-paragraph :content="$t('cart.order_cancelled_description')" />
    <button @click.prevent="clearCart">{{ $t('cart.fail_close_order') }}</button>
</template>

<!-- Successful Payment -->
<template v-else-if="order?.status == 'Paid' || intentStatus == 'succeeded'">
    <codex-title :content="$t('cart.transaction_successfull')" />
    <codex-paragraph :content="$t('cart.transaction_congratulation')" />
</template>

<!-- Requires Action (SCA) -->
<template v-else-if="intentStatus == 'requires_action'">
    <codex-title :content="$t('cart.bank_requested_confirmation')" />
    <codex-paragraph :content="$t('cart.bank_requested_confirmation_more_info')" />
</template>

<!-- Payment Failed -->
<template v-else-if="intentStatus == 'requires_payment_method'">
    <codex-title :content="$t('cart.payment_failed')" />
    <codex-paragraph :content="$t('cart.payment_failed_choose_alt')" />
</template>

<!-- Processing -->
<template v-else>
    <codex-title :content="$t('cart.payment_processing')" />
    <codex-paragraph :content="$t('cart.wait_while_confirm_payment')" />
</template>
```

### Footer Actions

```vue
<codex-button 
    v-if="error && intentStatus != 'requires_payment_method'" 
    @click.prevent="clearCart" 
    :defaultText="$t('cart.close_and_clear_cart')" 
    :processingText="$t('cart.closing')" 
    :disabledText="$t('cart.close_and_clear_cart')"
/>
```

### Integration with Payment Handler

The component uses the payment handler composable for state management:

```javascript
const { 
    currentCart,
    currentOrder,
    intentStatus,
    error,
    genericErrors,
    clearCart
} = usePaymentStatusInjector()
```

### Notes
- Comprehensive coverage of all Stripe payment intent statuses
- Error states include both title and description translations
- Integration with `usePaymentHandler` for centralized payment state
- Visual icons complement translation messages for better UX
- Generic error messages handled through the payment handler composable

## Examples

### E-commerce Checkout Flow
```vue
<template>
  <div class="ecommerce-checkout">
    <!-- CartStateManager is used within the main Cart component -->
    <codex-cart>
      <!-- This will show CartStateManager when processing/completed -->
    </codex-cart>
    
    <!-- Or use independently with payment provider -->
    <payment-status-provider>
      <codex-cart-state-manager>
        <template #header>
          <div class="checkout-header">
            <h2>Order Processing</h2>
            <div class="order-number" v-if="orderNumber">
              Order #{{ orderNumber }}
            </div>
          </div>
        </template>
        
        <template #footer>
          <div class="checkout-actions">
            <button @click="goToDashboard" class="dashboard-btn">
              View in Account
            </button>
            <button @click="continueShopping" class="continue-btn">
              Continue Shopping
            </button>
          </div>
        </template>
      </codex-cart-state-manager>
    </payment-status-provider>
  </div>
</template>

<script setup>
const orderNumber = ref('')

const goToDashboard = () => {
  router.push('/account/orders')
}

const continueShopping = () => {
  router.push('/products')
}
</script>
```

### Subscription Signup Flow
```vue
<template>
  <div class="subscription-checkout">
    <payment-status-provider>
      <codex-cart-state-manager>
        <template #header>
          <div class="subscription-header">
            <h2>Setting Up Your Subscription</h2>
            <div class="plan-info">
              <span>{{ selectedPlan.name }}</span>
              <span>{{ selectedPlan.price }}/month</span>
            </div>
          </div>
        </template>
        
        <template #content>
          <!-- Custom content for subscription states -->
          <div v-if="subscriptionProcessing" class="subscription-processing">
            <div class="processing-animation">
              <i class="ri-loader-4-fill spin"></i>
            </div>
            <h3>Activating Your Subscription</h3>
            <p>We're setting up your account and processing your first payment...</p>
            
            <div class="setup-steps">
              <div class="step" :class="{ completed: step.completed }" 
                   v-for="step in setupSteps" :key="step.id">
                <i :class="step.completed ? 'ri-check-line' : 'ri-loader-line'"></i>
                <span>{{ step.label }}</span>
              </div>
            </div>
          </div>
          
          <div v-else-if="subscriptionSuccess" class="subscription-success">
            <div class="success-icon">
              <i class="ri-checkbox-circle-fill"></i>
            </div>
            <h3>Welcome to {{ selectedPlan.name }}!</h3>
            <p>Your subscription is now active. You can access all premium features.</p>
            
            <div class="next-billing">
              <i class="ri-calendar-line"></i>
              <span>Next billing: {{ nextBillingDate }}</span>
            </div>
          </div>
          
          <!-- Fallback to default content -->
          <slot />
        </template>
        
        <template #footer>
          <div class="subscription-footer" v-if="subscriptionSuccess">
            <button @click="accessDashboard" class="primary-btn">
              Access Dashboard
            </button>
            <button @click="downloadApp" class="secondary-btn">
              Download App
            </button>
          </div>
        </template>
      </codex-cart-state-manager>
    </payment-status-provider>
  </div>
</template>

<script setup>
const selectedPlan = ref({
  name: 'Premium Plan',
  price: '$29.99'
})

const subscriptionProcessing = ref(false)
const subscriptionSuccess = ref(false)
const nextBillingDate = ref('February 1, 2024')

const setupSteps = ref([
  { id: 1, label: 'Processing payment', completed: true },
  { id: 2, label: 'Creating account', completed: true },
  { id: 3, label: 'Setting up features', completed: false },
  { id: 4, label: 'Sending welcome email', completed: false }
])

const accessDashboard = () => {
  router.push('/dashboard')
}

const downloadApp = () => {
  window.open('/download', '_blank')
}
</script>
```

### High-Value Purchase Processing
```vue
<template>
  <div class="premium-checkout">
    <payment-status-provider>
      <codex-cart-state-manager>
        <template #header>
          <div class="premium-header">
            <h2>Processing Premium Order</h2>
            <div class="order-value">
              Order Value: {{ orderTotal }}
            </div>
            <div class="white-glove-notice">
              🏆 White-glove service included
            </div>
          </div>
        </template>
        
        <template #content>
          <div v-if="premiumProcessing" class="premium-processing">
            <div class="premium-animation">
              <div class="luxury-spinner"></div>
            </div>
            <h3>Processing Your Premium Purchase</h3>
            <p>We're ensuring everything meets our highest standards...</p>
            
            <div class="premium-benefits">
              <div class="benefit">
                <i class="ri-award-line"></i>
                <span>Premium packaging</span>
              </div>
              <div class="benefit">
                <i class="ri-truck-line"></i>
                <span>White-glove delivery</span>
              </div>
              <div class="benefit">
                <i class="ri-customer-service-line"></i>
                <span>Dedicated support</span>
              </div>
            </div>
          </div>
          
          <div v-else-if="premiumSuccess" class="premium-success">
            <div class="luxury-success-icon">
              <i class="ri-vip-crown-fill"></i>
            </div>
            <h3>Premium Order Confirmed</h3>
            <p>Thank you for your premium purchase. A specialist will contact you within 24 hours.</p>
            
            <div class="concierge-info">
              <h4>Your Personal Concierge</h4>
              <div class="concierge-card">
                <img src="/images/concierge.jpg" alt="Concierge">
                <div class="concierge-details">
                  <strong>Sarah Johnson</strong>
                  <span>Premium Client Specialist</span>
                  <a href="tel:+1234567890">+1 (234) 567-8900</a>
                </div>
              </div>
            </div>
          </div>
          
          <slot />
        </template>
        
        <template #footer>
          <div class="premium-footer" v-if="premiumSuccess">
            <button @click="scheduleDelivery" class="luxury-btn">
              Schedule Delivery
            </button>
            <button @click="contactConcierge" class="luxury-btn secondary">
              Contact Concierge
            </button>
          </div>
        </template>
      </codex-cart-state-manager>
    </payment-status-provider>
  </div>
</template>

<script setup>
const orderTotal = ref('$25,000')
const premiumProcessing = ref(false)
const premiumSuccess = ref(false)

const scheduleDelivery = () => {
  router.push('/premium/delivery')
}

const contactConcierge = () => {
  window.open('tel:+1234567890')
}
</script>

<style scoped>
.luxury-spinner {
  width: 60px;
  height: 60px;
  border: 3px solid #f3f3f3;
  border-top: 3px solid #gold;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

.premium-benefits {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-top: 24px;
}

.benefit {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px;
  background: #f8f9fa;
  border-radius: 8px;
}

.luxury-success-icon {
  font-size: 72px;
  color: #ffd700;
  margin-bottom: 16px;
}

.concierge-card {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px;
  background: white;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  margin-top: 12px;
}

.concierge-card img {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  object-fit: cover;
}

.concierge-details {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.luxury-btn {
  background: linear-gradient(45deg, #ffd700, #ffed4e);
  color: #000;
  border: none;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
}

.luxury-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(255, 215, 0, 0.3);
}

.luxury-btn.secondary {
  background: transparent;
  border: 2px solid #ffd700;
  color: #ffd700;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
```

### International Payment Flow
```vue
<template>
  <div class="international-checkout">
    <payment-status-provider>
      <codex-cart-state-manager>
        <template #header>
          <div class="international-header">
            <h2>{{ $t('checkout.processing_international_order') }}</h2>
            <div class="currency-info">
              <span>{{ selectedCurrency }}</span>
              <span>{{ formatCurrency(orderTotal) }}</span>
            </div>
            <div class="shipping-destination">
              🌍 Shipping to {{ shippingCountry }}
            </div>
          </div>
        </template>
        
        <template #content>
          <div v-if="internationalProcessing" class="international-processing">
            <div class="globe-animation">
              <i class="ri-global-line rotating"></i>
            </div>
            <h3>{{ $t('checkout.processing_international') }}</h3>
            <p>{{ $t('checkout.verifying_international_payment') }}</p>
            
            <div class="processing-steps">
              <div class="step" :class="{ active: currentStep >= 1 }">
                <i class="ri-bank-line"></i>
                <span>{{ $t('checkout.verifying_payment') }}</span>
              </div>
              <div class="step" :class="{ active: currentStep >= 2 }">
                <i class="ri-exchange-line"></i>
                <span>{{ $t('checkout.processing_currency') }}</span>
              </div>
              <div class="step" :class="{ active: currentStep >= 3 }">
                <i class="ri-truck-line"></i>
                <span>{{ $t('checkout.arranging_shipping') }}</span>
              </div>
            </div>
          </div>
          
          <slot />
        </template>
        
        <template #footer>
          <div class="international-footer">
            <div class="support-info">
              <p>{{ $t('checkout.international_support') }}</p>
              <a :href="supportEmail">{{ supportEmail }}</a>
            </div>
          </div>
        </template>
      </codex-cart-state-manager>
    </payment-status-provider>
  </div>
</template>

<script setup>
const selectedCurrency = ref('EUR')
const orderTotal = ref(25000) // in cents
const shippingCountry = ref('Germany')
const currentStep = ref(1)
const supportEmail = ref('international@company.com')

const internationalProcessing = ref(false)

// Simulate processing steps
onMounted(() => {
  if (internationalProcessing.value) {
    const stepInterval = setInterval(() => {
      if (currentStep.value < 3) {
        currentStep.value++
      } else {
        clearInterval(stepInterval)
      }
    }, 2000)
  }
})
</script>

<style scoped>
.globe-animation {
  font-size: 48px;
  color: #007bff;
  margin-bottom: 16px;
}

.rotating {
  animation: rotate 2s linear infinite;
}

.processing-steps {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-top: 24px;
}

.step {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px;
  border-radius: 8px;
  background: #f8f9fa;
  opacity: 0.5;
  transition: all 0.3s ease;
}

.step.active {
  opacity: 1;
  background: #e7f3ff;
  border-left: 4px solid #007bff;
}

@keyframes rotate {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
```

### Mobile Payment Processing
```vue
<template>
  <div class="mobile-checkout">
    <payment-status-provider>
      <codex-cart-state-manager class="mobile-state-manager">
        <template #header>
          <div class="mobile-header">
            <h2>Processing Payment</h2>
            <div class="progress-bar">
              <div class="progress-fill" :style="{ width: progressPercentage + '%' }"></div>
            </div>
          </div>
        </template>
        
        <template #content>
          <div class="mobile-processing">
            <div class="mobile-animation">
              <div class="payment-cards">
                <div class="card" v-for="i in 3" :key="i" 
                     :style="{ animationDelay: i * 0.2 + 's' }">
                  💳
                </div>
              </div>
            </div>
            
            <h3>Secure Payment Processing</h3>
            <p>Your payment is being processed securely. Please don't close this screen.</p>
            
            <div class="security-badges">
              <div class="badge">🔒 SSL Encrypted</div>
              <div class="badge">✅ Bank Verified</div>
              <div class="badge">🛡️ Fraud Protected</div>
            </div>
          </div>
        </template>
        
        <template #footer>
          <div class="mobile-footer">
            <button @click="contactSupport" class="support-btn">
              Need Help? Contact Support
            </button>
          </div>
        </template>
      </codex-cart-state-manager>
    </payment-status-provider>
  </div>
</template>

<script setup>
const progressPercentage = ref(0)

const contactSupport = () => {
  // Open support chat or phone
  openSupportChat()
}

// Simulate progress
onMounted(() => {
  const progressInterval = setInterval(() => {
    if (progressPercentage.value < 100) {
      progressPercentage.value += 10
    } else {
      clearInterval(progressInterval)
    }
  }, 500)
})
</script>

<style scoped>
.mobile-state-manager {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.mobile-header {
  padding: 16px;
  background: #f8f9fa;
  border-bottom: 1px solid #e0e0e0;
}

.progress-bar {
  width: 100%;
  height: 4px;
  background: #e0e0e0;
  border-radius: 2px;
  margin-top: 12px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: #007bff;
  transition: width 0.3s ease;
}

.mobile-processing {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 24px;
  text-align: center;
}

.payment-cards {
  display: flex;
  gap: 12px;
  margin-bottom: 24px;
}

.card {
  font-size: 32px;
  animation: bounce 1s ease-in-out infinite;
}

.security-badges {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  justify-content: center;
  margin-top: 24px;
}

.badge {
  background: #e7f3ff;
  color: #007bff;
  padding: 6px 12px;
  border-radius: 16px;
  font-size: 12px;
  font-weight: 500;
}

.mobile-footer {
  padding: 16px;
  border-top: 1px solid #e0e0e0;
}

.support-btn {
  width: 100%;
  background: transparent;
  border: 1px solid #007bff;
  color: #007bff;
  padding: 12px;
  border-radius: 8px;
  font-weight: 500;
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-10px);
  }
  60% {
    transform: translateY(-5px);
  }
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-processing-card` | Main container for state manager |
| `codex` | Base component styling |
| `_c-header` | Header section |
| `_c-title` | Title styling |
| `_c-divider` | Divider line styling |
| `_c-content` | Main content area |
| `_c-processing-content` | Processing state content styling |
| `_c-grow` | Flex grow utility |
| `_c-justify-center` | Center justification utility |
| `_c-footer` | Footer section |
| `_c-error-container` | Error state container |
| `_c-error-icon` | Error icon styling |
| `_c-error-title` | Error title styling |
| `_c-error-desc` | Error description styling |
| `_c-success-container` | Success state container |
| `_c-success-icon` | Success icon styling |
| `_c-success-title` | Success title styling |
| `_c-success-desc` | Success description styling |
| `_c-processing-container` | Processing state container |
| `_c-processing-icon` | Processing icon styling |
| `_c-spin` | Spinning animation utility |
| `_c-description-card-error` | Error message card styling |
| `_c-btn` | Button base styling |
| `_c-btn-close-order` | Close order button styling |

## Best Practices

### State Management
- Always use within a payment status provider context
- Let the component handle state transitions automatically
- Don't manually manipulate payment states outside the provider
- Monitor for payment completion to trigger appropriate actions

### User Experience
- Provide clear visual feedback for each payment state
- Use appropriate loading animations during processing
- Show helpful error messages with actionable next steps
- Include progress indicators for longer processes

### Error Handling
- Display clear error messages for different failure types
- Provide recovery options when possible
- Log payment errors for debugging and improvement
- Handle network failures gracefully

### Security
- Never expose sensitive payment information
- Use secure communication for all payment operations
- Implement proper error handling without revealing system details
- Follow PCI compliance guidelines for payment processing

### Mobile Optimization
- Ensure touch-friendly button sizes
- Optimize animations for mobile performance
- Test on various mobile devices and screen sizes
- Consider mobile-specific payment flows

### Accessibility
- Provide screen reader announcements for state changes
- Use appropriate ARIA labels for status indicators
- Ensure keyboard navigation for all interactive elements
- Test with assistive technologies

### Performance
- Optimize animations for smooth performance
- Minimize re-renders during state transitions
- Use efficient loading indicators
- Monitor for memory leaks in long-running processes

## Component Registration
```javascript
// Global registration
app.component('CodexCartStateManager', CartStateManager)

// Local registration  
import CartStateManager from '@/components/Cart/CartStateManager.vue'

export default {
  components: {
    CodexCartStateManager: CartStateManager
  }
}
``` 