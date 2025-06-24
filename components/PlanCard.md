# Plan Card Component

## Overview
The Plan Card component displays subscription plan information in a card format, supporting features like variable start dates, recurring billing, booking limits, and subscription management. It handles both new subscriptions and existing customer states, with support for scheduled subscriptions and plan transitions.

## Basic Usage
```vue
<codex-plan-card
  :product="planData"
  :show-price="true"
/>
```

## Key Features
- Skeleton loading state
- Variable start date selection
- Subscription validation
- Cart integration
- Price formatting
- Booking limits display
- Badge support
- Featured plan styling
- Joining fee handling
- Conditional content
- Subscription state awareness
- Internationalization support
- Error handling
- Toast notifications

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| product | Object | Yes | - | Plan product data |
| loading | Boolean | No | false | Show skeleton loading state |
| showPrice | Boolean | No | false | Show price in add to cart button |
| variableStartDate | Boolean | No | true | Enable start date selection |
| hideIfDisabled | Boolean | No | false | Hide card if plan cannot be purchased |
| disableCurrentlyPurchasedPlans | Boolean | No | false | Disable plans customer already has |
| daysOfWeekToShow | Array | No | [0,1,2,3,4,5,6] | Days of week to show in start date selector |
| showPricePerCredit | Boolean | No | true | Show price per booking calculation |

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
| formatCurrency | Formats plan prices and joining fees |
| isMobile | Handles responsive layout adjustments |
| loadingItems | Manages skeleton loading state |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| add | - | Emitted when plan is added to cart |

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
| plan | Object | Plan data object |
| add | Function | Add to cart function |
| adding | Boolean | Loading state for add action |
| added | Boolean | Success state for add action |
| error | Object | Error state object |
| fieldErrors | Object | Field-specific errors |
| genericErrors | Array | Generic error messages |
| formatCurrency | Function | Currency formatting function |
| canPurchase | Boolean | Whether plan can be purchased |
| isInCart | Boolean | Whether plan is in cart |
| start | String | Selected start date |
| updateStart | Function | Update start date function |
| startDates | Array | Available start dates |
| startDateRequired | Boolean | Whether start date selection is required |

## States
1. Loading (skeleton)
2. Default
3. Featured
4. Disabled
5. In Cart
6. Already Active
7. Scheduled to Start
8. Error
9. Adding to Cart
10. Added Success
11. Start Date Required

## Internationalization

The component uses the following translation keys:

### Core Plan Information
| Key | Usage |
|-----|-------|
| `plan.title` | Default plan title when title prop is not provided |
| `product.whats_included` | What's included section title |
| `plan.description` | Plan description fallback |

### Plan Status and Availability
| Key | Usage |
|-----|-------|
| `plan.already_active` | Already subscribed message |
| `plan.unavailable` | Plan unavailable text |
| `plan.coming_soon` | Coming soon status |
| `plan.suspended` | Suspended plan status |
| `plan.archived` | Archived plan status |

### Start Date and Scheduling
| Key | Usage |
|-----|-------|
| `plan.starting_date` | Scheduled start date message |
| `plan.choose_start_date` | Start date selector label |
| `plan.start_immediately` | Immediate start text |
| `plan.starts_on` | Starts on date prefix |
| `plan.next_available_start` | Next available start date |
| `plan.custom_start_date` | Custom start date option |

### Pricing and Billing
| Key | Usage |
|-----|-------|
| `plan.joining_fee` | Joining fee label |
| `plan.per_separator` | Price per booking separator ("per") |
| `plan.credit` | Single booking/credit text |
| `plan.credits` | Multiple bookings/credits text |
| `plan.unlimited` | Unlimited access text |
| `plan.price_per_credit` | Price per credit calculation |
| `plan.billing_interval` | Billing interval display |

### Billing Intervals
| Key | Usage |
|-----|-------|
| `plan.monthly` | Monthly billing interval |
| `plan.weekly` | Weekly billing interval |
| `plan.yearly` | Yearly billing interval |
| `plan.daily` | Daily billing interval |
| `plan.every_3_months` | Quarterly billing interval |
| `plan.every_6_months` | Semi-annual billing interval |

### Plan Features and Limits
| Key | Usage |
|-----|-------|
| `plan.max_bookings_per_period` | Booking limit per billing period |
| `plan.rollover_credits` | Rollover credits feature |
| `plan.pause_subscription` | Pause subscription feature |
| `plan.cancel_anytime` | Cancel anytime feature |
| `plan.no_commitment` | No commitment feature |

### Cart and Purchase Actions
| Key | Usage |
|-----|-------|
| `product.add_to_cart` | Add to cart button text |
| `product.adding` | Adding to cart loading state |
| `product.starting` | Starting subscription text |
| `cart.added_to_cart` | Success toast message |
| `cart.in_cart` | Already in cart indicator |
| `plan.subscribe_now` | Subscribe now button |

### Plan States and Messages
| Key | Usage |
|-----|-------|
| `product.unavailable` | Unavailable state text |
| `product.error` | Generic error state text |
| `plan.enrollment_closed` | Enrollment closed message |
| `plan.waitlist_available` | Waitlist available message |
| `plan.limited_spots` | Limited spots available |

### Trial and Promotional
| Key | Usage |
|-----|-------|
| `plan.free_trial` | Free trial indicator |
| `plan.trial_period` | Trial period length |
| `plan.promotional_price` | Promotional pricing indicator |
| `plan.limited_time_offer` | Limited time offer notice |
| `plan.discount_applied` | Discount applied message |

### Translation Usage Examples
```vue
<!-- Plan credits display -->
<div class="plan-credits">
  {{ plan.max_bookings_per_period }} 
  {{ $t("plans.per_separator") }} 
  {{ translatedBillingInterval(plan.billing_interval) }}
</div>

<!-- Price per credit calculation -->
<div v-if="showPricePerCredit" class="price-per-credit">
  {{ formatCurrency(plan.price / plan.max_bookings_per_period) }} 
  {{ $t("plans.per_separator") }} 
  {{ $t("plans.credit") }}
</div>

<!-- Plan status badges -->
<div class="plan-badge">
  <template v-if="scheduledSubscriptionStartAt">
    {{ scheduledSubscriptionStartAt }}
  </template>
  <template v-else-if="customerHasSubscription">
    {{ $t('plans.already_active') }}
  </template>
</div>

<!-- Start date selection -->
<codex-select-field 
  v-if="variableStartDate"
  :name="'plan-start-date-'+plan.id"
  :label="$t('plans.choose_start_date')"
  :options="startDates"
  v-model="start"
  :placeholder="$t('plans.choose_start_date')"
/>

<!-- Add to cart button with dynamic text -->
<codex-button
  :processingText="$t('product.adding')"
  :defaultText="defaultText"
  :disabledText="$t('product.unavailable')"
  :errorText="$t('product.error')"
  @click="add"
/>

<!-- Joining fee display -->
<div v-if="plan.joining_fee > 0" class="joining-fee">
  ({{ $t('plans.joining_fee') }} {{ formatCurrency(plan.joining_fee) }})
</div>
```

## Examples

### Basic Plan Card
```vue
<codex-plan-card
  :product="plan"
  :show-price="true"
  :variable-start-date="true"
/>
```

### Featured Plan with Custom Content
```vue
<codex-plan-card
  :product="featuredPlan"
  class="featured-plan"
>
  <template #content="{ plan, formatCurrency }">
    <div class="custom-content">
      <h2>{{ plan.name }}</h2>
      <div class="price">{{ formatCurrency(plan.price) }}</div>
      <div class="bookings">{{ plan.max_bookings_per_period }} bookings per {{ plan.billing_interval }}</div>
    </div>
  </template>
</codex-plan-card>
```

### Plan with Custom Start Date Selection
```vue
<codex-plan-card
  :product="plan"
  :days-of-week-to-show="[1,2,3,4,5]"
>
  <template #footer="{ startDates, updateStart, start }">
    <div class="date-selector">
      <codex-select-field
        :options="startDates"
        v-model="start"
        @update:model-value="updateStart"
      />
    </div>
  </template>
</codex-plan-card>
```

### Plan with Custom Error Handling
```vue
<codex-plan-card
  :product="plan"
>
  <template #footer="{ genericErrors }">
    <div class="error-container">
      <codex-error :error="genericErrors" />
    </div>
  </template>
</codex-plan-card>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-plan-card`: Plan specific class
- `_c-border`: Border styling
- `_c-in-cart`: In cart state
- `_c-disabled`: Disabled state
- `_c-featured`: Featured plan styling
- `_c-content-featured`: Featured content styling
- `_c-badge`: Badge container
- `_c-product-badge`: Product badge styling
- `_c-content-column`: Content column layout
- `_c-product-title`: Product title styling
- `_c-product-desc`: Product description styling
- `_c-additional-content`: Additional content section
- `_c-subtitle`: Subtitle styling
- `_c-custom-list`: Custom list styling
- `_c-row`: Row layout
- `_c-column`: Column layout
- `_c-focal-text`: Emphasized text styling
- `_c-product-price`: Price styling
- `_c-per-billing`: Billing interval styling
- `_c-product-credits`: Credits display styling
- `_c-price-per-credit`: Price per credit styling
- `_c-btn-container`: Button container
- `_c-product-btn`: Product button styling

## Best Practices

### Recommended Usage
- Use skeleton loading for data fetching
- Implement clear pricing display
- Show appropriate subscription states
- Handle all subscription scenarios
- Provide clear start date selection
- Use consistent styling
- Consider mobile layouts
- Handle joining fees appropriately

### Accessibility Considerations
- Use proper heading hierarchy
- Ensure button accessibility
- Provide clear error messages
- Maintain keyboard navigation
- Use appropriate ARIA labels
- Consider color contrast
- Handle focus states
- Make date selection accessible

### Performance Considerations
- Optimize date calculations
- Handle subscription checks efficiently
- Manage state transitions
- Implement proper caching
- Optimize price calculations
- Handle loading states
- Consider lazy loading
- Cache start date options

### Common Pitfalls to Avoid
- Missing loading states
- Unclear pricing information
- Poor error handling
- Inconsistent styling
- Missing validation
- Poor mobile experience
- Unclear subscription states
- Confusing start date selection
- Missing joining fee information

### State Management
- Track subscription state
- Handle start date selection
- Manage loading states
- Track error states
- Handle success feedback
- Maintain plan status
- Update UI consistently
- Monitor cart state
- Track scheduled subscriptions

## Component Registration
The component is registered as `codex-plan-card` in the application. 

## Implementation Details

### Currency Formatting
The component uses the common `formatCurrency` utility for consistent price display:
```javascript
const { formatCurrency } = useCommon(props);

// Usage in template
<div class="_c-focal-text _c-product-price">
  {{ formatCurrency(plan.price) }}
</div>

<div class="cdx_joining-fee">
  {{ $t('plan.joining_fee')}} {{ formatCurrency(plan.joining_fee) }}
</div>
```

### Border Handling
The component respects the common `enableBorder` prop:
```vue
<div class="_c-card _c-plan-card" 
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