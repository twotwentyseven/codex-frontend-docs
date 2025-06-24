# Order Component

## Overview
The Order component provides a comprehensive interface for displaying and managing order details with payment status handling, collapsible cart summaries, and integrated payment processing. It features status-dependent displays, automatic payment method selection, customer authentication integration, and support for pending payment resolution through Stripe. The component manages the complete order workflow from display to payment completion.

## Basic Usage
```vue
<template>
  <div class="order-management">
    <codex-order
      :order="orderData"
      :cart="cartData"
      @clearOrder="handleClearOrder"
      @cartFinalized="handleOrderComplete"
      @paymentProcessing="handlePaymentProcessing"
      @intentStatusChanged="handleIntentStatus"
    >
      <template #header>
        <div class="custom-order-header">
          <h2>Order #{{ orderData.id }}</h2>
          <div class="order-meta">
            <span>Order Date: {{ formatDate(orderData.created_at) }}</span>
            <span class="status-badge" :class="orderData.status.toLowerCase()">
              {{ orderData.status }}
            </span>
          </div>
        </div>
      </template>
      
      <template #footer>
        <div class="order-actions">
          <button @click="downloadInvoice">Download Invoice</button>
          <button @click="contactSupport">Contact Support</button>
        </div>
      </template>
    </codex-order>
  </div>
</template>

<script setup>
const orderData = ref({
  id: 'ORD-2024-001',
  status: 'Payment pending',
  created_at: '2024-01-15',
  invoices: [
    {
      order_lines: [
        {
          product: 'Premium Plan',
          final_price: '$29.99',
          quantity: 1
        }
      ]
    }
  ]
})

const cartData = ref({
  lines: [],
  price_raw: 2999
})

const handleClearOrder = () => {
  console.log('Order cleared')
}

const handleOrderComplete = () => {
  console.log('Order completed successfully')
}

const handlePaymentProcessing = (processing) => {
  console.log('Payment processing:', processing)
}

const handleIntentStatus = (status) => {
  console.log('Intent status changed:', status)
}
</script>
```

## Key Features
- Order status-dependent display and controls
- Collapsible cart summary with line items
- Integrated payment processing for pending orders
- Customer authentication state management
- Real-time payment intent status monitoring
- Support for multiple order statuses (Paid, Refunded, Cancelled, etc.)
- Automatic payment method selection and retry
- Error handling with user-friendly messages

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `order` | `Object/Boolean` | `required` | Order data object containing status, invoices, and payment information |
| `cart` | `Object/Boolean` | `required` | Cart data object for payment processing context |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `clearOrder` | `none` | Emitted when user wants to clear/close the order |
| `cartFinalized` | `orderData` | Emitted when order payment is successfully completed |
| `paymentProcessing` | `processingState` | Emitted when payment processing status changes |
| `intentStatusChanged` | `status` | Emitted when payment intent status changes |

## Slots

| Slot | Description |
|------|-------------|
| `header` | Custom header content above order details |
| `content` | Complete content replacement for order display |
| `footer` | Custom footer content below order information |

## Order Status Types

The component handles different order statuses with appropriate displays:

| Status | Display | Available Actions |
|--------|---------|-------------------|
| `Paid` | Success confirmation | View receipt, download invoice |
| `Refunded` | Refund notification | Contact support |
| `Partially refunded` | Partial refund info | View details, contact support |
| `Payment pending` | Payment interface | Complete payment, clear order |
| `Cancelled` | Cancellation notice | View details |

## Order Data Structure

### Basic Order
```javascript
{
  id: 'ORD-2024-001',
  status: 'Paid',
  created_at: '2024-01-15',
  final_price_raw: 2999,
  invoices: [
    {
      order_lines: [
        {
          product: 'Product Name',
          final_price: '$29.99',
          quantity: 1
        }
      ]
    }
  ]
}
```

### Pending Payment Order
```javascript
{
  id: 'ORD-2024-002',
  status: 'Payment pending',
  final_price_raw: 2999,
  payment_method_summary: {
    paymentMethodGiftCard0: {
      name: 'Gift Card',
      paymentSource: {
        balance: '$10.00',
        last4: '1234'
      },
      amountRaw: 1000
    }
  },
  invoices: [/* invoice data */]
}
```

## Internationalization

The component uses the following translation keys:

### Order Status Display
| Key | Usage |
|-----|-------|
| `cart.order_fully_paid` | Order status when fully paid |
| `cart.order_refunded` | Order status when refunded |
| `cart.order_partially_refunded` | Order status when partially refunded |
| `cart.payment_pending` | Order status when payment is pending |
| `cart.order_cancelled_title` | Order status when cancelled |

### Cart Summary Controls
| Key | Usage |
|-----|-------|
| `cart.hide_cart_summary` | Hide cart summary button text |
| `cart.show_cart_summary` | Show cart summary button text |

### Payment Error Messages
| Key | Usage |
|-----|-------|
| `cart.payment_failed` | Payment failed title |
| `cart.payment_failed_choose_alt` | Payment failed description |
| `cart.fail_close_order` | Close order button after payment failure |

### Translation Usage Examples
```vue
<!-- Order status header -->
<div class="_c-title" v-if="order.status=='Paid'">
  {{ $t('cart.order_fully_paid') }}
</div>

<div class="_c-title" v-if="order.status=='Refunded'">
  {{ $t('cart.order_refunded') }}
</div>

<div class="_c-title" v-if="order.status=='Payment pending'">
  {{ $t('cart.payment_pending') }}
</div>

<!-- Cart summary toggle -->
<button @click="toggleCartSummary">
  <template v-if="showCartSummary">{{ $t('cart.hide_cart_summary') }}</template>
  <template v-else>{{ $t('cart.show_cart_summary') }}</template>
</button>

<!-- Payment error display -->
<div class="_c-error-container">
  <codex-title :content="$t('cart.payment_failed')" />
  <codex-paragraph :content="$t('cart.payment_failed_choose_alt')" />
  <button @click="clearOrder">{{ $t('cart.fail_close_order') }}</button>
</div>
```

### Translation Notes

#### Status-Dependent Translations
The Order component displays different translation keys based on the order status:
- Uses conditional rendering with `v-if` to show appropriate status messages
- Each status (Paid, Refunded, Partially refunded, Payment pending, Cancelled) has specific translation keys
- Status translations should be clear and match business terminology

#### Payment Flow Integration
The component integrates with payment processing components that have their own translation keys:
- Works with `codex-checkout-payment-methods` for payment interface
- Supports error handling with payment-specific translations
- Provides cart summary functionality with toggle controls

#### Child Component Translations
The Order component orchestrates several child components with their own translation requirements:
- `codex-order-line` components handle line item translations
- `codex-order-totals` components handle pricing translations
- Payment method components handle payment-specific translations

## Usage Examples

### Basic Order Display
```vue
<codex-order :order="order" :cart="cart">
  <template #header>
    <h1>{{ $t('order.title') }}</h1>
    <p>{{ $t('order.order_number', { number: order.number }) }}</p>
  </template>
</codex-order>
```

### With Custom Success State
```vue
<codex-order :order="order" :cart="cart">
  <template #footer v-if="order.status === 'paid'">
    <div class="custom-success">
      <h3>{{ $t('order.success.title') }}</h3>
      <p>{{ $t('order.success.message') }}</p>
      <codex-button 
        :default-text="$t('order.navigation.continue_shopping')"
        @click="continueShopping"
      />
    </div>
  </template>
</codex-order>
```

### With Error Handling
```vue
<codex-order 
  :order="order" 
  :cart="cart"
  @error="handleOrderError"
>
  <template #error="{ error }">
    <div class="order-error">
      <h3>{{ $t('order.error.title') }}</h3>
      <p>{{ $t('order.error.message') }}</p>
      <codex-button 
        :default-text="$t('order.error.try_again')"
        @click="retryOrder"
      />
    </div>
  </template>
</codex-order>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-order` | Main order container |
| `codex` | Base component styling |
| `_c-header` | Header section |
| `_c-divider` | Divider line styling |
| `_c-title` | Title styling |
| `_c-content` | Main content area |
| `_c-order-content` | Order-specific content styling |
| `_c-cart-summary-toggle` | Cart summary toggle button |
| `_c-accordion-toggle` | Accordion toggle styling |
| `_c-collapsed` | Collapsed state styling |
| `_c-cart-summary` | Cart summary container |
| `_c-accordion` | Accordion wrapper |
| `_c-accordion-inner` | Accordion inner content |
| `_c-card` | Card container |
| `_c-cart-card` | Cart-style card layout |
| `_c-line-items` | Line items container |
| `_c-error-container` | Error state container |
| `_c-error-icon` | Error icon styling |
| `_c-error-title` | Error title styling |
| `_c-error-desc` | Error description styling |
| `_c-payment-wrapper` | Payment methods wrapper |
| `_c-footer` | Footer section |

## Best Practices

### Order Status Management
- Display appropriate interfaces based on order status
- Provide clear next steps for pending payments
- Handle payment failures gracefully with retry options
- Show relevant actions for each order state

### Payment Processing
- Use unified payment handling for consistency
- Implement proper error states and recovery
- Provide clear feedback during payment processing
- Handle authentication requirements appropriately

### User Experience
- Show order summaries that can be collapsed on mobile
- Provide clear navigation back to shopping or account
- Display relevant order actions based on status
- Use appropriate loading states during operations

### Error Handling
- Display specific error messages for payment failures
- Provide alternative payment options when needed
- Handle network failures gracefully
- Log errors for debugging and support

### Mobile Optimization
- Use collapsible interfaces to save screen space
- Implement touch-friendly controls and navigation
- Consider one-handed operation patterns
- Optimize for various screen sizes

### Security
- Never expose sensitive payment information
- Use secure communication for all operations
- Implement proper authentication checks
- Follow PCI compliance guidelines

### Accessibility
- Provide screen reader announcements for status changes
- Use appropriate ARIA labels for interactive elements
- Ensure keyboard navigation works properly
- Test with assistive technologies

## Component Registration
```javascript
// Global registration
app.component('CodexOrder', Order)

// Local registration  
import Order from '@/components/Cart/Order.vue'

export default {
  components: {
    CodexOrder: Order
  }
}
``` 