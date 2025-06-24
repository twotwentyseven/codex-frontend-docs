# StripeManagePaymentMethods Component

## Overview
The StripeManagePaymentMethods component provides a comprehensive payment method management interface for customers to view, add, set default, and remove saved payment methods. It features visual card displays with brand recognition, default payment method management, secure payment method addition through Stripe Elements, and streamlined user experience for payment method organization. The component integrates seamlessly with Stripe's setup intents for secure payment method storage.

## Basic Usage
```vue
<template>
  <div class="payment-methods-manager">
    <codex-stripe-manage-payment-methods
      :title="'Payment Methods'"
      :titleTag="'h2'"
      @paymentMethodAdded="handleMethodAdded"
      @paymentMethodRemoved="handleMethodRemoved"
      @defaultMethodChanged="handleDefaultChanged"
    />
  </div>
</template>

<script setup>
const handleMethodAdded = () => {
  console.log('New payment method added')
}

const handleMethodRemoved = () => {
  console.log('Payment method removed')
}

const handleDefaultChanged = () => {
  console.log('Default payment method changed')
}
</script>
```

## Key Features
- Visual payment method cards with brand icons
- Add new payment methods through Stripe Elements
- Set and manage default payment methods
- Remove unused payment methods
- Skeleton loading states during data fetching
- Card holder name display with customer information
- Expiry date validation and display
- Comprehensive error handling and user feedback

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `title` | `String/Boolean` | `false` | Component title display |
| `titleTag` | `String` | `'h2'` | HTML tag for title element |
| `stripeOptions` | `Object` | `{}` | Custom Stripe configuration options |
| `stripeAppearance` | `Object` | Default theme | Stripe Elements appearance customization |

## Common Props

| Prop Name | Usage |
|-----------|-------|
| `commonProps` | Standard component props for consistency |

## Slots

| Slot | Description |
|------|-------------|
| `header` | Custom header content with title and add button |
| `content` | Complete content replacement for payment method management |
| `error-messages` | Custom error message display |
| `footer` | Custom footer content |

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

### Customer Information
```javascript
{
  first_name: 'John',
  last_name: 'Doe',
  email: 'john@example.com'
}
```

## Internationalization Keys

| Key | Default | Description |
|-----|---------|-------------|
| `card.add_new_payment_method` | "Add new payment method" | Add payment method button |
| `card.card_holder` | "Card Holder" | Card holder label |
| `card.expiry` | "Expiry" | Card expiry label |
| `card.set_as_default` | "Set as default" | Set default button |
| `card.default` | "Default" | Default method indicator |
| `card.forget_this_card` | "Forget this card" | Remove card button |
| `account.save_card` | "Save Card" | Save payment method button |

## Examples

### Customer Account Payment Methods
```vue
<template>
  <div class="account-payment-methods">
    <div class="account-section">
      <codex-stripe-manage-payment-methods
        :title="'My Payment Methods'"
        :titleTag="'h3'"
        :stripeAppearance="customAppearance"
        @paymentMethodAdded="refreshPaymentMethods"
        @paymentMethodRemoved="handleMethodRemoval"
        @defaultMethodChanged="updateDefaultMethod"
      >
        <template #header="{ title, titleTag }">
          <div class="payment-methods-header">
            <div class="header-content">
              <component :is="titleTag" class="section-title">{{ title }}</component>
              <p class="section-description">
                Manage your saved payment methods for faster checkout
              </p>
            </div>
            <div class="header-actions">
              <button @click="showPaymentTips" class="info-btn">
                <i class="ri-information-line"></i>
                Payment Security
              </button>
            </div>
          </div>
        </template>
        
        <template #footer>
          <div class="payment-methods-footer">
            <div class="security-info">
              <div class="security-item">
                <i class="ri-shield-check-line"></i>
                <span>Your payment information is encrypted and secure</span>
              </div>
              <div class="security-item">
                <i class="ri-lock-line"></i>
                <span>We never store your full card number</span>
              </div>
            </div>
            
            <div class="help-links">
              <a href="/help/payment-methods" target="_blank">
                <i class="ri-question-line"></i>
                Payment Methods Help
              </a>
              <a href="/privacy" target="_blank">
                <i class="ri-file-text-line"></i>
                Privacy Policy
              </a>
            </div>
          </div>
        </template>
      </codex-stripe-manage-payment-methods>
    </div>
  </div>
</template>

<script setup>
const customAppearance = {
  theme: 'stripe',
  variables: {
    fontFamily: 'Inter, system-ui, sans-serif',
    fontLineHeight: '1.5',
    borderRadius: '8px',
    colorPrimary: '#635bff',
    colorBackground: '#ffffff',
    colorText: '#30313d',
    colorDanger: '#df1b41'
  }
}

const refreshPaymentMethods = () => {
  console.log('Payment method added successfully')
  
  // Show success notification
  toast.success('Payment method added successfully!')
  
  // Track analytics
  analytics.track('payment_method_added', {
    timestamp: new Date(),
    source: 'account_settings'
  })
}

const handleMethodRemoval = () => {
  console.log('Payment method removed')
  
  // Show removal notification
  toast.info('Payment method removed')
  
  // Track analytics
  analytics.track('payment_method_removed', {
    timestamp: new Date()
  })
}

const updateDefaultMethod = () => {
  console.log('Default payment method updated')
  
  // Show success notification
  toast.success('Default payment method updated')
  
  // Track analytics
  analytics.track('default_payment_method_changed', {
    timestamp: new Date()
  })
}

const showPaymentTips = () => {
  // Show payment security tips modal
  modal.open('payment-security-tips')
}
</script>
```

### Subscription Payment Methods
```vue
<template>
  <div class="subscription-payment-methods">
    <div class="subscription-billing-section">
      <h2>Billing & Payment Methods</h2>
      
      <div class="current-subscription">
        <div class="subscription-details">
          <h3>{{ currentPlan.name }}</h3>
          <div class="plan-price">{{ currentPlan.price }}/{{ currentPlan.period }}</div>
          <div class="next-billing">
            Next billing: {{ formatDate(nextBillingDate) }}
          </div>
        </div>
        
        <div v-if="activePaymentMethod" class="active-payment">
          <h4>Current Payment Method</h4>
          <div class="payment-method-preview">
            <i :class="getCardIcon(activePaymentMethod.brand)"></i>
            <span>{{ activePaymentMethod.brand }} •••• {{ activePaymentMethod.last4 }}</span>
            <span class="expires">Expires {{ activePaymentMethod.exp_month }}/{{ activePaymentMethod.exp_year }}</span>
          </div>
        </div>
      </div>
      
      <codex-stripe-manage-payment-methods
        :title="'Payment Methods'"
        @paymentMethodAdded="handleSubscriptionMethodAdded"
        @defaultMethodChanged="updateSubscriptionPayment"
      >
        <template #header="{ title }">
          <div class="subscription-payment-header">
            <h3>{{ title }}</h3>
            <div class="billing-notice">
              <i class="ri-information-line"></i>
              <span>Your default payment method will be used for subscription billing</span>
            </div>
          </div>
        </template>
      </codex-stripe-manage-payment-methods>
      
      <div class="billing-history">
        <h3>Billing History</h3>
        <div class="history-list">
          <div v-for="invoice in recentInvoices" :key="invoice.id" class="history-item">
            <div class="invoice-date">{{ formatDate(invoice.date) }}</div>
            <div class="invoice-amount">{{ invoice.amount }}</div>
            <div class="invoice-status" :class="invoice.status">{{ invoice.status }}</div>
            <a :href="invoice.pdf_url" target="_blank" class="download-link">
              <i class="ri-download-line"></i>
              Download
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const currentPlan = ref({
  name: 'Premium Plan',
  price: '$29.99',
  period: 'monthly'
})

const nextBillingDate = ref('2024-02-15')

const activePaymentMethod = ref({
  brand: 'visa',
  last4: '4242',
  exp_month: 12,
  exp_year: 2025
})

const recentInvoices = ref([
  {
    id: 1,
    date: '2024-01-15',
    amount: '$29.99',
    status: 'paid',
    pdf_url: '/invoices/inv_2024_001.pdf'
  },
  {
    id: 2,
    date: '2023-12-15',
    amount: '$29.99',
    status: 'paid',
    pdf_url: '/invoices/inv_2023_012.pdf'
  }
])

const handleSubscriptionMethodAdded = () => {
  console.log('New payment method added for subscription')
  
  // Suggest setting as default for billing
  modal.confirm({
    title: 'Set as Default Payment Method?',
    message: 'Would you like to use this payment method for your subscription billing?',
    confirmText: 'Yes, Set as Default',
    onConfirm: () => {
      updateSubscriptionPayment()
    }
  })
}

const updateSubscriptionPayment = () => {
  console.log('Subscription payment method updated')
  
  // Update subscription with new default payment method
  updateSubscriptionBilling()
  
  toast.success('Subscription payment method updated')
}

const getCardIcon = (brand) => {
  const icons = {
    'visa': 'ri-visa-line',
    'mastercard': 'ri-mastercard-line',
    'amex': 'ri-paypal-line', // Using PayPal as placeholder
    'discover': 'ri-paypal-line'
  }
  return icons[brand] || 'ri-bank-card-line'
}

const formatDate = (date) => {
  return new Date(date).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}
</script>
```

### Mobile Payment Methods Management
```vue
<template>
  <div class="mobile-payment-methods">
    <div class="mobile-header">
      <button @click="goBack" class="back-btn">
        <i class="ri-arrow-left-line"></i>
      </button>
      <h2>Payment Methods</h2>
      <button @click="showHelp" class="help-btn">
        <i class="ri-question-line"></i>
      </button>
    </div>
    
    <div class="mobile-content">
      <codex-stripe-manage-payment-methods
        :title="false"
        class="mobile-payment-manager"
        @paymentMethodAdded="handleMobileMethodAdded"
        @paymentMethodRemoved="handleMobileMethodRemoved"
      >
        <template #content>
          <div class="mobile-payment-content">
            <div v-if="loading" class="mobile-loading">
              <div v-for="i in 3" :key="i" class="mobile-card-skeleton">
                <div class="skeleton-header"></div>
                <div class="skeleton-content"></div>
                <div class="skeleton-footer"></div>
              </div>
            </div>
            
            <div v-else class="mobile-payment-cards">
              <div v-for="(method, index) in paymentMethods" :key="method.id" class="mobile-payment-card">
                <div class="card-header">
                  <div class="card-brand">
                    <i :class="getCardIcon(method.brand)"></i>
                    <span>{{ method.brand.toUpperCase() }}</span>
                  </div>
                  <div v-if="method.default" class="default-badge">Default</div>
                </div>
                
                <div class="card-number">•••• •••• •••• {{ method.last4 }}</div>
                
                <div class="card-details">
                  <div class="card-holder">{{ customerName }}</div>
                  <div class="card-expiry">{{ method.exp_month }}/{{ method.exp_year }}</div>
                </div>
                
                <div class="card-actions">
                  <button 
                    v-if="!method.default" 
                    @click="setMobileDefault(method.id)"
                    class="mobile-action-btn primary"
                  >
                    Set as Default
                  </button>
                  
                  <button 
                    v-if="paymentMethods.length > 1"
                    @click="removeMobileMethod(method.id)"
                    class="mobile-action-btn danger"
                  >
                    Remove
                  </button>
                </div>
              </div>
              
              <button @click="addMobilePaymentMethod" class="add-mobile-method">
                <i class="ri-add-line"></i>
                <span>Add New Payment Method</span>
              </button>
            </div>
          </div>
        </template>
      </codex-stripe-manage-payment-methods>
    </div>
  </div>
</template>

<script setup>
const loading = ref(true)
const customerName = ref('John Doe')

const paymentMethods = ref([
  {
    id: 'pm_1',
    brand: 'visa',
    last4: '4242',
    exp_month: 12,
    exp_year: 2025,
    default: true
  },
  {
    id: 'pm_2', 
    brand: 'mastercard',
    last4: '5555',
    exp_month: 8,
    exp_year: 2026,
    default: false
  }
])

const handleMobileMethodAdded = () => {
  console.log('Payment method added on mobile')
  
  // Show mobile success toast
  showMobileToast('Payment method added successfully')
  
  // Haptic feedback
  if (navigator.vibrate) {
    navigator.vibrate([100])
  }
}

const handleMobileMethodRemoved = () => {
  console.log('Payment method removed on mobile')
  showMobileToast('Payment method removed')
}

const setMobileDefault = async (methodId) => {
  try {
    // Show loading state
    showMobileLoading('Setting as default...')
    
    // API call to set default
    await setDefaultPaymentMethod(methodId)
    
    // Update local state
    paymentMethods.value.forEach(method => {
      method.default = method.id === methodId
    })
    
    showMobileToast('Default payment method updated')
  } catch (error) {
    showMobileToast('Failed to update default method', 'error')
  } finally {
    hideMobileLoading()
  }
}

const removeMobileMethod = async (methodId) => {
  // Show confirmation
  const confirmed = await showMobileConfirmation(
    'Remove Payment Method',
    'Are you sure you want to remove this payment method?'
  )
  
  if (confirmed) {
    try {
      showMobileLoading('Removing payment method...')
      
      await removePaymentMethod(methodId)
      
      // Remove from local state
      paymentMethods.value = paymentMethods.value.filter(method => method.id !== methodId)
      
      showMobileToast('Payment method removed')
    } catch (error) {
      showMobileToast('Failed to remove payment method', 'error')
    } finally {
      hideMobileLoading()
    }
  }
}

const addMobilePaymentMethod = () => {
  router.push('/mobile/payment-methods/add')
}

const goBack = () => {
  router.go(-1)
}

const showHelp = () => {
  router.push('/mobile/help/payment-methods')
}

const getCardIcon = (brand) => {
  const icons = {
    'visa': 'ri-visa-line',
    'mastercard': 'ri-mastercard-line',
    'amex': 'ri-paypal-line',
    'discover': 'ri-paypal-line'
  }
  return icons[brand] || 'ri-bank-card-line'
}

// Mobile utility functions
const showMobileToast = (message, type = 'success') => {
  toast[type](message, {
    position: 'bottom-center',
    duration: 3000
  })
}

const showMobileLoading = (message) => {
  // Show mobile loading overlay
  loading.value = true
}

const hideMobileLoading = () => {
  loading.value = false
}

const showMobileConfirmation = (title, message) => {
  return new Promise((resolve) => {
    const confirmed = confirm(`${title}\n\n${message}`)
    resolve(confirmed)
  })
}

onMounted(() => {
  // Load payment methods
  setTimeout(() => {
    loading.value = false
  }, 1000)
})
</script>

<style scoped>
.mobile-payment-methods {
  min-height: 100vh;
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

.back-btn, .help-btn {
  background: none;
  border: none;
  font-size: 20px;
  padding: 8px;
}

.mobile-content {
  padding: 16px;
}

.mobile-payment-card {
  background: white;
  border-radius: 12px;
  padding: 16px;
  margin-bottom: 16px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.card-brand {
  display: flex;
  align-items: center;
  gap: 8px;
  font-weight: bold;
}

.default-badge {
  background: #28a745;
  color: white;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
}

.card-number {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 12px;
  letter-spacing: 1px;
}

.card-details {
  display: flex;
  justify-content: space-between;
  margin-bottom: 16px;
  color: #6c757d;
  font-size: 14px;
}

.card-actions {
  display: flex;
  gap: 8px;
}

.mobile-action-btn {
  flex: 1;
  padding: 8px 16px;
  border: none;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 500;
}

.mobile-action-btn.primary {
  background: #007bff;
  color: white;
}

.mobile-action-btn.danger {
  background: #dc3545;
  color: white;
}

.add-mobile-method {
  width: 100%;
  background: white;
  border: 2px dashed #dee2e6;
  border-radius: 12px;
  padding: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  color: #6c757d;
  font-weight: 500;
}

.mobile-card-skeleton {
  background: white;
  border-radius: 12px;
  padding: 16px;
  margin-bottom: 16px;
}

.skeleton-header,
.skeleton-content,
.skeleton-footer {
  height: 16px;
  background: #e9ecef;
  border-radius: 4px;
  margin-bottom: 8px;
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-card` | Main card container |
| `_c-payment-methods-card` | Payment methods specific card styling |
| `_c-header` | Header section |
| `_c-content` | Main content area |
| `_c-footer` | Footer section |
| `_c-payment-methods-list` | Payment methods grid container |
| `_c-grid` | Grid layout |
| `_c-payment-card` | Individual payment method card |
| `_c-default` | Default payment method styling |
| `_c-card-icon` | Card brand icon |
| `_c-card-brand` | Card brand styling |
| `_c-card-number` | Card number display |
| `_c-card-holder` | Card holder section |
| `_c-card-expiry` | Card expiry section |
| `_c-btn-container` | Button container |
| `_c-column` | Column layout |
| `_c-gap-lg` | Large gap spacing |
| `_c-remove` | Remove button styling |
| `_c-link` | Link styling |

## Best Practices

### Payment Method Management
- Display payment methods in an easily scannable grid layout
- Show clear visual indicators for default payment methods
- Provide confirmation dialogs for destructive actions (remove)
- Use appropriate card brand icons for recognition

### Security
- Never display full card numbers - only show last 4 digits
- Use secure Stripe Elements for adding new payment methods
- Implement proper authentication before allowing changes
- Follow PCI compliance guidelines for payment data handling

### User Experience
- Show loading states during payment method operations
- Provide clear feedback for successful operations
- Use skeleton loading for initial data loading
- Implement proper error handling with user-friendly messages

### Accessibility
- Use semantic HTML for payment method information
- Provide screen reader announcements for state changes
- Ensure sufficient color contrast for all elements
- Make all interactive elements keyboard accessible

### Mobile Optimization
- Use touch-friendly interface elements
- Implement swipe gestures for card management
- Optimize for one-handed operation
- Use haptic feedback for confirmations

### Performance
- Load payment methods efficiently on component mount
- Cache payment method data appropriately
- Minimize re-renders during operations
- Implement progressive loading for large lists

### Error Handling
- Display specific error messages for different failure types
- Provide retry mechanisms for transient failures
- Log errors for debugging and support purposes
- Handle network connectivity issues gracefully

## Component Registration
```javascript
// Global registration
app.component('CodexStripeManagePaymentMethods', StripeManagePaymentMethods)

// Local registration  
import StripeManagePaymentMethods from '@/components/Cart/StripeManagePaymentMethods.vue'

export default {
  components: {
    CodexStripeManagePaymentMethods: StripeManagePaymentMethods
  }
} 