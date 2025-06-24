# OrderCard Component

## Overview
The OrderCard component provides a tabular card interface for displaying individual order entries with comprehensive transaction information. It features order identification, date and time tracking, value display, payment method information, status indicators, and structured layout for effective order history management.

## Basic Usage
```vue
<codex-order-card
  :order="orderData"
  :loading="false"
  :enable-border="true"
/>
```

## Key Features
- Comprehensive order information display
- Tabular layout with structured columns
- Order ID and transaction details
- Date and time tracking
- Total value display with currency formatting
- Payment method information
- Order status indicators with styling
- Loading skeleton support
- Responsive design with mobile adaptations
- Fade-up transition animations

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| order | Object\|Number | Yes | - | Order data object or ID |
| loading | Boolean | No | false | Whether the card is in loading state |
| enableBorder | Boolean | No | false | Whether to enable card border styling |

### Common Props
All common props from `@/config/common` are supported.

## Events
Currently, the OrderCard component does not emit custom events. It serves as a display component for order information.

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content (overrides default order information) -->
</template>
```

### Content Slot
```vue
<template #content>
  <!-- Custom content section -->
</template>
```

### Footer Slot
```vue
<template #footer>
  <!-- Custom footer content -->
</template>
```

## Order Object Structure
The order prop expects an object with the following structure:
```javascript
{
  id: String|Number,          // Unique order identifier
  created_at: Date,           // Order creation date and time
  final_price: Number|String, // Final order total
  status: String,             // Order status ('paid', 'payment pending', 'cancelled', etc.)
  payments: [                 // Array of payment information
    {
      payment_type: String    // Payment method type
    }
  ]
}
```

## Order Status Management
The component handles different order statuses with appropriate styling:

### Status Classifications
- **Payment Pending**: Uses `_c-processing` class
- **Paid**: Uses `_c-success` class  
- **Cancelled**: Uses `_c-danger` class
- **Other Statuses**: Uses lowercase status as class name

### Status Display
- Status text is displayed in the status column
- Each status has associated CSS class for styling
- Status indicators provide visual feedback for order state

## Information Display Layout
The component uses a structured tabular layout with six columns:

### Column Structure
1. **Order ID**: Displays order number with # prefix
2. **Date**: Shows order date in short format
3. **Time**: Displays order time
4. **Value**: Shows final order total
5. **Payment Method**: Lists payment methods used
6. **Status**: Displays order status with styling

### Responsive Behavior
- Uses `_c-row` layout with space-between justification
- Adapts column sizing for different screen sizes
- Each column includes `data-name` attribute for mobile styling

## Loading State
The component supports skeleton loading with:
- **Tabular Layout**: Matches the six-column structure
- **Proportional Sizing**: Each column has appropriate width
- **Aligned Elements**: Maintains consistent spacing and alignment

## Payment Method Display
- **Multiple Payments**: Joins multiple payment types with commas
- **Fallback**: Shows 'n/a' when no payment information available
- **Array Handling**: Safely processes payment array data

## Internationalization

The `OrderCard` component uses translation keys for order table headers and status information:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `order.id` | Order ID column header | Order identification |
| `order.date` | Order date column header | Order creation date |
| `order.time` | Order time column header | Order creation time |
| `order.value` | Order value column header | Total order amount |
| `order.payment_method` | Payment method column header | How order was paid |
| `order.status` | Order status column header | Current order state |

### Implementation Examples

```vue
<!-- Order information display -->
<div class="_c-info-item _c-id" :data-name="$t('order.id')">
    #{{ order.id }}
</div>

<div class="_c-info-item _c-date" :data-name="$t('order.date')">
    {{ $filters.shortDate(order.created_at) }}
</div>

<div class="_c-info-item _c-time" :data-name="$t('order.time')">
    {{ $filters.time(order.created_at) }}
</div>

<div class="_c-info-item _c-total-value" :data-name="$t('order.value')">
    {{ order.final_price }}
</div>

<div class="_c-info-item _c-payment-method" :data-name="$t('order.payment_method')">
    {{ order.payments?.map(payment => payment.payment_type).join(', ') || 'n/a' }}
</div>

<div class="_c-info-item _c-payment-status" :data-name="$t('order.status')">
    <span class="_c-status">{{ order.status }}</span>
</div>
```

### Notes
- Translation keys used primarily for column headers and labels
- Order status values are displayed as raw text from the order object
- Payment method information is concatenated from payment objects
- Date and time formatting handled through Vue filters
- Integration with CSS data attributes for responsive design

## Examples

### Basic Implementation
```vue
<codex-order-card
  :order="order"
/>
```

### With Loading State
```vue
<codex-order-card
  :order="order"
  :loading="isLoading"
  :enable-border="true"
/>
```

### Custom Header Content
```vue
<codex-order-card
  :order="order"
>
  <template #header>
    <div class="order-header">
      <div class="order-details">
        <h4>Order #{{ order.id }}</h4>
        <span class="order-date">{{ formatFullDate(order.created_at) }}</span>
      </div>
      
      <div class="order-actions">
        <button @click="viewOrderDetails(order)">View Details</button>
        <button @click="downloadReceipt(order)" v-if="order.status === 'paid'">
          Download Receipt
        </button>
      </div>
    </div>
  </template>
</codex-order-card>
```

### Custom Content with Additional Information
```vue
<codex-order-card
  :order="order"
>
  <template #content>
    <div class="order-items">
      <h5>Order Items</h5>
      <div class="items-list">
        <div v-for="item in order.items" :key="item.id" class="order-item">
          <span class="item-name">{{ item.name }}</span>
          <span class="item-quantity">Qty: {{ item.quantity }}</span>
          <span class="item-price">{{ formatCurrency(item.price) }}</span>
        </div>
      </div>
      
      <div class="order-totals">
        <div class="total-line">
          <span>Subtotal:</span>
          <span>{{ formatCurrency(order.subtotal) }}</span>
        </div>
        <div class="total-line" v-if="order.tax_amount">
          <span>Tax:</span>
          <span>{{ formatCurrency(order.tax_amount) }}</span>
        </div>
        <div class="total-line total">
          <span>Total:</span>
          <span>{{ formatCurrency(order.final_price) }}</span>
        </div>
      </div>
    </div>
  </template>
</codex-order-card>
```

### Custom Footer with Actions
```vue
<codex-order-card
  :order="order"
>
  <template #footer>
    <div class="order-actions">
      <button 
        @click="reorderItems(order)" 
        v-if="order.status === 'paid'"
        class="reorder-btn"
      >
        Reorder
      </button>
      
      <button 
        @click="requestRefund(order)" 
        v-if="canRefund(order)"
        class="refund-btn"
      >
        Request Refund
      </button>
      
      <button @click="contactSupport(order)" class="support-btn">
        Contact Support
      </button>
    </div>
  </template>
</codex-order-card>
```

### In Orders List
```vue
<div class="orders-container">
  <div class="orders-header">
    <h3>Order History</h3>
    <div class="filter-options">
      <select v-model="statusFilter">
        <option value="">All Orders</option>
        <option value="paid">Paid</option>
        <option value="payment pending">Pending</option>
        <option value="cancelled">Cancelled</option>
      </select>
    </div>
  </div>
  
  <div class="orders-list">
    <codex-order-card
      v-for="order in filteredOrders"
      :key="order.id"
      :order="order"
      :enable-border="true"
    />
  </div>
</div>
```

### With Event Tracking
```vue
<codex-order-card
  :order="order"
>
  <template #header>
    <div class="trackable-order-header" @click="trackOrderView">
      <!-- Order header content -->
    </div>
  </template>
</codex-order-card>

<script setup>
const trackOrderView = () => {
  analytics.track('order_viewed', {
    order_id: order.id,
    order_status: order.status,
    order_value: order.final_price,
    payment_methods: order.payments?.map(p => p.payment_type)
  })
}
</script>
```

### Grouped Orders Display
```vue
<div class="orders-by-status">
  <div v-for="(statusOrders, status) in ordersByStatus" :key="status" class="status-group">
    <h4 class="status-header">{{ formatStatusHeader(status) }}</h4>
    <div class="status-orders">
      <codex-order-card
        v-for="order in statusOrders"
        :key="order.id"
        :order="order"
      />
    </div>
  </div>
</div>
```

### With Status-Specific Styling
```vue
<codex-order-card
  :order="order"
  :class="getOrderCardClass(order)"
/>

<script setup>
const getOrderCardClass = (order) => {
  return {
    'pending-order': order.status === 'payment pending',
    'completed-order': order.status === 'paid',
    'cancelled-order': order.status === 'cancelled',
    'high-value': parseFloat(order.final_price) > 100
  }
}
</script>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-order-card`: Order card specific styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-row`: Row flexbox layout
- `_c-items-center`: Center align flex items
- `_c-justify-between`: Space between flex items
- `_c-divider`: Divider styling (when border enabled)
- `_c-info-item`: Individual information item styling
- `_c-id`: Order ID styling
- `_c-date`: Date styling
- `_c-time`: Time styling
- `_c-total-value`: Total value styling
- `_c-payment-method`: Payment method styling
- `_c-payment-status`: Payment status styling
- `_c-status`: Status text styling
- `_c-processing`: Processing status styling
- `_c-success`: Success status styling
- `_c-danger`: Danger/cancelled status styling

## Best Practices

### Recommended Usage Patterns
- Always provide proper order object structure
- Handle loading states for better UX
- Use appropriate status styling for visual feedback
- Display payment information clearly
- Format currency values consistently
- Handle missing payment data gracefully
- Provide clear order identification
- Use appropriate date/time formatting

### Common Pitfalls to Avoid
- Not handling missing order data gracefully
- Missing loading state implementation
- Forgetting to handle different order statuses
- Not providing proper currency formatting
- Missing responsive considerations
- Insufficient handling of payment method arrays
- Not tracking order interaction analytics
- Missing accessibility considerations

### Accessibility Considerations
- Ensure cards are keyboard navigable
- Provide clear labels for order information
- Use appropriate ARIA attributes for status indicators
- Ensure adequate color contrast for status displays
- Provide screen reader friendly order information
- Include proper semantic HTML structure
- Use data attributes for responsive layouts
- Handle disabled states properly if interactive

### Error Handling
- Handle missing order data gracefully
- Provide fallbacks for malformed order objects
- Handle missing payment information appropriately
- Display meaningful error states
- Clear error states when data updates
- Handle currency formatting errors
- Provide user feedback for data issues

### State Management
- Track order status properly
- Handle status changes dynamically if needed
- Manage loading states consistently
- Handle order data updates appropriately
- Coordinate with parent component state
- Manage order list state effectively

### Performance Considerations
- Optimize order data rendering
- Minimize re-renders during data changes
- Handle large order lists efficiently
- Implement proper cleanup for event listeners
- Cache formatted order data appropriately
- Optimize status class calculations
- Handle real-time order updates efficiently

### Data Formatting
- Format currency consistently across all orders
- Use proper date/time formatting for user locale
- Handle different payment method types appropriately
- Format order IDs consistently
- Display status information clearly
- Handle numeric values properly
- Ensure consistent data presentation

### Status Management
- Display order status clearly and consistently
- Use appropriate visual indicators for each status
- Handle status transitions if orders update
- Provide clear status feedback
- Coordinate status styling with design system
- Handle edge cases in status logic
- Track status distribution for analytics

### Layout Considerations
- Ensure tabular layout works across screen sizes
- Handle long order IDs appropriately
- Manage text overflow in columns
- Provide consistent spacing and alignment
- Use responsive design principles
- Handle variable content lengths
- Maintain layout integrity with different data

## Component Registration
The component is registered as `codex-order-card` in the application. 