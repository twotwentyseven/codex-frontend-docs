# StripeConfirmSCA Component

## Overview
The StripeConfirmSCA component provides a specialized interface for handling Strong Customer Authentication (SCA) requirements in Stripe payment processing. It manages 3D Secure authentication flows, displays pending payment information, handles authentication confirmation, and provides completion workflows for successful transactions. The component integrates with customer authentication systems and offers flexible slot-based customization for different authentication scenarios.

## Basic Usage
```vue
<template>
  <div class="sca-authentication">
    <codex-stripe-confirm-sca />
  </div>
</template>

<script setup>
// Component automatically handles SCA flow based on URL parameters
// and customer authentication state
</script>
```

## Key Features
- Strong Customer Authentication (SCA) flow management
- 3D Secure payment confirmation handling
- Invoice display during authentication process
- Customer authentication state management
- Success state handling with completion actions
- Automatic payment intent loading from URL parameters
- Integration with Stripe's card payment confirmation
- Responsive authentication interface design

## Configuration Props

The component does not require explicit props as it automatically handles authentication flow based on:
- URL parameters (stripe_id)
- Customer authentication state
- Payment intent status

## Events

The component handles authentication flow internally and provides completion actions through:
- Account redirection after successful authentication
- Cart clearing functionality
- URL parameter cleanup

## Authentication Flow States

### Authentication Required
- Customer is not logged in
- Shows login/register buttons
- Prevents authentication until customer is authenticated

### Payment Pending
- Customer is authenticated
- Payment intent requires confirmation
- Shows invoice summary and confirmation button

### Payment Processing
- Confirmation in progress
- Shows loading state with processing feedback
- Handles Stripe confirmation response

### Payment Successful
- Authentication completed successfully
- Shows success message and next steps
- Provides account access and order closure options

## Payment Intent Data Structure

### Pending Payment Intent
```javascript
{
  id: 'pi_1234567890abcdef',
  client_secret: 'pi_1234567890abcdef_secret_xyz',
  amount: 2999,
  status: 'requires_action'
}
```

### Invoice Information
```javascript
{
  order_lines: [
    {
      product: 'Premium Plan',
      quantity: 1,
      final_price: '$29.99'
    }
  ],
  amount_due: 2999,
  discounts: 500,
  tax: 240
}
```

## Internationalization Keys

| Key | Default | Description |
|-----|---------|-------------|
| `sca.login_to_confirm` | "Login to confirm" | Login required header |
| `sca.payment_pending` | "Payment pending" | Authenticated header |
| `sca.transaction_successfull` | "Transaction successful" | Success title |
| `sca.transaction_congratulation` | "Congratulations on your purchase" | Success description |
| `sca.go_to_your_account` | "Go to your account" | Account access button |
| `sca.close_order` | "Close order" | Order closure button |
| `card.your_bank_asked_to_confirm_payment_of` | "Your bank asked to confirm payment of" | Payment confirmation text |
| `invoice.total` | "Total" | Invoice total label |
| `invoice.discounts` | "Discounts" | Discounts label |
| `invoice.including` | "Including" | Tax inclusion label |
| `invoice.in_taxes` | "in taxes" | Tax amount label |
| `cart.login` | "Login" | Login button text |
| `cart.register` | "Register" | Register button text |
| `button.confirm_payment` | "Confirm Payment" | Confirmation button |
| `button.processing` | "Processing..." | Processing state text |

## Slots

| Slot | Description |
|------|-------------|
| `header` | Custom header content for authentication state |
| `content` | Complete content replacement for authentication flow |
| `footer` | Custom footer content with actions |
| `login-register` | Custom login/register interface |
| `error-messages` | Custom error message display |

## Examples

### E-commerce SCA Authentication
```vue
<template>
  <div class="ecommerce-sca">
    <codex-stripe-confirm-sca>
      <template #header>
        <div class="ecommerce-sca-header">
          <div class="company-branding">
            <img src="/logo.png" alt="Company Logo" class="company-logo">
            <h2>Secure Payment Verification</h2>
          </div>
          <div class="security-badges">
            <span class="security-badge">
              <i class="ri-shield-check-line"></i>
              SSL Secured
            </span>
            <span class="security-badge">
              <i class="ri-bank-line"></i>
              Bank Verified
            </span>
          </div>
        </div>
      </template>
      
      <template #content>
        <div class="ecommerce-sca-content">
          <div v-if="!customer" class="authentication-required">
            <div class="auth-prompt">
              <h3>Authentication Required</h3>
              <p>Please log in to complete your secure payment verification.</p>
              
              <div class="auth-benefits">
                <div class="benefit-item">
                  <i class="ri-shield-check-line"></i>
                  <span>Enhanced security for your payment</span>
                </div>
                <div class="benefit-item">
                  <i class="ri-time-line"></i>
                  <span>Quick verification process</span>
                </div>
                <div class="benefit-item">
                  <i class="ri-check-line"></i>
                  <span>Fraud protection included</span>
                </div>
              </div>
            </div>
          </div>
          
          <div v-else-if="invoice" class="payment-verification">
            <div class="verification-header">
              <h3>Verify Your Payment</h3>
              <p>Your bank requires additional verification for this transaction.</p>
            </div>
            
            <div class="payment-summary">
              <div class="summary-header">
                <h4>Payment Summary</h4>
              </div>
              
              <div class="order-details">
                <div v-for="line in invoice.order_lines" :key="line.product" class="order-line">
                  <span class="product-name">{{ line.product }}</span>
                  <span class="quantity">Qty: {{ line.quantity }}</span>
                  <span class="price">{{ line.final_price }}</span>
                </div>
              </div>
              
              <div class="payment-breakdown">
                <div class="breakdown-line">
                  <span>Subtotal:</span>
                  <span>{{ formatCurrency(invoice.amount_due + invoice.discounts - invoice.tax) }}</span>
                </div>
                <div v-if="invoice.discounts > 0" class="breakdown-line discount">
                  <span>Discount:</span>
                  <span>-{{ formatCurrency(invoice.discounts) }}</span>
                </div>
                <div class="breakdown-line">
                  <span>Tax:</span>
                  <span>{{ formatCurrency(invoice.tax) }}</span>
                </div>
                <div class="breakdown-line total">
                  <span>Total:</span>
                  <span>{{ formatCurrency(invoice.amount_due) }}</span>
                </div>
              </div>
            </div>
            
            <div class="verification-notice">
              <i class="ri-information-line"></i>
              <span>This verification helps protect your account from unauthorized charges.</span>
            </div>
          </div>
        </div>
      </template>
      
      <template #footer>
        <div class="ecommerce-sca-footer">
          <div class="security-info">
            <h4>Your Security Matters</h4>
            <div class="security-features">
              <div class="feature">
                <i class="ri-lock-line"></i>
                <div>
                  <strong>Encrypted Communication</strong>
                  <p>All data is encrypted using bank-level security</p>
                </div>
              </div>
              <div class="feature">
                <i class="ri-shield-star-line"></i>
                <div>
                  <strong>Fraud Protection</strong>
                  <p>Advanced fraud detection keeps your payment safe</p>
                </div>
              </div>
            </div>
          </div>
          
          <div class="support-contact">
            <p>Need help? <a href="/support" target="_blank">Contact Support</a></p>
          </div>
        </div>
      </template>
    </codex-stripe-confirm-sca>
  </div>
</template>

<script setup>
const formatCurrency = (amount) => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(amount / 100)
}
</script>
```

### Subscription SCA Authentication
```vue
<template>
  <div class="subscription-sca">
    <codex-stripe-confirm-sca>
      <template #header>
        <div class="subscription-sca-header">
          <h2>Subscription Payment Verification</h2>
          <div class="subscription-info">
            <div class="plan-badge">{{ subscriptionPlan.name }}</div>
            <div class="billing-info">{{ subscriptionPlan.price }}/{{ subscriptionPlan.period }}</div>
          </div>
        </div>
      </template>
      
      <template #content>
        <div class="subscription-sca-content">
          <div v-if="invoice" class="subscription-verification">
            <div class="verification-steps">
              <div class="step completed">
                <div class="step-number">1</div>
                <div class="step-content">
                  <h4>Plan Selected</h4>
                  <p>{{ subscriptionPlan.name }} subscription</p>
                </div>
              </div>
              
              <div class="step active">
                <div class="step-number">2</div>
                <div class="step-content">
                  <h4>Payment Verification</h4>
                  <p>Confirming your payment method</p>
                </div>
              </div>
              
              <div class="step">
                <div class="step-number">3</div>
                <div class="step-content">
                  <h4>Subscription Active</h4>
                  <p>Access to all features</p>
                </div>
              </div>
            </div>
            
            <div class="subscription-benefits">
              <h4>What's included in {{ subscriptionPlan.name }}:</h4>
              <ul class="benefits-list">
                <li v-for="benefit in subscriptionPlan.benefits" :key="benefit">
                  <i class="ri-check-line"></i>
                  <span>{{ benefit }}</span>
                </li>
              </ul>
            </div>
            
            <div class="billing-cycle-info">
              <div class="billing-notice">
                <i class="ri-calendar-line"></i>
                <div>
                  <strong>Billing Cycle</strong>
                  <p>You'll be charged {{ subscriptionPlan.price }} every {{ subscriptionPlan.period }}. Cancel anytime.</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
    </codex-stripe-confirm-sca>
  </div>
</template>

<script setup>
const subscriptionPlan = ref({
  name: 'Premium Plan',
  price: '$29.99',
  period: 'month',
  benefits: [
    'Unlimited access to all features',
    'Priority customer support',
    'Advanced analytics and reporting',
    'Custom integrations',
    'Mobile app access'
  ]
})
</script>
```

### Mobile SCA Authentication
```vue
<template>
  <div class="mobile-sca">
    <codex-stripe-confirm-sca>
      <template #header>
        <div class="mobile-sca-header">
          <div class="mobile-title">
            <h2>Payment Verification</h2>
          </div>
          <div class="mobile-progress">
            <div class="progress-bar">
              <div class="progress-fill" :style="{ width: getProgressWidth() + '%' }"></div>
            </div>
            <span class="progress-text">{{ getProgressText() }}</span>
          </div>
        </div>
      </template>
      
      <template #content>
        <div class="mobile-sca-content">
          <div v-if="!customer" class="mobile-auth-required">
            <div class="auth-icon">
              <i class="ri-user-line"></i>
            </div>
            <h3>Login Required</h3>
            <p>Please log in to verify your payment securely.</p>
            
            <div class="mobile-auth-benefits">
              <div class="mobile-benefit">
                <i class="ri-shield-check-line"></i>
                <span>Secure verification</span>
              </div>
              <div class="mobile-benefit">
                <i class="ri-time-line"></i>
                <span>Quick process</span>
              </div>
            </div>
          </div>
          
          <div v-else-if="invoice" class="mobile-verification">
            <div class="verification-card">
              <div class="card-header">
                <i class="ri-shield-check-line"></i>
                <h3>Verify Payment</h3>
              </div>
              
              <div class="payment-amount">
                <span class="amount">{{ formatCurrency(invoice.amount_due) }}</span>
                <span class="currency">USD</span>
              </div>
              
              <div class="verification-message">
                <p>Your bank requires additional verification for this payment.</p>
              </div>
            </div>
            
            <div class="mobile-order-summary">
              <div class="summary-toggle" @click="toggleSummary">
                <span>Order Summary</span>
                <i :class="summaryExpanded ? 'ri-arrow-up-s-line' : 'ri-arrow-down-s-line'"></i>
              </div>
              
              <div v-if="summaryExpanded" class="summary-details">
                <div v-for="line in invoice.order_lines" :key="line.product" class="mobile-order-line">
                  <span class="product">{{ line.product }}</span>
                  <span class="price">{{ line.final_price }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
      
      <template #footer>
        <div class="mobile-sca-footer">
          <div class="mobile-security-notice">
            <i class="ri-lock-line"></i>
            <span>Secured by bank-level encryption</span>
          </div>
        </div>
      </template>
    </codex-stripe-confirm-sca>
  </div>
</template>

<script setup>
const summaryExpanded = ref(false)
const verificationStep = ref(2) // Current step in verification process

const toggleSummary = () => {
  summaryExpanded.value = !summaryExpanded.value
}

const getProgressWidth = () => {
  return (verificationStep.value / 3) * 100
}

const getProgressText = () => {
  return `Step ${verificationStep.value} of 3`
}

const formatCurrency = (amount) => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(amount / 100)
}
</script>

<style scoped>
.mobile-sca {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.mobile-sca-header {
  padding: 24px 16px;
  text-align: center;
}

.mobile-progress {
  margin-top: 16px;
}

.progress-bar {
  height: 4px;
  background: rgba(255,255,255,0.3);
  border-radius: 2px;
  overflow: hidden;
  margin-bottom: 8px;
}

.progress-fill {
  height: 100%;
  background: white;
  transition: width 0.5s ease;
}

.progress-text {
  font-size: 14px;
  opacity: 0.9;
}

.mobile-sca-content {
  padding: 0 16px 16px;
}

.verification-card {
  background: white;
  color: #333;
  border-radius: 16px;
  padding: 24px;
  text-align: center;
  margin-bottom: 16px;
}

.card-header {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  margin-bottom: 16px;
}

.card-header i {
  font-size: 24px;
  color: #28a745;
}

.payment-amount {
  display: flex;
  align-items: baseline;
  justify-content: center;
  gap: 4px;
  margin-bottom: 16px;
}

.amount {
  font-size: 32px;
  font-weight: bold;
  color: #007bff;
}

.currency {
  font-size: 16px;
  color: #6c757d;
}

.mobile-order-summary {
  background: rgba(255,255,255,0.1);
  border-radius: 12px;
  overflow: hidden;
}

.summary-toggle {
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
}

.summary-details {
  border-top: 1px solid rgba(255,255,255,0.2);
  padding: 16px;
}

.mobile-order-line {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
}

.mobile-sca-footer {
  padding: 16px;
  text-align: center;
}

.mobile-security-notice {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
  opacity: 0.9;
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-card` | Main container card |
| `_c-stripe-sca` | SCA-specific styling |
| `_c-cart-editor` | Cart editor layout |
| `_c-header` | Header section |
| `_c-divider` | Visual divider |
| `_c-title` | Title styling |
| `_c-content` | Main content area |
| `_c-grow` | Flex grow styling |
| `_c-justify-center` | Center justified content |
| `_c-success-container` | Success state container |
| `_c-success-icon` | Success icon styling |
| `_c-success-title` | Success title styling |
| `_c-subtitle` | Subtitle styling |
| `_c-success-desc` | Success description |
| `_c-btn-container` | Button container |
| `_c-fill` | Fill available space |
| `_c-btn` | Button styling |
| `_c-primary-btn` | Primary button |
| `_c-secondary-btn` | Secondary button |
| `_c-invoice-summary` | Invoice summary section |
| `_c-cart-card` | Cart-style card |
| `_c-line-items` | Line items container |
| `_c-footer` | Footer section |
| `_c-total` | Total section |
| `_c-content-column` | Column content layout |
| `_c-price-container` | Price display container |
| `_c-text-bold` | Bold text |
| `_c-flex` | Flexbox layout |
| `_c-justify-between` | Space between flex items |
| `_c-discounts` | Discounts styling |
| `_c-taxes` | Tax information styling |
| `_c-text-sm` | Small text size |

## Best Practices

### Authentication Flow
- Always verify customer authentication before allowing payment confirmation
- Provide clear messaging about why authentication is required
- Handle both authenticated and unauthenticated states appropriately
- Implement proper error handling for authentication failures

### Security
- Never store or log sensitive payment information
- Use Stripe's secure confirmation methods
- Implement proper HTTPS and CSP headers
- Follow PCI compliance guidelines

### User Experience
- Show clear progress indicators during verification
- Provide helpful context about why verification is needed
- Display order information during the authentication process
- Offer clear next steps after successful authentication

### Error Handling
- Handle authentication failures gracefully
- Provide retry mechanisms for failed confirmations
- Show user-friendly error messages
- Log errors for debugging and support

### Mobile Optimization
- Use responsive design for authentication interfaces
- Implement touch-friendly controls
- Consider mobile-specific authentication flows
- Optimize for various screen sizes

### Accessibility
- Provide screen reader announcements for state changes
- Use appropriate ARIA labels for authentication steps
- Ensure keyboard navigation works properly
- Test with assistive technologies

### Performance
- Load authentication resources efficiently
- Minimize blocking operations during confirmation
- Implement appropriate loading states
- Handle slow network connections gracefully

## Component Registration
```javascript
// Global registration
app.component('CodexStripeConfirmSCA', StripeConfirmSCA)

// Local registration  
import StripeConfirmSCA from '@/components/Cart/StripeConfirmSCA.vue'

export default {
  components: {
    CodexStripeConfirmSCA: StripeConfirmSCA
  }
} 