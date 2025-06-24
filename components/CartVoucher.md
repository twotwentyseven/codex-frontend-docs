# CartVoucher Component

## Overview
The CartVoucher component provides a comprehensive interface for managing discount vouchers and gift cards in the shopping cart. It features automatic URL voucher detection, scroll-based visibility management, real-time validation, and support for multiple payment methods including gift cards. The component handles voucher application, removal, and displays active vouchers with appropriate feedback states.

## Basic Usage
```vue
<template>
  <div class="cart-voucher-section">
    <codex-cart-voucher
      :cart="cartData"
      :readonly="false"
      @applyVoucher="handleVoucherApply"
      @clearVoucher="handleVoucherClear"
      @clearPaymentMethod="handlePaymentMethodClear"
    >
      <template #error-messages="{ genericErrors }">
        <div class="custom-error-display">
          <span>{{ genericErrors }}</span>
        </div>
      </template>
    </codex-cart-voucher>
  </div>
</template>

<script setup>
const cartData = ref({
  voucher: {
    code: 'SAVE20',
    amount: '$10.00'
  },
  payment_methods: {
    giftcard: {
      name: 'Gift Card',
      paymentSource: {
        balance: '$25.00',
        last4: '1234'
      }
    }
  }
})

const handleVoucherApply = (code) => {
  console.log('Voucher applied:', code)
}

const handleVoucherClear = () => {
  console.log('Voucher cleared')
}

const handlePaymentMethodClear = (key) => {
  console.log('Payment method cleared:', key)
}
</script>
```

## Key Features
- Voucher code input with real-time validation
- Automatic URL voucher detection and application
- Gift card and payment method management
- Scroll-based visibility control with smooth animations
- Real-time success and error feedback
- Read-only mode for order confirmation
- Internationalized labels and messages
- Comprehensive error handling with user feedback
- Integration with useCart composition API

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `cart` | `Object` | `required` | Cart data object containing vouchers and payment methods |
| `readonly` | `Boolean` | `false` | Whether to show controls or read-only display |

## Common Props

| Prop Name | Usage |
|-----------|-------|
| `commonProps` | Standard component props for consistency |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `applyVoucher` | `voucherCode` | Emitted when a voucher is successfully applied |
| `clearVoucher` | `none` | Emitted when active voucher is removed |
| `clearPaymentMethod` | `paymentMethodKey` | Emitted when gift card/payment method is removed |

## Slots

| Slot | Description |
|------|-------------|
| `error-messages` | Custom error message display with genericErrors data |

### Slot Props

| Prop | Type | Description |
|------|------|-------------|
| `genericErrors` | `String` | Error messages from voucher operations |

## States

### Default State (No Voucher)
```vue
<codex-cart-voucher 
  :cart="emptyCart"
  :readonly="false"
/>
```

### With Active Voucher
```vue
<codex-cart-voucher 
  :cart="cartWithVoucher"
  :readonly="false"
/>
```

### With Gift Cards
```vue
<codex-cart-voucher 
  :cart="cartWithGiftCards"
  :readonly="false"
/>
```

### Read-only Mode
```vue
<codex-cart-voucher 
  :cart="cart"
  :readonly="true"
/>
```

### Collapsed State
```vue
<!-- Automatically collapses based on scroll direction -->
<codex-cart-voucher 
  :cart="cart"
  class="_c-collapsed"
/>
```

## Cart Data Structure

### Basic Cart with Voucher
```javascript
{
  voucher: {
    code: 'SUMMER20',
    amount: '$15.00',
    description: '20% off summer items'
  }
}
```

### Cart with Gift Card
```javascript
{
  payment_methods: {
    'giftcard_abc123': {
      name: 'Gift Card',
      paymentSource: {
        balance: '$50.00',
        last4: '5678'
      }
    }
  }
}
```

### Cart with Multiple Payment Methods
```javascript
{
  voucher: {
    code: 'WELCOME10'
  },
  payment_methods: {
    'giftcard_1': {
      name: 'Gift Card',
      paymentSource: {
        balance: '$25.00',
        last4: '1234'
      }
    },
    'store_credit': {
      name: 'Store Credit',
      paymentSource: {
        balance: '$15.00',
        last4: '9876'
      }
    }
  }
}
```

## Internationalization

The component uses the following translation keys:

### Voucher Form Interface
| Key | Usage |
|-----|-------|
| `cart.voucher_label` | Voucher input field label |
| `cart.enter_voucher_code` | Voucher input placeholder text |
| `cart.applying` | Applying voucher processing text |
| `cart.apply` | Apply voucher button text |
| `cart.applied` | Success state button text |

### Voucher Management
| Key | Usage |
|-----|-------|
| `cart.clear_voucher` | Clear voucher button aria-label |
| `cart.clear_gift_card` | Clear gift card link text |

### Translation Usage Examples
```vue
<!-- Voucher form -->
<form class="_c-voucher-form" @submit.prevent="submitVoucher">
  <codex-input-field
    type="text"
    name="voucher"
    id="voucher"
    :label="t('cart.voucher_label')"
    :placeholder="t('cart.enter_voucher_code')"
    v-model="voucherCode"
  />
  
  <codex-button 
    class="_c-btn-voucher"
    :processing-text="t('cart.applying')" 
    :default-text="t('cart.apply')"
    :disabled-text="t('cart.apply')"
    :success-text="t('cart.applied')"
    :processing="loading" 
    :success="success"
    :disabled="loading || (!voucherCode)" 
    @click="submitVoucher"
  />
</form>

<!-- Active voucher display -->
<div class="_c-active-vouchers _c-badge" v-if="cart.voucher">
  <i class="ri-price-tag-3-line _c-text-icon-sm"></i>
  <span class="_c-voucher-name">{{ cart.voucher.code }}</span>
  
  <button 
    v-if="!readonly" 
    class="_c-voucher-remove" 
    @click.prevent="clearVoucher" 
    :aria-label="t('cart.clear_voucher')"
  >
    <i class="ri-close-fill _c-text-icon-sm"></i>
  </button>
</div>

<!-- Gift card payment method -->
<div class="_c-active-vouchers" v-for="(paymentMethod, key) in cart.payment_methods" :key="paymentMethod.name">
  <svg><!-- Gift card icon --></svg>
  <span class="_c-voucher-name">
    Balance {{ paymentMethod?.paymentSource?.balance }} (ending in {{ paymentMethod?.paymentSource?.last4 }})
  </span>
  <a v-if="!readonly" class="_c-remove-icon" @click="clearPaymentMethod(key)">
    {{ t('cart.clear_gift_card') }}
  </a>
</div>

## Usage Examples

### Basic Voucher Input
```vue
<codex-cart-voucher :cart="cart">
  <template #header>
    <h3>{{ $t('voucher.input.label') }}</h3>
  </template>
</codex-cart-voucher>
```

### With Gift Card Support
```vue
<codex-cart-voucher 
  :cart="cart"
  :enable-gift-cards="true"
  @gift-card-applied="handleGiftCardApplied"
>
  <template #gift-card-section>
    <div class="custom-gift-card">
      <h4>{{ $t('voucher.gift_card.title') }}</h4>
      <!-- Custom gift card interface -->
    </div>
  </template>
</codex-cart-voucher>
```

### With Error Handling
```vue
<codex-cart-voucher 
  :cart="cart"
  @voucher-error="handleVoucherError"
>
  <template #error-messages="{ errors }">
    <div class="custom-errors">
      <h4>{{ $t('voucher.errors.title') }}</h4>
      <div v-for="error in errors" :key="error.type">
        {{ $t(`voucher.errors.${error.type}`) }}
      </div>
    </div>
  </template>
</codex-cart-voucher>
```

## Scroll Behavior

The component features intelligent scroll-based visibility:

### Scroll States
```vue
<!-- Expanded when scrolling up -->
<codex-cart-voucher :cart="cart" />

<!-- Collapsed when scrolling down -->
<codex-cart-voucher :cart="cart" class="_c-collapsed" />
```

### Manual Toggle
```vue
<!-- User can manually toggle visibility -->
<codex-cart-voucher 
  :cart="cart" 
  @customEvent="handleManualToggle"
/>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-voucher-container` | Main voucher container |
| `_c-divider` | Divider line styling |
| `_c-collapsed` | Collapsed state styling |
| `_c-voucher-form` | Voucher input form container |
| `_c-btn-voucher` | Voucher apply button |
| `_c-active-vouchers` | Active voucher/payment method display |
| `_c-badge` | Badge styling for active vouchers |
| `_c-voucher-name` | Voucher code/name display |
| `_c-voucher-remove` | Remove voucher button |
| `_c-remove-icon` | Remove payment method link |
| `_c-text-icon-sm` | Small icon styling |

## Best Practices

### Voucher Management
- Always validate voucher codes server-side
- Provide clear feedback for invalid vouchers
- Show voucher impact on pricing immediately
- Handle voucher conflicts gracefully (trials, other discounts)

### User Experience
- Auto-collapse on scroll for mobile optimization
- Provide quick-apply options for common vouchers
- Show voucher suggestions when codes fail
- Use haptic feedback on mobile for confirmation

### Error Handling
- Display specific error messages for different failure types
- Suggest alternatives when vouchers are invalid
- Handle region and currency restrictions clearly
- Provide recovery options for partial failures

### Gift Card Integration
- Show remaining balances clearly
- Allow multiple gift cards to be applied
- Handle insufficient balance scenarios
- Provide easy removal of gift cards

### Mobile Optimization
- Use touch-friendly controls and sizing
- Implement barcode scanning where supported
- Optimize for one-handed operation
- Use modal overlays for complex voucher browsing

### Security
- Never expose sensitive voucher validation logic
- Implement rate limiting for voucher attempts
- Log suspicious voucher activity
- Validate all voucher operations server-side

### Accessibility
- Provide clear labels and aria-labels
- Ensure keyboard navigation works properly
- Use semantic HTML for voucher displays
- Test with screen readers

### Performance
- Debounce voucher input validation
- Cache valid voucher states
- Optimize scroll event handling
- Minimize re-renders during interactions

## Component Registration
```javascript
// Global registration
app.component('CodexCartVoucher', CartVoucher)

// Local registration  
import CartVoucher from '@/components/Cart/CartVoucher.vue'

export default {
  components: {
    CodexCartVoucher: CartVoucher
  }
}
``` 