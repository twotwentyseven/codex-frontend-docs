# GiftCardCard Component

## Overview
The GiftCardCard component displays individual gift card products with comprehensive cart integration, variant selection, and customizable content. It provides a complete interface for showcasing gift card details, pricing options, and purchase functionality with support for multiple denominations, loading states, and extensive customization. The component handles variant selection automatically and integrates seamlessly with the cart system.

## Basic Usage
```vue
<template>
  <div class="gift-card-display">
    <codex-gift-card-card 
      :product="selectedGiftCard"
      :enable-border="true"
      :show-price="true"
      :show-title="true"
    />
  </div>
</template>

<script setup>
const selectedGiftCard = {
  id: 1,
  name: 'Store Gift Card',
  handle: 'store-gift-card',
  featured: false,
  expires_offset: '12 months',
  variants: [
    { id: 1, formatted_price: '$25.00' },
    { id: 2, formatted_price: '$50.00' },
    { id: 3, formatted_price: '$100.00' }
  ]
}
</script>
```

## Key Features
- Individual gift card display with detailed information
- Cart integration with add-to-cart functionality
- Multiple variant/denomination support
- Automatic variant selection for single variants
- Loading states with skeleton card
- Conditional rendering and display options
- Custom SVG branding integration
- Pricing display with currency formatting
- Internationalization support
- Extensive slot customization

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `product` | `Object\|Number` | `required` | Gift card object or ID |

### Display Control Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `loading` | `Boolean` | `false` | Show skeleton loading state |
| `hideIfDisabled` | `Boolean` | `false` | Hide card if gift card cannot be purchased |
| `showPrice` | `Boolean` | `false` | Display price in button text |
| `showTitle` | `Boolean` | `true` | Display gift card name in header |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Enables border styling on the gift card |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatCurrency` | Formats gift card prices for display |

## Computed Properties

### Variant Management
- `hasVariants` - Whether gift card has multiple variants
- `currentVariant` - Currently selected variant object
- `giftcard` - Reference to the product prop

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| N/A | N/A | Component uses internal cart system |

## Slots

### Header Slot
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ giftcard, add, adding, added, error, fieldErrors, genericErrors, formatCurrency, selectedVariant }` | Custom header content |

### Content Slot
| Slot | Props | Description |
|------|-------|-------------|
| `content` | `{ giftcard, add, adding, added, error, fieldErrors, genericErrors, formatCurrency, selectedVariant }` | Main gift card content |

### Footer Slot
| Slot | Props | Description |
|------|-------|-------------|
| `footer` | `{ giftcard, add, adding, added, error, fieldErrors, genericErrors, formatCurrency, selectedVariant }` | Footer with variant selection and purchase button |

## Loading States

The component provides comprehensive loading states:
- Skeleton card with structured layout during loading
- Loading indicators on purchase buttons
- Conditional rendering based on data availability
- Loading states for cart operations

## Variant Selection

The component handles variant selection automatically:
- Single variant: Auto-selected on mount
- Multiple variants: Dropdown selection required
- Purchase button disabled until variant selected
- Price display updates based on selection

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-card` | Base card styling |
| `_c-giftcard-card` | Gift card specific styling |
| `_c-featured` | Featured gift card styling |
| `_c-header` | Header section styling |
| `_c-content` | Content area styling |
| `_c-footer` | Footer section styling |
| `_c-title` | Title styling |
| `_c-product-title` | Product title styling |
| `_c-focal-text` | Prominent text styling |
| `_c-product-price` | Price display styling |
| `_c-from` | "From" price prefix styling |
| `_c-product-expiry` | Expiry information styling |
| `_c-text-sm` | Small text styling |
| `_c-btn-container` | Button container styling |
| `_c-product-btn` | Product button styling |

## Internationalization

The component uses the following translation keys:

### Core Gift Card Information
| Key | Usage |
|-----|-------|
| `giftcards.from` | "From" prefix for variable pricing |
| `giftcards.expiry` | Expiry information label |
| `giftcards.choose_value` | Variant selection label and placeholder |

### Cart and Purchase Actions
| Key | Usage |
|-----|-------|
| `product.add_to_cart` | Add to cart button text |
| `product.adding` | Adding to cart loading state |
| `product.unavailable` | Unavailable state text |
| `product.error` | Generic error state text |
| `cart.added_to_cart` | Success toast message |

### Translation Usage Examples
```vue
<!-- Gift card pricing display -->
<div class="gift-card-price">
  <template v-if="!hasVariants || selectedVariant == false">
    <span v-if="selectedVariant == false" class="from-text">
      {{ $t("giftcards.from") }}
    </span>
    {{ giftcard.variants[0].formatted_price }}
  </template>
</div>

<!-- Gift card expiry information -->
<div v-if="giftcard.expires_offset" class="gift-card-expiry">
  {{ $t("giftcards.expiry") }} {{ giftcard.expires_offset }}
</div>

<!-- Variant selection dropdown -->
<codex-select-field 
  v-if="hasVariants"
  option-name="formatted_price"
  option-value="id"
  :label="$t('giftcards.choose_value')"
  :options="giftcard.variants"
  v-model="selectedVariant"
  :placeholder="$t('giftcards.choose_value')"
/>

<!-- Add to cart button with states -->
<codex-button
  :processingText="$t('product.adding')"
  :defaultText="$t('product.add_to_cart')"
  :disabledText="$t('product.unavailable')"
  :errorText="$t('product.error')"
  :processing="adding"
  :disabled="selectedVariant == false"
  @click="add"
>
  <template v-if="showPrice" #after>
    - {{ currentVariant.formatted_price }}
  </template>
</codex-button>
```

### Additional Translation Considerations

#### Variant Handling
- The component automatically handles variant selection and pricing display
- Translation keys support both single and multiple variant scenarios
- The "from" prefix is only shown for multiple variants when no specific variant is selected

#### Cart Integration
- Uses standard product translation keys for consistency with other product cards
- Success messages are displayed via toast notifications using `cart.added_to_cart`
- Error handling follows the same pattern as other cart-enabled components

#### Expiry Information
- The `giftcards.expiry` key is used to display expiration information
- Actual expiry periods come from the product data and may need localization based on the time format preferences

## Best Practices

### Gift Card Display
- Use clear, attractive visual presentation
- Provide comprehensive pricing information
- Support multiple denomination options effectively
- Include relevant expiry and usage information
- Implement proper variant selection UX

### User Experience
- Auto-select single variants for immediate purchase
- Show loading states during cart operations
- Provide clear feedback for selection states
- Implement smooth cart integration
- Handle errors gracefully with user feedback

### Performance
- Use skeleton loading for better perceived performance
- Optimize gift card imagery for fast loading
- Cache variant data appropriately
- Implement efficient cart operations
- Consider lazy loading for multiple cards

### Accessibility
- Ensure proper keyboard navigation through variant selection
- Provide descriptive labels for all form inputs
- Use appropriate ARIA attributes for dynamic content
- Test with screen readers for gift card information
- Maintain sufficient color contrast for all elements

### Security
- Implement proper gift card validation
- Use secure payment processing
- Validate variant selections server-side
- Monitor for fraudulent purchases
- Implement proper gift card code generation

### Mobile Responsiveness
- Ensure variant selection works well on touch devices
- Optimize card layout for small screens
- Test purchase flow on mobile devices
- Consider collapsible content for detailed information
- Maintain readability across all screen sizes

## Component Registration
```javascript
// Global registration
app.component('CodexGiftCardCard', GiftCardCard)

// Local registration  
import GiftCardCard from '@/components/products/GiftCardCard.vue'

export default {
  components: {
    CodexGiftCardCard: GiftCardCard
  }
}
``` 