# Orders Component

## Overview
The Orders component provides a comprehensive interface for displaying customer order history with detailed order information. It features tabular order display with headers, pagination support, loading states, error handling, and flexible slot-based customization for order management.

## Basic Usage
```vue
<codex-orders
  :per-page="10"
  :group="'customer-orders'"
/>
```

## Key Features
- Structured order display with table headers
- Order card integration for detailed information
- Loading states with skeleton placeholders
- No results handling
- Error message display
- Pagination support with filter context
- Customer authentication integration
- Responsive table layout
- Internationalization support
- Flexible slot-based layout

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| perPage | Number | No | 10 | Number of orders per page |
| group | String | No | undefined | Filter context group for pagination and filtering |

### Common Props
All common props from `@/config/common` are supported, including:
| Prop Name | Usage |
|-----------|-------|
| loadingItems | Number of skeleton items to show during loading |

### Common Functions
The component uses utilities from `useCommon`:
| Function | Usage |
|----------|-------|
| loadingItems | Provides skeleton loading item configuration |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| orders-loaded | `orders: Array` | Emitted when orders are successfully loaded |
| order-selected | `order: Object` | Emitted when an order is selected for viewing |

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content>
  <!-- Custom content and order display -->
</template>
```

### Footer Slot
```vue
<template #footer="{ pagination }">
  <!-- Custom footer content (pagination by default) -->
</template>
```

### Error Messages Slot
```vue
<template #error-messages="{ genericErrors }">
  <!-- Custom error display -->
</template>
```

### No Results Slot
```vue
<template #no-results>
  <!-- Custom no results message -->
</template>
```

### Slot Props
| Prop Name | Type | Description |
|-----------|------|-------------|
| pagination | Object | Pagination state information |
| genericErrors | Array | Generic error messages |

## States
The component has multiple states in order of priority:
1. **Unauthenticated State** - Component hidden when no customer
2. **Loading State** - Shows skeleton order cards during data fetch
3. **Error State** - Displays error messages
4. **No Results State** - Shows no orders message
5. **Orders Display State** - Normal order table display with headers and cards

## Order Information Display
The component displays orders in a structured table format with headers:
- **ID**: Order identification number
- **Date**: Order creation date
- **Time**: Order creation time
- **Value**: Total order value
- **Payment Method**: Payment method used
- **Status**: Current payment status

## Internationalization

The component uses the following translation keys:

### Order Table Headers
| Key | Usage |
|-----|-------|
| `order.id` | Order ID column header |
| `order.date` | Order date column header |
| `order.time` | Order time column header |
| `order.value` | Order value column header |
| `order.payment_method` | Payment method column header |
| `order.status` | Order status column header |

### Core Interface Messages
| Key | Usage |
|-----|-------|
| `order.you_have_no_orders` | No orders available message |

### Translation Usage Examples
```vue
<!-- Order table headers -->
<div class="_c-header _c-order-card">
  <div class="_c-row _c-justify-between _c-divider">
    <div class="_c-info-item _c-id">{{ $t('order.id') }}</div>
    <div class="_c-info-item _c-date">{{ $t('order.date') }}</div>
    <div class="_c-info-item _c-time">{{ $t('order.time') }}</div>
    <div class="_c-info-item _c-total-value">{{ $t('order.value') }}</div>
    <div class="_c-info-item _c-payment-method">{{ $t('order.payment_method') }}</div>
    <div class="_c-info-item _c-payment-status">{{ $t('order.status') }}</div>
  </div>
</div>

<!-- No orders message -->
<div v-else class="_c-no-results">
  <slot name="no-results">{{ $t('order.you_have_no_orders') }}</slot>
</div>
```

### Translation Notes

#### Order Management Interface
The Orders component provides comprehensive translation support for:
- Structured table headers showing order information categories
- Empty state messaging when no orders exist
- Integration with order card components for detailed order display

#### Order Card Integration
The component integrates with `codex-order-card` components that handle their own translations for:
- Order details and line items
- Payment status and method information
- Order actions (view details, reorder, etc.)
- Date and time formatting
- Currency and pricing display

#### Table Structure Translation
The component creates a structured table layout with translated headers for:
- **Order ID**: Unique order identification
- **Date**: Order creation date
- **Time**: Order creation time
- **Value**: Total order amount
- **Payment Method**: Payment method used (card, PayPal, etc.)
- **Status**: Current order/payment status

#### Empty State Handling
When no orders are available, the component provides:
- Translated empty state message
- Customizable slot for alternative empty state content
- Integration with call-to-action buttons for shopping

#### Error State Integration
The component integrates with error handling that uses translations for:
- API error messages
- Network connectivity issues
- Authentication problems
- Data loading failures

#### Pagination Integration
The component works with pagination that handles its own translations for:
- Page navigation controls
- Results count display
- Loading state messages

## Examples

### Basic Implementation
```vue
<codex-orders />
```

### With Custom Page Size
```vue
<codex-orders
  :per-page="20"
  :group="'user-orders'"
/>
```

### With Custom Header
```vue
<codex-orders>
  <template #header>
    <div class="orders-header">
      <h2>Order History</h2>
      <p>Review your past orders and payment information</p>
    </div>
  </template>
</codex-orders>
```

### Custom No Results State
```vue
<codex-orders>
  <template #no-results>
    <div class="custom-no-orders">
      <h3>No Orders Yet</h3>
      <p>You haven't placed any orders yet. Start shopping to see your order history here!</p>
      <codex-button 
        @click="navigateToShop"
        :default-text="'Start Shopping'"
        variant="primary"
      />
    </div>
  </template>
</codex-orders>
```

### Custom Error Handling
```vue
<codex-orders>
  <template #error-messages="{ genericErrors }">
    <div class="custom-error-display">
      <i class="error-icon" />
      <div>
        <h4>Unable to Load Orders</h4>
        <ul>
          <li v-for="error in genericErrors" :key="error">
            {{ error }}
          </li>
        </ul>
        <button @click="retryLoadOrders">Try Again</button>
      </div>
    </div>
  </template>
</codex-orders>
```

### Custom Footer with Order Statistics
```vue
<codex-orders>
  <template #footer="{ pagination }">
    <div class="orders-footer">
      <div class="order-stats">
        <span>Total Orders: {{ orders.length }}</span>
        <span>This Year: {{ thisYearCount }}</span>
        <span>Total Spent: {{ totalSpent }}</span>
      </div>
      <codex-pagination :group="group" />
    </div>
  </template>
</codex-orders>
```

### With Filter Context
```vue
<codex-orders
  :group="'filtered-orders'"
  @orders-loaded="handleOrdersLoaded"
  @order-selected="viewOrderDetails"
/>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-orders-card`: Orders-specific card styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-order-card`: Order table header styling
- `_c-row`: Table row layout
- `_c-justify-between`: Space between flex items
- `_c-divider`: Header divider styling
- `_c-info-item`: Table column styling
- `_c-id`: Order ID column
- `_c-date`: Order date column
- `_c-time`: Order time column
- `_c-total-value`: Order value column
- `_c-payment-method`: Payment method column
- `_c-payment-status`: Payment status column
- `_c-no-results`: No results state styling

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Implement proper loading states during order fetch
- Use appropriate page sizes for performance
- Handle order data with proper formatting
- Implement error retry mechanisms
- Provide clear order information display
- Use pagination for large order datasets

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing loading states during data fetch
- Forgetting to handle empty order lists
- Not providing clear order information
- Missing error handling for failed loads
- Not updating order list when data changes
- Insufficient handling of payment status information

### Accessibility Considerations
- Ensure order table is keyboard navigable
- Provide clear column headers
- Use appropriate ARIA attributes for table content
- Ensure order cards are accessible
- Provide screen reader friendly order information
- Use semantic HTML for tabular data
- Include proper sorting and filtering feedback

### Error Handling
- Display clear error messages for failed loads
- Provide retry mechanisms for network failures
- Handle authentication errors appropriately
- Show validation errors when appropriate
- Clear error states when operations succeed
- Handle partial data load failures
- Provide fallback states for missing order data

### State Management
- Track order data state properly
- Handle customer authentication changes
- Manage loading states consistently
- Handle pagination state with filters
- Coordinate with order update operations
- Manage order selection state
- Handle real-time order updates when applicable

### Performance Considerations
- Implement virtual scrolling for large order lists
- Lazy load order card components
- Optimize table rendering performance
- Handle large order datasets efficiently
- Consider pagination for better performance
- Implement proper cleanup for event listeners
- Cache order data appropriately

### Order Data Management
- Format order dates consistently
- Handle currency formatting properly
- Display payment status clearly
- Manage order state transitions
- Handle partial order information
- Provide clear order identification
- Format order values appropriately

### Filter Context Integration
- Use appropriate group names for filtering
- Handle filter state changes
- Coordinate pagination with filters
- Update orders when filters change
- Handle filter reset scenarios
- Provide clear filter feedback
- Maintain filter state across navigation

### Table Layout Management
- Ensure responsive table design
- Handle column overflow appropriately
- Maintain consistent column alignment
- Provide clear header labels
- Handle dynamic column sizing
- Ensure mobile-friendly table display
- Implement proper table scrolling

## Component Registration
The component is registered as `codex-orders` in the application. 