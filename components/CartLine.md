# CartLine Component

## Overview
The CartLine component renders individual cart items with comprehensive pricing displays, quantity controls, and subscription management features. It supports both one-time purchases and subscription products with joining fees, trial periods, and recurring payments. The component features loading states, price comparison displays, quantity management, and customizable skeleton loading layouts for different product types.

## Basic Usage
```vue
<template>
  <div class="cart-items">
    <codex-cart-line
      v-for="line in cartLines"
      :key="line.hash"
      :line="line"
      :loading="getLineLoading(line.hash)"
      :readonly="false"
      :allow-multiples="line.buyable_type !== 'Plan'"
      @increment="handleIncrement"
      @decrement="handleDecrement"
      @remove="handleRemove"
    />
  </div>
</template>

<script setup>
const cartLines = ref([
  {
    hash: 'line_123',
    name: 'Premium Membership',
    quantity: 1,
    line_price: '$19.99',
    line_original_price: '$24.99',
    line_ongoing_price: '$19.99/month',
    line_ongoing_description: 'per month',
    buyable_type: 'Plan',
    properties: {
      payment_starts_at: '2024-02-01'
    }
  }
])

const handleIncrement = (lineHash) => {
  console.log('Increment item:', lineHash)
}

const handleDecrement = (lineHash) => {
  console.log('Decrement item:', lineHash)
}

const handleRemove = (lineHash) => {
  console.log('Remove item:', lineHash)
}

const getLineLoading = (lineHash) => {
  return false // Return loading state for specific line
}
</script>
```

## Key Features
- Comprehensive price display with original and discounted prices
- Subscription product support with joining fees and recurring payments
- Trial period and membership start date display
- Quantity controls with increment/decrement/remove actions
- Skeleton loading states for different product layouts
- Read-only mode for order confirmation displays
- Flexible pricing calculations with join fee combinations
- Accessibility support with proper button states
- Internationalized text and date formatting

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `line` | `Object` | `required` | Cart line item data with pricing and product info |
| `loading` | `Boolean` | `false` | Whether this line is in loading state |
| `readonly` | `Boolean` | `true` | Whether to show controls or read-only display |
| `allowMultiples` | `Boolean` | `true` | Whether quantity can be changed (disabled for plans) |

## Common Props

| Prop Name | Usage |
|-----------|-------|
| `formatCurrency` | Formats all monetary values according to locale |

## Common Functions

| Function | Usage |
|----------|-------|
| `formatCurrency` | Used to display all price values in correct currency format |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `increment` | `lineHash` | Emitted when quantity should be increased |
| `decrement` | `lineHash` | Emitted when quantity should be decreased |
| `remove` | `lineHash` | Emitted when item should be removed |
| `setQuantity` | `lineHash, newQuantity` | Emitted when quantity is set directly |

## Slots

| Slot | Description |
|------|-------------|
| `item-header` | Custom header content with item name and price |
| `item-content` | Custom content area with subscription details |
| `item-footer` | Custom footer with quantity controls and remove button |

## States

### Default Product State
```vue
<codex-cart-line 
  :line="productLine"
  :readonly="false"
  :allow-multiples="true"
/>
```

### Subscription Plan State
```vue
<codex-cart-line 
  :line="subscriptionLine"
  :readonly="false"
  :allow-multiples="false"
/>
```

### Loading State
```vue
<codex-cart-line 
  :line="line"
  :loading="true"
/>
```

### Read-only State (Order Confirmation)
```vue
<codex-cart-line 
  :line="line"
  :readonly="true"
/>
```

## Line Item Data Structure

### Basic Product Line
```javascript
{
  hash: 'line_abc123',
  name: 'Premium Widget',
  quantity: 2,
  line_price: '$29.99',
  line_original_price: '$39.99',
  line_price_raw: 2999,
  line_original_price_raw: 3999,
  buyable_type: 'Product'
}
```

### Subscription Line with Trial
```javascript
{
  hash: 'line_sub456',
  name: 'Premium Membership',
  quantity: 1,
  line_price: '$0.00',
  line_original_price: '$9.99',
  line_ongoing_price: '$19.99/month',
  line_original_ongoing_price: '$29.99/month',
  line_ongoing_price_raw: 1999,
  line_ongoing_description: 'per month',
  buyable_type: 'Plan',
  buyable: {
    trial_days: 14,
    trial_expires: '2024-02-15'
  },
  properties: {
    start_at: '2024-02-01',
    payment_starts_at: '2024-02-15'
  }
}
```

## Internationalization

The component uses the following translation keys:

### Subscription and Membership Information
| Key | Usage |
|-----|-------|
| `cart.joining_fee` | Joining fee label for subscription plans |
| `cart.recurring_payments` | Recurring payments label |
| `cart.trial_start_date` | Trial start date label |
| `cart.membership_start` | Membership start date label |

### Cart Actions
| Key | Usage |
|-----|-------|
| `cart.remove_btn` | Remove item button text |

### Translation Usage Examples
```vue
<!-- Subscription pricing details -->
<div v-if="line.line_ongoing_price_raw > 0" class="_c-joining-fee _c-flex _c-justify-between">
  {{ $t('cart.joining_fee') }}
  <span class="_c-text-bold">{{ line.line_price }}</span>
</div>

<div v-if="line.line_ongoing_price_raw > 0" class="_c-recurring-payments _c-flex _c-justify-between">
  {{ $t('cart.recurring_payments') }}
  <span class="_c-text-bold">{{ line.line_ongoing_price }} {{ line.line_ongoing_description }}</span>
</div>

<!-- Trial and membership dates -->
<div v-if="line.buyable?.trial_days > 0" class="_c-trial-start _c-flex _c-justify-between">
  {{ $t('cart.trial_start_date') }}
  <span class="_c-text-bold">{{ startAt }}</span>
</div>

<div v-if="line.line_ongoing_price_raw > 0" class="_c-membership-start _c-flex _c-justify-between">
  {{ $t('cart.membership_start') }}
  <span class="_c-text-bold">{{ $filters.dateFormat(line.properties.payment_starts_at, 'Do MMM YYYY') }}</span>
</div>

<!-- Remove button -->
<button class="_c-remove _c-link" @click.prevent="remove">
  {{ $t('cart.remove_btn') }}
</button>
```

### Translation Notes

#### Subscription-Focused Translations
The CartLine component is heavily focused on subscription product support:
- Joining fee and recurring payment labels for subscription plans
- Trial period and membership start date information
- Complex pricing scenarios with multiple fee types

#### Quantity Controls
The component includes quantity management with minimal translation needs:
- Uses symbol-based increment/decrement buttons (+ and -)
- Numeric quantity display without translation requirements
- Remove button uses translation for accessibility

#### Date Formatting Integration
The component integrates with date formatting utilities:
- Uses `$filters.dateFormat` for consistent date presentation
- Supports various date formats through filter system
- Maintains locale-appropriate date formatting

#### Price Display Integration
The component uses formatted pricing from the backend:
- Displays `line_price` and `line_ongoing_price` as pre-formatted strings
- Supports strike-through pricing for discounted items
- Integrates with currency formatting for consistent display

## Usage Examples

### Basic Cart Line
```vue
<codex-cart-line :line="cartLine">
  <template #product-info>
    <h3>{{ cartLine.product_name || $t('cart.line.unnamed_product') }}</h3>
  </template>
</codex-cart-line>
```

### With Custom Actions
```vue
<codex-cart-line 
  :line="cartLine"
  @quantity-changed="handleQuantityChange"
  @remove="handleRemove"
>
  <template #actions>
    <codex-button 
      :default-text="$t('cart.line.actions.save_later')"
      @click="saveForLater"
    />
  </template>
</codex-cart-line>
```

### With Error Handling
```vue
<codex-cart-line 
  :line="cartLine"
  :error="lineError"
  @retry="retryLineUpdate"
>
  <template #error="{ error }">
    <div class="custom-error">
      <p>{{ $t('cart.line.error.update_failed') }}</p>
      <codex-button 
        :default-text="$t('cart.line.error.retry')"
        @click="retryUpdate"
      />
    </div>
  </template>
</codex-cart-line>
```

## Best Practices

### Quantity Management
- Disable quantity controls for subscription plans and one-time purchases
- Provide visual feedback during quantity changes with loading states
- Implement debouncing for rapid quantity changes
- Show clear disabled states when controls are not available

### Price Display
- Always show original prices when items are discounted
- Clearly separate joining fees from recurring charges for subscriptions
- Use consistent currency formatting throughout
- Display trial periods and membership start dates prominently

### Subscription Handling
- Clearly communicate trial periods and billing start dates
- Show recurring payment information prominently
- Handle subscription-specific actions like plan modifications
- Provide clear cancellation options where appropriate

### Loading States
- Use skeleton loading layouts appropriate to the product type
- Show loading states during quantity changes and removals
- Disable all controls during loading to prevent duplicate actions
- Provide immediate visual feedback for user actions

### Accessibility
- Ensure all buttons have proper disabled states
- Use semantic HTML for price comparisons
- Provide clear labels for screen readers
- Implement proper keyboard navigation for controls

### Mobile Optimization
- Use touch-friendly button sizes for quantity controls
- Optimize layout for mobile screens with appropriate spacing
- Consider swipe gestures for removal actions
- Test thoroughly on various mobile devices

### Error Handling
- Gracefully handle failed quantity updates
- Provide clear error messages for invalid operations
- Implement retry mechanisms for failed actions
- Log errors for debugging and improvement

## Component Registration
```javascript
// Global registration
app.component('CodexCartLine', CartLine)

// Local registration  
import CartLine from '@/components/Cart/CartLine.vue'

export default {
  components: {
    CodexCartLine: CartLine
  }
}
``` 