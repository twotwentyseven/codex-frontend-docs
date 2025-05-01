# Gift Card Component

## Overview
The Gift Card component displays gift card products in a card format, supporting features like variable value selection, price formatting, and cart integration. It handles both single-value and multi-variant gift cards with a clean, modern interface.

## Basic Usage
```vue
<codex-giftcard-card
  :product="giftCardData"
  :show-price="true"
/>
```

## Key Features
- Skeleton loading state
- Multiple value variants support
- Cart integration
- Price formatting
- Featured card styling
- Expiry information display
- Conditional content
- Value selection
- Error handling
- Toast notifications
- Custom branding display

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| product | Object | Yes | - | Gift card product data |
| hideIfDisabled | Boolean | No | false | Hide card if gift card cannot be purchased |
| showPrice | Boolean | No | false | Show price in add to cart button |
| showTitle | Boolean | No | true | Show gift card title |
| loading | Boolean | No | false | Show skeleton loading state |

### Common Props
The component uses the following common props from `@/config/common`:

| Prop Name | Usage |
|-----------|-------|
| enableBorder | Controls card border visibility |
| className | Adds custom CSS classes |
| showPoweredBy | Controls branding visibility |

### Common Functions
The component uses the following utilities from `useCommon`:

| Function | Usage |
|----------|-------|
| formatCurrency | Formats gift card prices and variants |
| isMobile | Handles responsive layout adjustments |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| add | - | Emitted when gift card is added to cart |

## Slots

### Header Slot
```vue
<template #header="slotProps">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="slotProps">
  <!-- Custom content -->
</template>
```

### Footer Slot
```vue
<template #footer="slotProps">
  <!-- Custom footer content -->
</template>
```

### Slot Props
| Prop Name | Type | Description |
|-----------|------|-------------|
| bundle | Object | Gift card data object |
| add | Function | Add to cart function |
| adding | Boolean | Loading state for add action |
| added | Boolean | Success state for add action |
| error | Object | Error state object |
| fieldErrors | Object | Field-specific errors |
| genericErrors | Array | Generic error messages |
| formatCurrency | Function | Currency formatting function |
| canPurchase | Boolean | Whether gift card can be purchased |
| isInCart | Boolean | Whether gift card is in cart |
| selectedVariant | String/Boolean | Selected gift card variant |

## States
1. Loading (skeleton)
2. Default
3. Featured
4. Value Selection Required
5. Error
6. Adding to Cart
7. Added Success

## Internationalization
The component uses the following translation keys:
- `giftcard.from`: Starting price indicator
- `giftcard.expiry`: Expiry information
- `giftcard.choose_value`: Value selection prompt
- `product.adding`: Adding to cart text
- `product.add_to_cart`: Add to cart button text
- `product.unavailable`: Unavailable state text
- `product.error`: Error state text
- `cart.added_to_cart`: Success toast message

## Examples

### Basic Gift Card
```vue
<codex-giftcard-card
  :product="giftCard"
  :show-price="true"
  :show-title="true"
/>
```

### Featured Gift Card with Custom Content
```vue
<codex-giftcard-card
  :product="giftCard"
  class="featured-giftcard"
>
  <template #content="{ bundle, formatCurrency }">
    <div class="custom-content">
      <h2>{{ bundle.name }}</h2>
      <div class="price">{{ formatCurrency(bundle.variants[0].price) }}</div>
      <div class="expiry">Valid for {{ bundle.expires_offset }}</div>
    </div>
  </template>
</codex-giftcard-card>
```

### Gift Card with Custom Value Selection
```vue
<codex-giftcard-card
  :product="giftCard"
>
  <template #footer="{ selectedVariant }">
    <div class="value-selector">
      <codex-select-field
        :settings="giftCard.variants"
        v-model="selectedVariant"
        :placeholder="$t('giftcard.choose_value')"
      />
    </div>
  </template>
</codex-giftcard-card>
```

### Gift Card with Custom Error Handling
```vue
<codex-giftcard-card
  :product="giftCard"
>
  <template #footer="{ genericErrors }">
    <div class="error-container">
      <codex-error :error="genericErrors" />
    </div>
  </template>
</codex-giftcard-card>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-giftcard-card`: Gift card specific class
- `_c-featured`: Featured styling
- `_c-header`: Header section
- `_c-title`: Title styling
- `_c-product-title`: Product title styling
- `_c-content`: Content section
- `_c-focal-text`: Emphasized text styling
- `_c-product-price`: Price styling
- `_c-from`: From price indicator
- `_c-product-expiry`: Expiry text styling
- `_c-text-sm`: Small text styling
- `_c-footer`: Footer section
- `_c-btn-container`: Button container
- `_c-product-btn`: Product button styling

## Best Practices

### Recommended Usage
- Use skeleton loading for data fetching
- Implement clear pricing display
- Show appropriate value options
- Handle all variant states
- Provide clear value selection
- Use consistent styling
- Consider mobile layouts
- Display expiry information clearly

### Accessibility Considerations
- Use proper heading hierarchy
- Ensure button accessibility
- Provide clear error messages
- Maintain keyboard navigation
- Use appropriate ARIA labels
- Consider color contrast
- Handle focus states
- Make value selection accessible

### Performance Considerations
- Optimize variant handling
- Handle cart updates efficiently
- Manage state transitions
- Implement proper caching
- Optimize price calculations
- Handle loading states
- Consider lazy loading
- Cache variant options

### Common Pitfalls to Avoid
- Missing loading states
- Unclear pricing information
- Poor error handling
- Inconsistent styling
- Missing validation
- Poor mobile experience
- Unclear value selection
- Missing expiry information

### State Management
- Track selected variant
- Handle value selection
- Manage loading states
- Track error states
- Handle success feedback
- Maintain gift card status
- Update UI consistently
- Monitor cart state

## Component Registration
The component is registered as `codex-giftcard-card` in the application.

## Implementation Details

### Currency Formatting
The component uses the common `formatCurrency` utility for consistent price display:
```javascript
const { formatCurrency } = useCommon(props);

// Usage in template
<div class="_c-product-price">
  {{ formatCurrency(currentVariant[0].price) }}
</div>
```

### Border Handling
The component respects the common `enableBorder` prop:
```vue
<div class="_c-card _c-giftcard-card" 
  :class="{'_c-border': enableBorder}">
``` 