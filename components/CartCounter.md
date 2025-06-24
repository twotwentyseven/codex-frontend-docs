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

## Examples

### Header Navigation Counter
```vue
<template>
  <header class="site-header">
    <nav class="navigation">
      <a href="/" class="logo">Store</a>
      
      <div class="nav-items">
        <a href="/products">Products</a>
        <a href="/about">About</a>
        
        <div class="cart-indicator" @click="openCart">
          <i class="ri-shopping-cart-line"></i>
          <codex-cart-counter>
            <template #cart-summary="{ cartSize }">
              <span class="cart-badge" v-if="cartSize > 0">
                {{ cartSize }}
              </span>
            </template>
          </codex-cart-counter>
        </div>
      </div>
    </nav>
  </header>
</template>

<script setup>
const openCart = () => {
  // Open cart modal or navigate to cart page
  router.push('/cart')
}
</script>

<style scoped>
.cart-indicator {
  position: relative;
  cursor: pointer;
  padding: 8px;
}

.cart-badge {
  position: absolute;
  top: -2px;
  right: -2px;
  background: #ff4444;
  color: white;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  font-weight: bold;
}
</style>
```

### Mobile Cart Button
```vue
<template>
  <div class="mobile-cart-button" @click="toggleCart">
    <div class="cart-icon">
      <i class="ri-shopping-bag-line"></i>
      <codex-cart-counter>
        <template #cart-summary="{ cartSize }">
          <div class="mobile-counter" :class="{ 'has-items': cartSize > 0 }">
            <span class="count">{{ cartSize }}</span>
            <span class="label">{{ cartSize === 1 ? 'item' : 'items' }}</span>
          </div>
        </template>
      </codex-cart-counter>
    </div>
  </div>
</template>

<script setup>
const toggleCart = () => {
  // Toggle mobile cart drawer
  emit('toggle-cart')
}
</script>

<style scoped>
.mobile-cart-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: #007bff;
  color: white;
  border-radius: 50px;
  padding: 12px 16px;
  box-shadow: 0 4px 12px rgba(0, 123, 255, 0.3);
  cursor: pointer;
  z-index: 100;
}

.cart-icon {
  display: flex;
  align-items: center;
  gap: 8px;
}

.mobile-counter {
  display: flex;
  flex-direction: column;
  align-items: center;
  line-height: 1;
}

.mobile-counter.has-items {
  animation: pulse 0.3s ease-in-out;
}

.count {
  font-weight: bold;
  font-size: 14px;
}

.label {
  font-size: 10px;
  opacity: 0.9;
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}
</style>
```

### Detailed Cart Summary
```vue
<template>
  <div class="cart-summary-widget">
    <codex-cart-counter>
      <template #cart-summary="{ cart, customer, cartSize }">
        <div class="detailed-summary">
          <div class="cart-info">
            <h3>Your Cart</h3>
            <div class="item-count">
              <span class="count">{{ cartSize }}</span>
              <span class="label">{{ cartSize === 1 ? 'item' : 'items' }}</span>
            </div>
          </div>
          
          <div v-if="cart && cartSize > 0" class="cart-preview">
            <div class="total-amount">
              <span class="label">Total:</span>
              <span class="amount">{{ cart.price || '$0.00' }}</span>
            </div>
            
            <div v-if="cart.voucher" class="voucher-applied">
              <i class="ri-price-tag-3-line"></i>
              <span>{{ cart.voucher.code }} applied</span>
            </div>
          </div>
          
          <div v-if="customer" class="customer-info">
            <span class="welcome">Welcome, {{ customer.first_name }}!</span>
          </div>
          
          <div v-else class="guest-prompt">
            <span>Sign in for faster checkout</span>
          </div>
        </div>
      </template>
    </codex-cart-counter>
  </div>
</template>

<style scoped>
.cart-summary-widget {
  background: white;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.detailed-summary {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.cart-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cart-info h3 {
  margin: 0;
  font-size: 18px;
  color: #333;
}

.item-count {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.count {
  font-size: 24px;
  font-weight: bold;
  color: #007bff;
}

.label {
  font-size: 14px;
  color: #666;
}

.cart-preview {
  border-top: 1px solid #eee;
  padding-top: 12px;
}

.total-amount {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-weight: bold;
}

.amount {
  color: #007bff;
  font-size: 18px;
}

.voucher-applied {
  display: flex;
  align-items: center;
  gap: 4px;
  color: #28a745;
  font-size: 12px;
  margin-top: 4px;
}

.customer-info {
  color: #28a745;
  font-size: 14px;
}

.guest-prompt {
  color: #666;
  font-size: 12px;
  font-style: italic;
}
</style>
```

### Sidebar Cart Widget
```vue
<template>
  <aside class="sidebar-cart">
    <codex-cart-counter>
      <template #cart-summary="{ cart, cartSize }">
        <div class="cart-widget">
          <div class="widget-header">
            <h4>Shopping Cart</h4>
            <div class="cart-toggle" @click="toggleExpanded">
              <span class="item-count">{{ cartSize }}</span>
              <i :class="expanded ? 'ri-arrow-up-s-line' : 'ri-arrow-down-s-line'"></i>
            </div>
          </div>
          
          <div v-if="expanded && cartSize > 0" class="cart-contents">
            <div v-for="line in cart?.lines?.slice(0, 3)" :key="line.id" class="cart-item">
              <span class="item-name">{{ line.name }}</span>
              <span class="item-quantity">{{ line.quantity }}x</span>
              <span class="item-price">{{ line.line_price }}</span>
            </div>
            
            <div v-if="cart?.lines?.length > 3" class="more-items">
              +{{ cart.lines.length - 3 }} more items
            </div>
            
            <div class="cart-total">
              <strong>Total: {{ cart.price }}</strong>
            </div>
            
            <button @click="goToCart" class="view-cart-btn">
              View Full Cart
            </button>
          </div>
          
          <div v-else-if="expanded && cartSize === 0" class="empty-cart">
            <p>Your cart is empty</p>
            <a href="/products" class="shop-link">Start Shopping</a>
          </div>
        </div>
      </template>
    </codex-cart-counter>
  </aside>
</template>

<script setup>
const expanded = ref(false)

const toggleExpanded = () => {
  expanded.value = !expanded.value
}

const goToCart = () => {
  router.push('/cart')
}
</script>

<style scoped>
.sidebar-cart {
  position: sticky;
  top: 20px;
  background: #f8f9fa;
  border-radius: 8px;
  overflow: hidden;
}

.cart-widget {
  padding: 0;
}

.widget-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px;
  background: #007bff;
  color: white;
  cursor: pointer;
}

.widget-header h4 {
  margin: 0;
  font-size: 16px;
}

.cart-toggle {
  display: flex;
  align-items: center;
  gap: 8px;
}

.item-count {
  background: rgba(255, 255, 255, 0.2);
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: bold;
}

.cart-contents {
  padding: 16px;
}

.cart-item {
  display: grid;
  grid-template-columns: 1fr auto auto;
  gap: 8px;
  padding: 8px 0;
  border-bottom: 1px solid #eee;
  font-size: 14px;
}

.cart-item:last-child {
  border-bottom: none;
}

.item-name {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.item-quantity {
  color: #666;
}

.item-price {
  font-weight: bold;
}

.more-items {
  color: #666;
  font-size: 12px;
  font-style: italic;
  margin: 8px 0;
}

.cart-total {
  margin: 16px 0;
  padding-top: 16px;
  border-top: 2px solid #eee;
  text-align: center;
}

.view-cart-btn {
  width: 100%;
  background: #007bff;
  color: white;
  border: none;
  padding: 12px;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.view-cart-btn:hover {
  background: #0056b3;
}

.empty-cart {
  padding: 16px;
  text-align: center;
  color: #666;
}

.shop-link {
  color: #007bff;
  text-decoration: none;
}

.shop-link:hover {
  text-decoration: underline;
}
</style>
```

### Notification Style Counter
```vue
<template>
  <div class="notification-cart">
    <codex-cart-counter>
      <template #cart-summary="{ cart, cartSize }">
        <div class="notification-style" :class="{ 'has-items': cartSize > 0 }">
          <div class="notification-icon">
            <i class="ri-shopping-cart-2-line"></i>
            <span v-if="cartSize > 0" class="notification-badge">
              {{ cartSize > 99 ? '99+' : cartSize }}
            </span>
          </div>
          
          <div v-if="cartSize > 0" class="notification-content">
            <div class="notification-title">
              {{ cartSize }} {{ cartSize === 1 ? 'item' : 'items' }} in cart
            </div>
            <div class="notification-subtitle">
              Total: {{ cart?.price || '$0.00' }}
            </div>
          </div>
          
          <div v-else class="notification-content">
            <div class="notification-title">Cart is empty</div>
            <div class="notification-subtitle">Add items to get started</div>
          </div>
        </div>
      </template>
    </codex-cart-counter>
  </div>
</template>

<style scoped>
.notification-cart {
  position: relative;
}

.notification-style {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  background: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  transition: all 0.3s ease;
  cursor: pointer;
}

.notification-style.has-items {
  background: #e7f3ff;
  border-color: #007bff;
}

.notification-style:hover {
  background: #e9ecef;
}

.notification-style.has-items:hover {
  background: #cce7ff;
}

.notification-icon {
  position: relative;
  font-size: 24px;
  color: #6c757d;
}

.notification-style.has-items .notification-icon {
  color: #007bff;
}

.notification-badge {
  position: absolute;
  top: -8px;
  right: -8px;
  background: #dc3545;
  color: white;
  border-radius: 10px;
  padding: 2px 6px;
  font-size: 10px;
  font-weight: bold;
  min-width: 16px;
  text-align: center;
}

.notification-content {
  flex-grow: 1;
}

.notification-title {
  font-weight: 600;
  color: #495057;
  font-size: 14px;
}

.notification-subtitle {
  color: #6c757d;
  font-size: 12px;
  margin-top: 2px;
}

.notification-style.has-items .notification-title {
  color: #007bff;
}
</style>
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