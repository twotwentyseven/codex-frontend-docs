# GiftCards Component

## Overview
The GiftCards component displays a collection of gift card products with filtering capabilities, loading states, and comprehensive cart integration. It provides a complete interface for browsing and purchasing gift cards with support for various denominations, custom layouts, and interactive features. The component integrates with filter contexts, handles empty states, and supports customizable templates for different business scenarios.

## Basic Usage
```vue
<template>
  <div class="gift-cards-section">
    <codex-gift-cards 
      :title="'Gift Cards'"
      :hide-if-no-results="false"
      :group="'gift-cards'"
      :per-page="3"
    />
  </div>
</template>

<script setup>
// Gift cards automatically load via composition API
</script>
```

## Key Features
- Gift card product display with card-based layout
- Filter integration with primary and secondary filters
- Loading states with skeleton placeholders
- Empty state handling with customizable messages
- Cart integration for gift card purchases
- Support for multiple gift card denominations
- Grid layout with configurable alignment
- Internationalization support
- Responsive design
- Slot-based customization for all sections

## Configuration Props

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `title` | `String` | `''` | Page title, falls back to translation |

### Display Control Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `hideIfNoResults` | `Boolean` | `false` | Hide component when no gift cards available |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `alignment` | `String` | `'center'` | Content alignment (start, end, center) |

### Filter Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `group` | `String` | `'default'` | Filter group identifier |
| `perPage` | `Number` | `3` | Number of items to display per page |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Enables border styling on gift card cards |
| `titleTag` | HTML tag for the title element |
| `showPoweredBy` | Controls "Powered By" footer display |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatCurrency` | Formats gift card prices for display |
| `truncateString` | Truncates long descriptions |
| `toggleOpen` | Handles expandable content sections |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| N/A | N/A | Component uses internal cart and filter systems |

## Slots

### Header Slot
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ giftCards }` | Custom header content |

### Filter Slots
| Slot | Props | Description |
|------|-------|-------------|
| `filters` | `{ group }` | Complete filter wrapper |
| `primary-filters` | `{ group }` | Primary filter content |
| `secondary-filters` | `{ group }` | Secondary filter content |

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `content` | `{ giftCards }` | Main content area with gift cards grid |
| `no-results` | N/A | Empty state message |

### Footer Slot
| Slot | Props | Description |
|------|-------|-------------|
| `footer` | `{ giftCards }` | Footer content |

## Loading States

The component provides loading states through:
- Skeleton cards during data fetching
- Loading indicators on gift card cards
- Conditional rendering based on `loading` state
- Integration with filter context ready state

## Filter Integration

The component integrates with the filter system through:
- Filter context detection and registration
- Automatic data loading when filters change
- Primary and secondary filter slot support
- Results count display in filter wrapper

## Internationalization

The component uses the following translation keys:

### Gift Card Interface Messages
| Key | Usage |
|-----|-------|
| `giftcards.title` | Default gift cards page title |
| `giftcards.introduction` | Gift cards page introduction text |
| `giftcard.no_gift_cards_available` | No gift cards available message |

### Translation Usage Examples
```vue
<!-- Gift cards header -->
<div class="_c-header">
  <codex-title :tag="titleTag" :content="title || $t('giftcards.title')" />
  <codex-paragraph tag="p" :content="$t('giftcards.introduction')" />
  <codex-error :error="genericErrors" />
</div>

<!-- No results state -->
<div v-if="!giftCards?.length && !hideIfNoResults && !loading" class="_c-no-results">
  <slot name="no-results">{{ $t("giftcard.no_gift_cards_available") }}</slot>
</div>
```

### Translation Notes

#### Collection-Level Translations
The GiftCards component handles collection-level messaging:
- Page title with fallback to translation when title prop is not provided
- Introduction text to describe gift card offerings
- No results messaging for empty collections

#### Child Component Integration
The component delegates detailed translations to child components:
- `codex-gift-card-card` components handle individual gift card translations
- Gift card cards include pricing, customization, and purchase action translations
- Filter components handle search and filtering translations

#### Filter System Integration
The component integrates with filter systems that have their own translations:
- Primary and secondary filter slots support
- Filter result count display
- Filter context integration for data loading

#### Customizable Content
The component supports content customization through slots:
- No results slot allows custom empty state messages
- Header and footer slots support custom content with translations
- Filter slots enable custom filter interfaces with appropriate translations

#### Gift Card Specific Features
The component supports gift card-specific functionality:
- Multiple denomination support through child components
- Custom messaging and personalization through gift card cards
- Delivery options and scheduling through individual card interfaces

## Usage Examples

### Basic Gift Cards Display
```vue
<codex-gift-cards>
  <template #header>
    <h2>{{ $t('gift_cards.title') }}</h2>
    <p>{{ $t('gift_cards.description') }}</p>
  </template>
</codex-gift-cards>
```

### With Custom Purchase Flow
```vue
<codex-gift-cards 
  @purchase="handlePurchase"
  @customize="handleCustomize"
>
  <template #purchase-form>
    <div class="custom-purchase-form">
      <h4>{{ $t('gift_cards.purchase.select_amount') }}</h4>
      <!-- Custom purchase form -->
    </div>
  </template>
</codex-gift-cards>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-giftcards` | Main container class |
| `_c-header` | Header section styling |
| `_c-content` | Content area styling |
| `_c-footer` | Footer section styling |
| `_c-grid` | Grid layout for gift card cards |
| `_c-no-results` | Empty state styling |

## Best Practices

### Gift Card Configuration
- Offer multiple denominations to suit different budgets
- Provide clear expiration information and policies
- Include attractive visual designs and themes
- Support both digital and physical delivery options
- Implement proper gift card validation and security

### User Experience
- Show loading states during data fetching
- Provide clear empty state messaging
- Enable easy denomination selection
- Support gift card preview functionality
- Implement clear purchasing flow

### Performance
- Implement lazy loading for gift card images
- Cache gift card data to reduce API calls
- Optimize image assets for fast loading
- Use skeleton loading for better perceived performance
- Consider CDN for gift card imagery

### Filtering
- Provide intuitive filter options (amount, theme, occasion)
- Show filter result counts
- Allow filter clearing and reset
- Group filters logically by purpose
- Support quick filters for popular selections

### Accessibility
- Ensure proper heading hierarchy
- Provide descriptive alt text for gift card images
- Support keyboard navigation
- Use appropriate ARIA labels for interactive elements
- Test with screen readers for gift card information

### Security
- Implement proper gift card code generation
- Use secure delivery methods for digital cards
- Validate gift card purchases thoroughly
- Monitor for fraudulent activity
- Implement proper redemption tracking

## Component Registration
```javascript
// Global registration
app.component('CodexGiftCards', GiftCards)

// Local registration  
import GiftCards from '@/components/products/GiftCards.vue'

export default {
  components: {
    CodexGiftCards: GiftCards
  }
}
``` 