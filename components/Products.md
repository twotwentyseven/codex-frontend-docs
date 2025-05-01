# Products Component

## Overview
The Products component is a versatile container for displaying and filtering product listings. It provides a structured layout with header, content, and footer sections, supporting dynamic filtering, loading states, and customizable display options for different product types.

## Basic Usage
```vue
<codex-products
  :handle="'my-products'"
  :filters="['category', 'price']"
  :per-page="27"
>
  <!-- Optional custom header content -->
  <template #header>
    <h1>Our Products</h1>
  </template>
</codex-products>
```

## Key Features
- Flexible slot-based layout structure (header, content, footer)
- Built-in filtering system
- Loading state handling with skeleton loading
- Internationalization support
- Customizable grid layout
- No results handling
- Dynamic product type rendering
- Powered by attribution option

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| handle | String | No | - | Product collection handle identifier |
| hideIfNoResults | Boolean | No | false | Hides the entire component if no products are found |
| hideDisabledBundles | Boolean | No | false | Controls visibility of disabled bundle products |
| showPricesInline | Boolean | No | false | Determines if prices should be shown inline |
| perPage | Number | No | 27 | Number of products to display per page |

### Layout Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| alignment | String | No | 'center' | Grid alignment ('start', 'end', 'center') |
| titleTag | String | No | - | HTML tag for the title element |
| title | String | No | - | Custom title text (falls back to translation) |
| showPoweredBy | Boolean | No | - | Controls visibility of powered by attribution |

### Filter Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| filters | Array | No | [] | Array of filter configurations to apply |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| filter-change | FilterEvent | Emitted when filters are modified |
| products-loaded | ProductsArray | Emitted when products are successfully loaded |
| error | Error | Emitted when an error occurs during loading |

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ products }">
  <!-- Custom content layout -->
</template>
```

### Footer Slot
```vue
<template #footer>
  <!-- Custom footer content -->
</template>
```

### No Results Slot
```vue
<template #no-results>
  <!-- Custom no results message -->
</template>
```

## States
1. Initial Loading
2. Products Loaded
3. No Results
4. Error State
5. Filtered Results

## Internationalization
The component uses the following translation keys:
- `products.title`: Default title text
- `product.introduction`: Introduction text
- `product.no_products_available`: No results message

## Examples

### Basic Products Grid
```vue
<codex-products
  handle="featured-products"
  :per-page="12"
  alignment="center"
>
</codex-products>
```

### Products with Custom Filters
```vue
<codex-products
  handle="collection-products"
  :filters="[
    { type: 'category', label: 'Product Category' },
    { type: 'price', label: 'Price Range' }
  ]"
  :hideDisabledBundles="true"
>
</codex-products>
```

### Custom Layout with Slots
```vue
<codex-products handle="special-products">
  <template #header>
    <h1>Special Offers</h1>
    <p>Discover our exclusive deals</p>
  </template>
  
  <template #content="{ products }">
    <div class="custom-grid">
      <div v-for="product in products" :key="product.id">
        <codex-product-card :product="product" />
      </div>
    </div>
  </template>
  
  <template #no-results>
    <div class="custom-empty-state">
      <p>No products found in this collection</p>
    </div>
  </template>
</codex-products>
```

## CSS Classes
- `_c-products`: Main container
- `_c-header`: Header section container
- `_c-content`: Content section container
- `_c-footer`: Footer section container
- `_c-grid`: Products grid container
- `_c-no-results`: No results message container

## Best Practices

### Recommended Usage
- Initialize with appropriate filters based on your product catalog
- Use meaningful handles for product collections
- Implement error handling for failed product loads
- Consider pagination for large product sets
- Utilize slots for custom layouts when needed

### Accessibility Considerations
- Ensure proper heading hierarchy in custom headers
- Maintain keyboard navigation in product grids
- Provide clear loading and error states
- Use ARIA labels where appropriate

### Performance Considerations
- Optimize product images for grid display
- Use appropriate per-page limits
- Implement lazy loading for images
- Consider pagination or infinite scroll for large collections

### Common Pitfalls to Avoid
- Overloading filters which may impact performance
- Not handling loading states appropriately
- Forgetting to handle error states
- Inconsistent grid layouts across viewports

### State Management
- Handle filter state changes efficiently
- Maintain loading state during data fetches
- Properly reset state when handle changes
- Manage error states appropriately

## Component Registration
The component is registered as `codex-products` in the application. 