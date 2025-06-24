# OrderTotals Component

## Overview
The OrderTotals component provides comprehensive display of order financial summaries including subtotals, taxes, discounts, and final amounts. It features automatic currency formatting, tax breakdown displays, discount calculations, and support for multiple payment methods. The component handles complex pricing scenarios including promotional discounts, gift card applications, and multi-currency transactions with real-time total calculations.

## Basic Usage
```vue
<template>
  <div class="order-totals-container">
    <codex-order-totals
      :order="orderData"
      :show-breakdown="true"
      @totalCalculated="handleTotalCalculation"
    >
      <template #header>
        <h3>Order Summary</h3>
      </template>
      
      <template #footer>
        <div class="payment-info">
          <p>All prices include applicable taxes</p>
        </div>
      </template>
    </codex-order-totals>
  </div>
</template>

<script setup>
const orderData = ref({
  subtotal: 4999, // $49.99 in cents
  tax_amount: 400, // $4.00 in cents
  discount_amount: 500, // $5.00 in cents
  shipping_cost: 999, // $9.99 in cents
  total: 5398, // $53.98 in cents
  currency: 'USD',
  tax_breakdown: [
    { name: 'Sales Tax', rate: 8, amount: 400 }
  ],
  discounts: [
    { name: 'WELCOME10', amount: 500, type: 'percentage' }
  ]
})

const handleTotalCalculation = (totals) => {
  console.log('Order totals calculated:', totals)
}
</script>
```

## Key Features
- Comprehensive order total calculations and display
- Automatic currency formatting based on locale
- Tax breakdown with rates and amounts
- Discount and promotion handling
- Gift card and credit applications
- Shipping cost calculations
- Multi-currency support
- Real-time total updates
- Customizable display sections through slots

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `order` | `Object` | `required` | Order data containing pricing, taxes, and discounts |
| `showBreakdown` | `Boolean` | `true` | Whether to show detailed tax and discount breakdown |
| `currency` | `String` | `'USD'` | Currency code for formatting (overrides order currency) |
| `locale` | `String` | `'en-US'` | Locale for number formatting |

## Common Props

| Prop Name | Usage |
|-----------|-------|
| `formatCurrency` | Automatically formats all monetary values in the order totals |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `totalCalculated` | `totalsObject` | Emitted when order totals are calculated |
| `discountApplied` | `discountData` | Emitted when discount calculations are applied |
| `taxCalculated` | `taxData` | Emitted when tax calculations are completed |

## Slots

| Slot | Description |
|------|-------------|
| `header` | Custom header content above totals display |
| `breakdown` | Custom breakdown section for taxes and discounts |
| `footer` | Custom footer content below totals |

## Order Data Structure

### Basic Order Totals
```javascript
{
  subtotal: 4999,        // Subtotal in cents
  tax_amount: 400,       // Total tax in cents
  discount_amount: 500,  // Total discounts in cents
  shipping_cost: 999,    // Shipping cost in cents
  total: 5398,          // Final total in cents
  currency: 'USD'
}
```

### Detailed Order Totals
```javascript
{
  subtotal: 4999,
  tax_amount: 640,
  discount_amount: 1000,
  shipping_cost: 999,
  total: 5638,
  currency: 'USD',
  tax_breakdown: [
    { name: 'State Tax', rate: 8.5, amount: 425 },
    { name: 'City Tax', rate: 2.5, amount: 215 }
  ],
  discounts: [
    { name: 'WELCOME10', amount: 500, type: 'percentage', code: 'WELCOME10' },
    { name: 'Member Discount', amount: 500, type: 'fixed' }
  ],
  gift_cards: [
    { code: 'GC123456', amount: 2000, remaining: 1500 }
  ],
  credits: [
    { type: 'Store Credit', amount: 1000 }
  ]
}
```

## Internationalization

The component uses the following translation keys:

### Order Pricing Display
| Key | Usage |
|-----|-------|
| `cart.subtotal` | Subtotal line label |
| `cart.discount` | Discount line label |
| `cart.total` | Total price label |
| `cart.including` | Tax inclusion prefix (\"Including\") |
| `cart.in_taxes` | Tax suffix (\"in taxes\") |

### Translation Usage Examples
```vue
<!-- Subtotal section -->
<div class="_c-sub-total _c-content-column _c-divider">
  <div class="_c-price-container _c-text-bold _c-flex _c-justify-between">
    <div>{{ $t('cart.subtotal') }}</div>
    <div>{{ formatCurrency(order.base_price_raw) }}</div>
  </div>
  
  <div v-if="order.discount_raw > 0" class="_c-discount-container _c-text-sm _c-flex _c-justify-between">
    <div class="_c-voucher _c-flex" v-if="order.voucher">
      <div class="_c-text-bold">{{ $t('cart.discount') }}</div>
      <div>
        <i class="ri-price-tag-3-line"></i>
        <span class="_c-voucher-name">{{ order.voucher.code }}</span>
      </div>
    </div>
    <div>-{{ formatCurrency(order.discount_raw) }}</div>
  </div>
</div>

<!-- Total section -->
<div class="_c-total">
  <div class="_c-price-container _c-text-bold _c-flex _c-justify-between">
    <div>{{ $t('cart.total') }}</div>
    <div>{{ formatCurrency(order.final_price_raw) }}</div>
  </div>
  
  <div class="_c-taxes _c-text-sm">
    {{ $t('cart.including') }} {{ order.tax }} {{ $t('cart.in_taxes') }}
  </div>
</div>
```

### Translation Notes

#### Focused Translation Scope
The OrderTotals component has a focused set of translation requirements:
- Primary focus on pricing and financial summary terms
- Standard e-commerce terminology for subtotal, discount, and total
- Tax inclusion messaging for transparency

#### Payment Method Integration
The component displays payment method information with minimal translation needs:
- Gift card balances with last4 digits display
- Visual icons for payment methods (SVG-based)
- Automatic balance calculations with currency formatting

#### Currency Integration
The component heavily relies on currency formatting:
- Uses `formatCurrency` function for all monetary values
- Supports multi-currency display through configuration
- Maintains consistent formatting across all price elements

#### Voucher and Discount Display
The component integrates voucher information:
- Shows voucher codes alongside discount amounts
- Visual voucher indicators with icons
- Discount amount display with negative formatting

## Usage Examples

### Basic Order Totals
```vue
<codex-order-totals :order="order">
  <template #header>
    <h3>{{ $t('order.totals.title') }}</h3>
  </template>
</codex-order-totals>
```

### With Custom Breakdown
```vue
<codex-order-totals :order="order">
  <template #breakdown>
    <div class="custom-breakdown">
      <div class="subtotal">
        <span>{{ $t('order.totals.subtotal') }}:</span>
        <span>{{ formatCurrency(order.subtotal) }}</span>
      </div>
      <div class="tax">
        <span>{{ $t('order.totals.tax') }}:</span>
        <span>{{ formatCurrency(order.tax_amount) }}</span>
      </div>
    </div>
  </template>
</codex-order-totals>
```

### With International Features
```vue
<codex-order-totals 
  :order="internationalOrder"
  :show-currency-conversion="true"
>
  <template #footer>
    <div class="international-footer">
      <p>{{ $t('order.totals.currency_note', { currency: order.currency }) }}</p>
    </div>
  </template>
</codex-order-totals>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-order-totals` | Main totals container |
| `_c-totals-header` | Header section |
| `_c-breakdown` | Breakdown section container |
| `_c-tax-breakdown` | Tax details section |
| `_c-discount-breakdown` | Discount details section |
| `_c-subtotal-row` | Subtotal display row |
| `_c-tax-row` | Tax amount row |
| `_c-discount-row` | Discount amount row |
| `_c-shipping-row` | Shipping cost row |
| `_c-total-row` | Final total row |
| `_c-amount` | Amount display styling |
| `_c-label` | Label text styling |
| `_c-savings` | Savings highlight styling |
| `_c-totals-footer` | Footer section |

## Best Practices

### Order Data Structure
- Ensure all monetary values are in consistent units (cents)
- Include all necessary tax and fee information
- Provide clear breakdown of discounts and credits
- Structure data for easy internationalization

### Currency Handling
- Use consistent currency formatting throughout
- Support multiple currencies with proper conversion
- Display exchange rates when converting currencies
- Handle rounding appropriately for currency precision

### Tax Calculations
- Show tax breakdowns for transparency
- Include tax rates for user understanding
- Handle different tax systems (VAT, sales tax, etc.)
- Provide clear tax information and compliance

### User Experience
- Show real-time total updates as items change
- Provide clear breakdown of all charges
- Highlight savings and discounts prominently
- Use progressive disclosure for complex breakdowns

### Accessibility
- Use semantic HTML for financial information
- Provide clear labels for all amounts
- Ensure sufficient color contrast
- Make totals accessible to screen readers

### Performance
- Optimize for fast calculation and rendering
- Cache currency conversion rates appropriately
- Minimize re-calculations when possible
- Handle large numbers efficiently

## Component Registration
```javascript
// Global registration
app.component('CodexOrderTotals', OrderTotals)

// Local registration  
import OrderTotals from '@/components/Cart/OrderTotals.vue'

export default {
  components: {
    CodexOrderTotals: OrderTotals
  }
}
``` 