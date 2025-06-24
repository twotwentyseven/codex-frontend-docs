# Bundle Card Component

## Overview
The Bundle Card component displays product bundle information in a card format, supporting features like pricing, credits, purchase limitations, cart integration, and loading states. It provides a flexible layout with customizable sections and responsive design.

## Basic Usage
```vue
<codex-bundle-card
  :product="bundleData"
  :show-price="true"
/>
```

## Key Features
- Skeleton loading state
- Purchase validation
- Cart integration
- Price formatting
- Credits display
- Badge support
- Featured bundle styling
- Expiry information
- Conditional content
- Internationalization support
- Error handling
- Toast notifications

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| product | Object | Yes | - | Bundle product data |
| purchasedBundles | Object | No | [] | Array of previously purchased bundles |
| hideIfDisabled | Boolean | No | false | Hide card if bundle cannot be purchased |
| showPrice | Boolean | No | true | Show price in add to cart button |
| showPricePerCredit | Boolean | No | false | Show price per credit calculation |
| loading | Boolean | No | false | Show skeleton loading state |

### Common Props
The component uses the following common props from `@/config/common`:

| Prop Name | Usage |
|-----------|-------|
| enableBorder | Controls card border visibility |
| className | Adds custom CSS classes |
| showPoweredBy | Controls branding visibility |
| title | Optional custom title override |
| titleTag | HTML tag for the title (h1-h6, div) |

### Common Functions
The component uses the following utilities from `useCommon`:

| Function | Usage |
|----------|-------|
| formatCurrency | Formats bundle prices and per-credit calculations |
| isMobile | Handles responsive layout adjustments |
| loadingItems | Manages skeleton loading state |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| add | - | Emitted when bundle is added to cart |

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
| bundle | Object | Bundle data object |
| add | Function | Add to cart function |
| adding | Boolean | Loading state for add action |
| added | Boolean | Success state for add action |
| error | Object | Error state object |
| fieldErrors | Object | Field-specific errors |
| genericErrors | Array | Generic error messages |
| formatCurrency | Function | Currency formatting function |
| canPurchase | Boolean | Whether bundle can be purchased |
| isInCart | Boolean | Whether bundle is in cart |

## States
1. Loading (skeleton)
2. Default
3. Featured
4. Disabled
5. In Cart
6. Error
7. Adding to Cart
8. Added Success

## Internationalization

The component uses the following translation keys:

### Core Product Information
| Key | Usage |
|-----|-------|
| `product.title` | Default product title when title prop is not provided |
| `product.whats_included` | What's included section title |
| `product.description` | Product description fallback |

### Bundle-Specific Information
| Key | Usage |
|-----|-------|
| `bundles.credits` | Credits text with pluralization (e.g., "10 credits") |
| `bundles.credit` | Single credit text for price per credit display |
| `bundles.per_separator` | Price per credit separator ("per") |
| `bundles.expiry_prefix` | Expiry text prefix ("Expires in") |

### Bundle Expiry and Time Translation
| Key | Usage |
|-----|-------|
| `bundles.days` | Days time unit for expiry display |
| `bundles.weeks` | Weeks time unit for expiry display |
| `bundles.months` | Months time unit for expiry display |
| `bundles.years` | Years time unit for expiry display |

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
<!-- Bundle credits display with pluralization -->
<div class="bundle-credits">
  {{ bundle.total_credits }} {{ $t('bundles.credits', bundle.total_credits) }}
</div>

<!-- Price per credit calculation -->
<div v-if="showPricePerCredit" class="price-per-credit">
  {{ formatCurrency(bundle.price / bundle.total_credits) }} 
  {{ $t('bundles.per_separator') }} 
  {{ $t('bundles.credit') }}
</div>

<!-- Bundle expiry information -->
<div v-if="bundle.expires_offset" class="bundle-expiry">
  {{ $t('bundles.expiry_prefix') }} {{ translatedExpiry(bundle.expires_offset) }}
</div>

<!-- Add to cart button with multiple states -->
<codex-button
  :processingText="$t('product.adding')"
  :defaultText="$t('product.add_to_cart')"
  :disabledText="$t('product.unavailable')"
  :errorText="$t('product.error')"
  @click="add"
/>
```

## Examples

### Basic Bundle Card
```vue
<codex-bundle-card
  :product="bundle"
  :show-price="true"
  :show-price-per-credit="true"
/>
```

### Featured Bundle with Custom Content
```vue
<codex-bundle-card
  :product="featuredBundle"
  class="featured-bundle"
>
  <template #content="{ bundle, formatCurrency }">
    <div class="custom-content">
      <h2>{{ bundle.name }}</h2>
      <div class="price">{{ formatCurrency(bundle.price) }}</div>
      <div class="credits">{{ bundle.total_credits }} Credits</div>
    </div>
  </template>
</codex-bundle-card>
```

### Bundle with Custom Error Handling
```vue
<codex-bundle-card
  :product="bundle"
>
  <template #footer="{ genericErrors }">
    <div class="error-container">
      <codex-error :error="genericErrors" />
    </div>
  </template>
</codex-bundle-card>
```

### Loading State
```vue
<codex-bundle-card
  :product="bundle"
  :loading="true"
/>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-bundle-card`: Bundle specific class
- `_c-border`: Border styling
- `_c-in-cart`: In cart state
- `_c-disabled`: Disabled state
- `_c-featured`: Featured bundle styling
- `_c-badge`: Badge container
- `_c-product-badge`: Product badge styling
- `_c-content-column`: Content column layout
- `_c-product-title`: Product title styling
- `_c-product-desc`: Product description styling
- `_c-additional-content`: Additional content section
- `_c-subtitle`: Subtitle styling
- `_c-custom-list`: Custom list styling
- `_c-focal-text`: Emphasized text styling
- `_c-product-price`: Price styling
- `_c-product-credits`: Credits display styling
- `_c-price-per-credit`: Price per credit styling
- `_c-btn-container`: Button container
- `_c-product-btn`: Product button styling
- `_c-product-expiry`: Expiry text styling

## Best Practices

### Recommended Usage
- Use skeleton loading for data fetching
- Implement clear pricing display
- Show appropriate purchase limitations
- Handle all cart states
- Provide clear error feedback
- Use consistent styling
- Consider mobile layouts

### Accessibility Considerations
- Use proper heading hierarchy
- Ensure button accessibility
- Provide clear error messages
- Maintain keyboard navigation
- Use appropriate ARIA labels
- Consider color contrast
- Handle focus states

### Performance Considerations
- Optimize image loading
- Handle cart updates efficiently
- Manage state transitions
- Implement proper caching
- Optimize price calculations
- Handle loading states
- Consider lazy loading

### Common Pitfalls to Avoid
- Missing loading states
- Unclear pricing information
- Poor error handling
- Inconsistent styling
- Missing validation
- Poor mobile experience
- Unclear purchase limitations

### State Management
- Track cart state
- Handle purchase validation
- Manage loading states
- Track error states
- Handle success feedback
- Maintain bundle status
- Update UI consistently

## Component Registration
The component is registered as `codex-bundle-card` in the application.

## Implementation Details

### Currency Formatting
The component uses the common `formatCurrency` utility for consistent price display:
```javascript
const { formatCurrency } = useCommon(props);

// Usage in template
<div class="_c-focal-text _c-product-price">
  {{ formatCurrency(bundle.price) }}
</div>

<div class="_c-price-per-credit">
  {{ formatCurrency(bundle.price / bundle.total_credits) }} {{ $t("bundles.per_separator") }}
</div>
```

### Border Handling
The component respects the common `enableBorder` prop:
```vue
<div class="_c-card _c-bundle-card" 
  :class="{'_c-border': enableBorder, '_c-in-cart': isInCart}">
```

### Loading State
The component uses the common loading state management:
```vue
<codex-skeleton-card v-if="loading" :layout="{
  container: [
    { row: [{ size: 'lg', width: '100%' }] },
    // ... additional rows
  ]
}" />
``` 