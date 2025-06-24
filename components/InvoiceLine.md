# InvoiceLine Component

## Overview
The InvoiceLine component renders individual line items within invoices, displaying product information, pricing, quantity, and tax details. It features customizable header, content, and footer sections through slots, automatic divider handling for multiple items, and structured presentation of tax information with rates. The component supports template overrides and integrates seamlessly with the Invoice component.

## Basic Usage
```vue
<template>
  <div class="invoice-lines-container">
    <codex-invoice-line
      v-for="(line, index) in invoiceLines"
      :key="index"
      :line="line"
      :line-key="index"
      :last-index="invoiceLines.length - 1"
    >
      <template #item-header>
        <div class="custom-header">
          <span class="product-code">{{ line.product_code }}</span>
          <span class="product-name">{{ line.product }}</span>
        </div>
      </template>
    </codex-invoice-line>
  </div>
</template>

<script setup>
const invoiceLines = ref([
  {
    product: 'Premium Plan',
    final_price: '$29.99',
    quantity: 1,
    tax_total: '$2.40',
    tax_rate: 8
  },
  {
    product: 'Setup Fee',
    final_price: '$9.99',
    quantity: 1,
    tax_total: '$0.80',
    tax_rate: 8
  }
])
</script>
```

## Key Features
- Individual invoice line item display
- Product name and pricing presentation
- Quantity and tax information display
- Automatic divider management between items
- Customizable sections through slots
- Template override support
- Tax rate and total calculations
- Responsive layout design

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `line` | `Object` | `required` | Line item data containing product, price, and tax information |
| `lineKey` | `Number` | `required` | Index of current line item for divider logic |
| `lastIndex` | `Number` | `undefined` | Index of last item to control divider display |

## Slots

| Slot | Description |
|------|-------------|
| `item-header` | Custom header content replacing product name and price |
| `item-content` | Custom content replacing quantity and tax information |
| `item-footer` | Custom footer content below line item details |

## Line Item Data Structure

### Basic Line Item
```javascript
{
  product: 'Product Name',
  final_price: '$29.99',
  quantity: 1,
  tax_total: '$2.40',
  tax_rate: 8.5
}
```

### Service Line Item
```javascript
{
  product: 'Professional Consultation',
  final_price: '$150.00',
  quantity: 2,
  tax_total: '$24.00',
  tax_rate: 8,
  description: 'One-hour consultation sessions',
  duration: '2 hours'
}
```

### Subscription Line Item
```javascript
{
  product: 'Monthly Subscription',
  final_price: '$0.00',
  ongoing_price: '$29.99/month',
  quantity: 1,
  tax_total: '$0.00',
  tax_rate: 0,
  trial_period: '14 days free'
}
```

## Internationalization

The `InvoiceLine` component uses translation keys for invoice line item details:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `sca.quantity` | Displays quantity label for line items | Product quantity in invoice |
| `sca.tax` | Shows tax information label | Tax amount and rate display |

### Implementation Examples

```vue
<!-- Quantity display -->
<div class="_c-invoice-quantity">
    {{ $t('sca.quantity') }}
    <span>{{ line.quantity }}</span>
</div>

<!-- Tax information -->
<div class="_c-taxes">
    {{ $t('sca.tax') }}
    <span>{{ line.tax_total }} ({{ line.tax_rate }}% tax)</span>
</div>
```

### Notes
- Minimal translation requirements focused on invoice line metadata
- Uses SCA (Strong Customer Authentication) namespace for financial terms
- Tax rate and totals are displayed as raw values from the line object
- Product name and pricing come directly from line object properties

## Examples

### Product Sales Invoice Line
```vue
<template>
  <div class="product-invoice-lines">
    <codex-invoice-line
      v-for="(item, index) in productLines"
      :key="item.id"
      :line="item"
      :line-key="index"
      :last-index="productLines.length - 1"
    >
      <template #item-header>
        <div class="product-header">
          <div class="product-info">
            <h4 class="product-name">{{ item.product }}</h4>
            <div class="product-sku">SKU: {{ item.sku }}</div>
          </div>
          <div class="price-info">
            <div class="unit-price">{{ item.unit_price }} each</div>
            <div class="total-price">{{ item.final_price }}</div>
          </div>
        </div>
      </template>
      
      <template #item-content>
        <div class="product-details">
          <div class="quantity-info">
            <span>{{ $t('sca.quantity') }}:</span>
            <span class="quantity-value">{{ item.quantity }}</span>
          </div>
          <div class="product-description" v-if="item.description">
            <span>Description:</span>
            <span>{{ item.description }}</span>
          </div>
          <div class="tax-info">
            <span>{{ $t('sca.tax') }}:</span>
            <span class="tax-details">
              {{ item.tax_total }} ({{ item.tax_rate }}% {{ item.tax_type || 'tax' }})
            </span>
          </div>
          <div class="discount-info" v-if="item.discount_amount">
            <span>Discount:</span>
            <span class="discount-amount">-{{ item.discount_amount }}</span>
          </div>
        </div>
      </template>
      
      <template #item-footer>
        <div class="product-footer">
          <div class="warranty-info" v-if="item.warranty">
            <i class="ri-shield-check-line"></i>
            <span>{{ item.warranty }} warranty included</span>
          </div>
        </div>
      </template>
    </codex-invoice-line>
  </div>
</template>

<script setup>
const productLines = ref([
  {
    id: 1,
    product: 'Wireless Headphones',
    sku: 'WH-1000XM4',
    unit_price: '$199.99',
    final_price: '$199.99',
    quantity: 1,
    tax_total: '$16.00',
    tax_rate: 8,
    tax_type: 'sales tax',
    description: 'Noise-canceling wireless headphones',
    warranty: '2-year'
  },
  {
    id: 2,
    product: 'Phone Case',
    sku: 'PC-IP14-BLK',
    unit_price: '$24.99',
    final_price: '$19.99',
    quantity: 2,
    tax_total: '$3.20',
    tax_rate: 8,
    discount_amount: '$10.00',
    description: 'Protective silicone case'
  }
])
</script>
```

### Service Invoice Line
```vue
<template>
  <div class="service-invoice-lines">
    <codex-invoice-line
      v-for="(service, index) in serviceLines"
      :key="service.id"
      :line="service"
      :line-key="index"
      :last-index="serviceLines.length - 1"
    >
      <template #item-header>
        <div class="service-header">
          <div class="service-info">
            <h4 class="service-name">{{ service.product }}</h4>
            <div class="service-category">{{ service.category }}</div>
          </div>
          <div class="service-pricing">
            <div class="hourly-rate">{{ service.hourly_rate }}/hour</div>
            <div class="total-amount">{{ service.final_price }}</div>
          </div>
        </div>
      </template>
      
      <template #item-content>
        <div class="service-details">
          <div class="duration-info">
            <span>Duration:</span>
            <span class="duration-value">{{ service.hours }} hours</span>
          </div>
          <div class="service-date">
            <span>Service Date:</span>
            <span>{{ formatDate(service.service_date) }}</span>
          </div>
          <div class="service-description">
            <span>Description:</span>
            <span>{{ service.description }}</span>
          </div>
          <div class="tax-info">
            <span>{{ $t('sca.tax') }}:</span>
            <span class="tax-details">
              {{ service.tax_total }} ({{ service.tax_rate }}% VAT)
            </span>
          </div>
        </div>
      </template>
      
      <template #item-footer>
        <div class="service-footer">
          <div class="consultant-info" v-if="service.consultant">
            <i class="ri-user-line"></i>
            <span>Consultant: {{ service.consultant }}</span>
          </div>
        </div>
      </template>
    </codex-invoice-line>
  </div>
</template>

<script setup>
const serviceLines = ref([
  {
    id: 1,
    product: 'Web Development Consultation',
    category: 'Technology Consulting',
    hourly_rate: '$150.00',
    final_price: '$600.00',
    hours: 4,
    quantity: 1,
    tax_total: '$48.00',
    tax_rate: 8,
    service_date: '2024-01-15',
    description: 'Frontend architecture review and recommendations',
    consultant: 'John Smith'
  },
  {
    id: 2,
    product: 'SEO Audit',
    category: 'Marketing Services',
    hourly_rate: '$120.00',
    final_price: '$240.00',
    hours: 2,
    quantity: 1,
    tax_total: '$19.20',
    tax_rate: 8,
    service_date: '2024-01-16',
    description: 'Comprehensive website SEO analysis',
    consultant: 'Jane Doe'
  }
])

const formatDate = (date) => {
  return new Date(date).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}
</script>
```

### Subscription Invoice Line
```vue
<template>
  <div class="subscription-invoice-lines">
    <codex-invoice-line
      v-for="(subscription, index) in subscriptionLines"
      :key="subscription.id"
      :line="subscription"
      :line-key="index"
      :last-index="subscriptionLines.length - 1"
    >
      <template #item-header>
        <div class="subscription-header">
          <div class="plan-info">
            <h4 class="plan-name">
              {{ subscription.product }}
              <span class="plan-tier">{{ subscription.tier }}</span>
            </h4>
            <div class="billing-cycle">{{ subscription.billing_cycle }}</div>
          </div>
          <div class="subscription-pricing">
            <div v-if="subscription.setup_fee" class="setup-fee">
              Setup: {{ subscription.setup_fee }}
            </div>
            <div class="recurring-price">{{ subscription.final_price }}</div>
          </div>
        </div>
      </template>
      
      <template #item-content>
        <div class="subscription-details">
          <div class="billing-period">
            <span>Billing Period:</span>
            <span>{{ subscription.billing_start }} - {{ subscription.billing_end }}</span>
          </div>
          <div v-if="subscription.trial_days" class="trial-info">
            <span>Trial Period:</span>
            <span class="trial-highlight">{{ subscription.trial_days }} days free</span>
          </div>
          <div class="features-included">
            <span>Features:</span>
            <ul class="feature-list">
              <li v-for="feature in subscription.features" :key="feature">
                {{ feature }}
              </li>
            </ul>
          </div>
          <div class="tax-info">
            <span>{{ $t('sca.tax') }}:</span>
            <span class="tax-details">
              {{ subscription.tax_total }} ({{ subscription.tax_rate }}% VAT)
            </span>
          </div>
        </div>
      </template>
      
      <template #item-footer>
        <div class="subscription-footer">
          <div class="cancellation-policy">
            <i class="ri-information-line"></i>
            <span>Cancel anytime. No long-term contracts.</span>
          </div>
        </div>
      </template>
    </codex-invoice-line>
  </div>
</template>

<script setup>
const subscriptionLines = ref([
  {
    id: 1,
    product: 'Premium Plan',
    tier: 'Pro',
    billing_cycle: 'Monthly',
    final_price: '$29.99/month',
    setup_fee: '$9.99',
    quantity: 1,
    tax_total: '$3.20',
    tax_rate: 8,
    billing_start: 'Feb 1, 2024',
    billing_end: 'Feb 29, 2024',
    trial_days: 14,
    features: [
      'Unlimited API calls',
      'Priority support',
      'Advanced analytics',
      '10GB storage'
    ]
  }
])
</script>
```

### International Tax Invoice Line
```vue
<template>
  <div class="international-invoice-lines">
    <codex-invoice-line
      v-for="(item, index) in internationalLines"
      :key="item.id"
      :line="item"
      :line-key="index"
      :last-index="internationalLines.length - 1"
    >
      <template #item-header>
        <div class="international-header">
          <div class="product-info">
            <h4 class="product-name">{{ item.product }}</h4>
            <div class="origin-country">Made in {{ item.origin_country }}</div>
          </div>
          <div class="pricing-info">
            <div class="base-price">{{ item.base_price }} {{ item.base_currency }}</div>
            <div class="converted-price">{{ item.final_price }} {{ item.display_currency }}</div>
          </div>
        </div>
      </template>
      
      <template #item-content>
        <div class="international-details">
          <div class="quantity-info">
            <span>{{ $t('sca.quantity') }}:</span>
            <span class="quantity-value">{{ item.quantity }}</span>
          </div>
          <div class="currency-conversion" v-if="item.exchange_rate">
            <span>Exchange Rate:</span>
            <span>1 {{ item.base_currency }} = {{ item.exchange_rate }} {{ item.display_currency }}</span>
          </div>
          <div class="tax-breakdown">
            <div class="vat-info" v-if="item.vat_total">
              <span>VAT ({{ item.vat_rate }}%):</span>
              <span>{{ item.vat_total }}</span>
            </div>
            <div class="duty-info" v-if="item.customs_duty">
              <span>Customs Duty:</span>
              <span>{{ item.customs_duty }}</span>
            </div>
            <div class="total-tax">
              <span>Total {{ $t('sca.tax') }}:</span>
              <span class="tax-amount">{{ item.tax_total }}</span>
            </div>
          </div>
        </div>
      </template>
      
      <template #item-footer>
        <div class="international-footer">
          <div class="shipping-info" v-if="item.shipping_method">
            <i class="ri-truck-line"></i>
            <span>{{ item.shipping_method }} - {{ item.delivery_time }}</span>
          </div>
        </div>
      </template>
    </codex-invoice-line>
  </div>
</template>

<script setup>
const internationalLines = ref([
  {
    id: 1,
    product: 'Swiss Watch',
    origin_country: 'Switzerland',
    base_price: '250.00',
    base_currency: 'CHF',
    final_price: '$275.00',
    display_currency: 'USD',
    exchange_rate: '1.10',
    quantity: 1,
    vat_rate: 7.7,
    vat_total: '$19.25',
    customs_duty: '$5.50',
    tax_total: '$24.75',
    shipping_method: 'Express International',
    delivery_time: '3-5 business days'
  }
])
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-cart-item` | Main line item container |
| `_c-divider` | Divider styling between items |
| `_c-item-header` | Header section container |
| `_c-flex` | Flexbox layout |
| `_c-justify-between` | Space between flex items |
| `_c-desc` | Product description styling |
| `_c-text-bold` | Bold text styling |
| `_c-price-container` | Price display container |
| `_c-price` | Price text styling |
| `_c-item-content` | Content section container |
| `_c-invoice-quantity` | Quantity display styling |
| `_c-taxes` | Tax information styling |
| `_c-item-footer` | Footer section container |

## Best Practices

### Line Item Data Structure
- Ensure all required fields (product, final_price, quantity) are present
- Include tax information for proper invoice compliance
- Use consistent currency formatting across all line items
- Provide meaningful product names and descriptions

### Tax Handling
- Display tax rates as percentages with appropriate decimal places
- Show both tax amounts and rates for transparency
- Handle multiple tax types (VAT, sales tax, customs duty) appropriately
- Ensure tax calculations are accurate and compliant

### Customization
- Use slots to customize presentation without breaking functionality
- Maintain consistent styling with parent Invoice component
- Consider responsive design for mobile invoice viewing
- Implement appropriate spacing and dividers between items

### International Support
- Handle multiple currencies and exchange rates
- Support various tax systems (VAT, GST, sales tax)
- Display origin countries for international products
- Include customs and duty information where applicable

### Accessibility
- Use semantic HTML structure for line item information
- Provide clear labels for screen readers
- Ensure sufficient color contrast for all text
- Make interactive elements keyboard accessible

### Performance
- Optimize for rendering large numbers of line items
- Use efficient list rendering techniques
- Minimize re-renders when line item data changes
- Consider virtualization for very long item lists

## Component Registration
```javascript
// Global registration
app.component('CodexInvoiceLine', InvoiceLine)

// Local registration  
import InvoiceLine from '@/components/Cart/InvoiceLine.vue'

export default {
  components: {
    CodexInvoiceLine: InvoiceLine
  }
}
``` 