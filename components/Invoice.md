# Invoice Component

## Overview
The Invoice component provides a structured display for invoice information with line items, totals, and tax details. It features customizable header, content, and footer sections through slots, automatic currency formatting, and integration with InvoiceLine components for detailed item breakdown. The component supports flexible layout customization while maintaining consistent invoice presentation standards.

## Basic Usage
```vue
<template>
  <div class="invoice-container">
    <codex-invoice
      :invoice="invoiceData"
    >
      <template #header>
        <div class="custom-header">
          <h2>Invoice #{{ invoiceData.invoice_number }}</h2>
          <div class="invoice-date">{{ invoiceData.created_at }}</div>
        </div>
      </template>
      
      <template #footer>
        <div class="payment-info">
          <p>Payment due within 30 days</p>
        </div>
      </template>
    </codex-invoice>
  </div>
</template>

<script setup>
const invoiceData = ref({
  invoice_number: 'INV-2024-001',
  amount_due: 2999, // In cents
  discounts: 500,
  tax: '$2.40',
  order_lines: [
    {
      product: 'Premium Plan',
      final_price: '$29.99',
      quantity: 1,
      tax_total: '$2.40',
      tax_rate: 8.5
    }
  ]
})
</script>
```

## Key Features
- Structured invoice layout with header, content, and footer sections
- Automatic invoice line item rendering
- Currency formatting for all monetary values  
- Customizable invoice sections through slots
- Integrated tax and discount calculations
- Responsive design for various screen sizes
- Template override support for complete customization

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `invoice` | `Object` | `required` | Invoice data object containing line items and totals |

## Common Props

| Prop Name | Usage |
|-----------|-------|
| `commonProps` | Standard component props for consistency |

## Common Functions

| Function | Usage |
|----------|-------|
| `formatCurrency` | Formats all monetary values (amount_due, discounts) in the invoice |

## Slots

| Slot | Description |
|------|-------------|
| `header` | Custom header content above invoice details |
| `content` | Complete content replacement for invoice body |
| `footer` | Custom footer content below invoice totals |

## Invoice Data Structure

### Basic Invoice
```javascript
{
  invoice_number: 'INV-2024-001',
  amount_due: 2999, // Amount in cents
  discounts: 500,   // Discount in cents
  tax: '$2.40',     // Formatted tax amount
  order_lines: [
    {
      product: 'Product Name',
      final_price: '$29.99',
      quantity: 1,
      tax_total: '$2.40',
      tax_rate: 8.5
    }
  ]
}
```

### Subscription Invoice
```javascript
{
  invoice_number: 'INV-SUB-2024-002',
  amount_due: 0, // Trial period
  discounts: 2999,
  tax: '$0.00',
  order_lines: [
    {
      product: 'Premium Subscription',
      final_price: '$0.00',
      ongoing_price: '$29.99/month',
      quantity: 1,
      trial_days: 14
    }
  ]
}
```

## Internationalization

The component uses the following translation keys:

### Invoice Totals Display
| Key | Usage |
|-----|-------|
| `invoice.total` | Invoice total amount label |
| `invoice.discounts` | Discounts line label |
| `invoice.including` | Tax inclusion prefix (\"Including\") |
| `invoice.in_taxes` | Tax suffix (\"in taxes\") |

### Translation Usage Examples
```vue
<!-- Invoice footer with totals -->
<div class="_c-footer">
  <div class="_c-total _c-divider">
    <div class="_c-price-container _c-text-bold _c-flex _c-justify-between">
      <div>{{ $t('invoice.total')}}</div>
      <div>{{ formatCurrency(invoice.amount_due) }}</div>
    </div>
    
    <div class="_c-discounts _c-text-sm">
      {{ $t('invoice.discounts') }} {{ formatCurrency(invoice.discounts) }}
    </div>
    
    <div class="_c-taxes _c-text-sm">
      {{ $t('invoice.including') }} {{invoice.tax}} {{ $t('invoice.in_taxes') }}
    </div>
  </div>
</div>
```

### Translation Notes

#### Minimal Translation Requirements
The Invoice component has minimal direct translation needs as it primarily:
- Displays financial totals with currency formatting
- Uses `codex-invoice-line` child components for line item details
- Focuses on structured invoice layout rather than user interface

#### Child Component Integration
The Invoice component delegates most translation work to child components:
- `codex-invoice-line` components handle individual line item translations
- Invoice lines contain product names, descriptions, and pricing details
- Parent components typically provide invoice-specific context and headers

#### Currency Formatting Integration
The component uses `formatCurrency` function for all monetary displays:
- Automatic formatting based on locale settings
- Consistent currency presentation across all invoice elements
- Integration with business currency configuration

## Usage Examples

### Basic Invoice Display
```vue
<codex-invoice :invoice="invoice">
  <template #header>
    <h1>{{ $t('invoice.title') }}</h1>
    <p>{{ $t('invoice.number', { number: invoice.invoice_number }) }}</p>
  </template>
</codex-invoice>
```

### With Custom Actions
```vue
<codex-invoice :invoice="invoice">
  <template #footer>
    <div class="custom-actions">
      <codex-button 
        :default-text="$t('invoice.actions.download_pdf')"
        @click="downloadPDF"
      />
      <codex-button 
        :default-text="$t('invoice.actions.email')"
        @click="emailInvoice"
      />
    </div>
  </template>
</codex-invoice>
```

### With Error Handling
```vue
<codex-invoice 
  :invoice="invoice"
  @error="handleInvoiceError"
>
  <template #error="{ error }">
    <div class="invoice-error">
      <h3>{{ $t('invoice.error.title') }}</h3>
      <p>{{ $t('invoice.error.message') }}</p>
    </div>
  </template>
</codex-invoice>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-invoice` | Main invoice container |
| `codex` | Base component styling |
| `_c-header` | Header section |
| `_c-content` | Main content area |
| `_c-order-content` | Order-specific content styling |
| `_c-card` | Card container for invoice items |
| `_c-cart-card` | Cart-style card layout |
| `_c-line-items` | Line items container |
| `_c-footer` | Footer section |
| `_c-total` | Total section container |
| `_c-content-column` | Column layout for content |
| `_c-divider` | Divider line styling |
| `_c-price-container` | Price display container |
| `_c-text-bold` | Bold text styling |
| `_c-flex` | Flexbox container |
| `_c-justify-between` | Space between flex items |
| `_c-discounts` | Discount section styling |
| `_c-text-sm` | Small text styling |
| `_c-taxes` | Tax information section |

## Best Practices

### Invoice Data Structure
- Ensure all monetary values are in consistent units (cents for calculations)
- Provide formatted strings for display where appropriate
- Include all necessary tax and discount information
- Structure line items with complete product details

### Customization
- Use slots for custom branding and layout requirements
- Maintain consistent styling with the rest of the application
- Consider responsive design for mobile invoice viewing
- Implement print-friendly styles when needed

### Accessibility
- Provide clear headings and structure for screen readers
- Use appropriate table markup for line items when needed
- Ensure sufficient color contrast for all text
- Include descriptive labels for all monetary amounts

### Internationalization
- Support multiple currencies and tax systems
- Localize date formats and number formatting
- Provide translated labels for all invoice sections
- Handle right-to-left text direction where needed

### Error Handling
- Validate invoice data structure before rendering
- Provide fallback displays for missing information
- Handle currency formatting errors gracefully
- Log rendering errors for debugging

### Performance
- Optimize for fast invoice rendering with large line item lists
- Consider virtualization for very long invoices
- Minimize re-renders when invoice data changes
- Cache formatted currency values where appropriate

## Component Registration
```javascript
// Global registration
app.component('CodexInvoice', Invoice)

// Local registration  
import Invoice from '@/components/Cart/Invoice.vue'

export default {
  components: {
    CodexInvoice: Invoice
  }
}
``` 