# CartCounter Component

## Overview
The CartCounter component provides a simple, reusable cart item counter that displays the total number of items in the user's cart. It automatically loads cart data when needed, integrates with the cart composition API, and provides a customizable slot for displaying cart summary information with access to cart data, customer information, and cart size.

## Basic Usage
```vue
<template>
  <div class="header-cart">
    <span class="cart-label">Cart</span>
    <codex-cart-counter />
    <i class="ri-shopping-cart-line"></i>
  </div>
</template>
```

## Key Features
- Automatic cart data loading when not available
- Real-time cart size calculation and display
- Template override support for custom styling
- Slot-based customization with cart data access
- Integration with useCart composition API
- Customer state awareness
- Minimal performance footprint

## Configuration Props
This component does not accept any props.

## Events
This component does not emit any events.

## Slots

| Slot | Description |
|------|-------------|
| `cart-summary` | Custom cart summary display with cart, customer, and cartSize data |

### Slot Props

| Prop | Type | Description |
|------|------|-------------|
| `cart` | `Object` | Complete cart data object |
| `customer` | `Object` | Current customer information |
| `cartSize` | `Number` | Total number of items in cart |

## States

### Default State
```vue
<!-- Shows numeric count of cart items -->
<codex-cart-counter />
```

### Custom Display State
```vue
<codex-cart-counter>
  <template #cart-summary="{ cart, customer, cartSize }">
    <div class="custom-cart-display">
      <span class="count">{{ cartSize }}</span>
      <span class="items">items</span>
    </div>
  </template>
</codex-cart-counter>
```

### Empty Cart State
```vue
<!-- Automatically shows 0 when cart is empty -->
<codex-cart-counter />
```

## CSS Classes

| Class | Description |
|-------|-------------|
| Default | No specific classes - renders content directly |

## Template Override

The component supports template override via the `templateOverride` property:

```javascript
// In component registration
{
  templateOverride: '#codex-template-cart-counter'
}
```

## Best Practices

### Performance
- The component automatically loads cart data only when needed
- Cart size is calculated efficiently from existing cart state
- Minimal re-renders due to focused cart state watching

### User Experience
- Always show the current cart count, even when 0
- Provide visual feedback when cart count changes
- Consider animation for count updates to draw attention
- Make cart counter clickable to access full cart

### Integration
- Use alongside other cart components for consistent state
- Combine with cart modals or drawers for complete experience
- Integrate with navigation and mobile menu systems
- Consider placement in headers, sidebars, and floating buttons

### Accessibility
- Ensure cart counter is keyboard accessible when interactive
- Provide screen reader announcements for cart count changes
- Use appropriate ARIA labels for cart status
- Consider high contrast modes for visibility

### Mobile Optimization
- Make cart counters touch-friendly on mobile devices
- Consider different display styles for mobile vs desktop
- Use appropriate sizing for mobile tap targets
- Test visibility across different screen sizes

### Customization
- Use the slot system for complex cart displays
- Access cart, customer, and cartSize data for rich interfaces
- Maintain consistent styling with your design system
- Consider loading states for cart data fetching

## Component Registration
```javascript
// Global registration
app.component('CodexCartCounter', CartCounter)

// Local registration  
import CartCounter from '@/components/Cart/CartCounter.vue'

export default {
  components: {
    CodexCartCounter: CartCounter
  }
}
```

## Internationalization

The CartCounter component has minimal translation requirements as it primarily displays numeric cart item counts.

### Translation Notes

#### Minimal Translation Requirements
The CartCounter component itself does not use direct translation keys as it:
- Displays numeric cart size by default
- Provides cart data through slots for custom formatting
- Relies on parent components for contextual translations

#### Custom Display Translation
Translation can be implemented in the slot content for custom cart displays:

```vue
<codex-cart-counter>
  <template #cart-summary="{ cart, customer, cartSize }">
    <div class="translated-cart-display">
      <span class="count">{{ cartSize }}</span>
      <span class="label">
        {{ $t('cart.items', { count: cartSize }) }}
      </span>
    </div>
  </template>
</codex-cart-counter>
```

#### Pluralization Support
For languages requiring pluralization, implement in the slot:

```vue
<codex-cart-counter>
  <template #cart-summary="{ cartSize }">
    <span class="cart-text">
      {{ $t('cart.item_count', cartSize, { 
        count: cartSize,
        singular: $t('cart.item'),
        plural: $t('cart.items')
      }) }}
    </span>
  </template>
</codex-cart-counter>
```

#### Accessibility Translation
For screen readers and accessibility:

```vue
<codex-cart-counter>
  <template #cart-summary="{ cartSize }">
    <span 
      :aria-label="$t('accessibility.cart_items', { count: cartSize })"
      class="cart-counter"
    >
      {{ cartSize }}
    </span>
  </template>
</codex-cart-counter>
```

#### Integration with Cart Components
The CartCounter typically works alongside other cart components that handle their own translations:
- Cart modal or drawer components
- Checkout process translations
- Product name and pricing translations
- Cart action button translations

#### Empty State Translation
For empty cart states:

```vue
<codex-cart-counter>
  <template #cart-summary="{ cartSize }">
    <div v-if="cartSize === 0" class="empty-cart">
      <span>{{ $t('cart.empty') }}</span>
    </div>
    <div v-else class="cart-with-items">
      <span>{{ cartSize }}</span>
      <span>{{ $t('cart.items') }}</span>
    </div>
  </template>
</codex-cart-counter>
```

#### Currency and Formatting
When displaying cart totals alongside counts:

```vue
<codex-cart-counter>
  <template #cart-summary="{ cart, cartSize }">
    <div class="cart-summary">
      <div class="item-count">
        {{ $t('cart.items_count', { count: cartSize }) }}
      </div>
      <div class="cart-total">
        {{ $t('cart.total') }}: {{ formatCurrency(cart.total) }}
      </div>
    </div>
  </template>
</codex-cart-counter>
``` 