# Plans Component

## Overview
The Plans component displays a collection of subscription plans with filtering capabilities, loading states, and comprehensive cart integration. It provides a complete interface for browsing and selecting subscription plans with support for various configuration options, custom layouts, and interactive features. The component integrates with filter contexts, handles empty states, and supports customizable templates for different business needs.

## Basic Usage
```vue
<template>
  <div class="subscription-plans">
    <codex-plans 
      :title="'Choose Your Plan'"
      :hide-if-no-results="false"
      :show-prices-inline="true"
      :group="'subscription-plans'"
    />
  </div>
</template>

<script setup>
// Plans automatically load via composition API
</script>
```

## Key Features
- Subscription plan display with card-based layout
- Filter integration with primary and secondary filters
- Loading states with skeleton placeholders
- Empty state handling with customizable messages
- Cart integration for plan purchases
- Support for variable start dates and scheduling
- Plan availability and status management
- Internationalization support
- Responsive grid layout
- Slot-based customization for all sections

## Configuration Props

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `title` | `String` | `''` | Page title, falls back to translation |

### Display Control Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `hideIfNoResults` | `Boolean` | `false` | Hide component when no plans available |
| `hideDisabledPlans` | `Boolean` | `false` | Hide plans that cannot be purchased |
| `showPricesInline` | `Boolean` | `false` | Display prices inline with plan details |

### Plan Configuration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `variableStartDate` | `Boolean` | `false` | Allow users to select plan start dates |
| `disableCurrentlyPurchasedPlans` | `Boolean` | `false` | Disable plans already purchased by customer |

### Schedule Configuration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `daysOfWeekToShow` | `Array` | `[0,1,2,3,4,5,6]` | Days of week to display (0=Sunday, 6=Saturday) |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `alignment` | `String` | `'center'` | Content alignment (start, end, center) |

### Filter Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `group` | `String` | `'default'` | Filter group identifier |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |
| `...filters.props` | `Various` | Inherits filter-related props |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Enables border styling on plan cards |
| `titleTag` | HTML tag for the title element |
| `showPoweredBy` | Controls "Powered By" footer display |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatCurrency` | Formats plan prices for display |
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
| `header` | `{ plans }` | Custom header content |

### Filter Slots
| Slot | Props | Description |
|------|-------|-------------|
| `filters` | `{ group }` | Complete filter wrapper |
| `primary-filters` | `{ group }` | Primary filter content |
| `secondary-filters` | `{ group }` | Secondary filter content |

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `content` | `{ plans }` | Main content area with plans grid |
| `no-results` | N/A | Empty state message |

### Footer Slot
| Slot | Props | Description |
|------|-------|-------------|
| `footer` | `{ plans }` | Footer content |

## Loading States

The component provides loading states through:
- Skeleton cards during data fetching
- Loading indicators on plan cards
- Conditional rendering based on `loading` state
- Integration with filter context ready state

## Filter Integration

The component integrates with the filter system through:
- Filter context detection and registration
- Automatic data loading when filters change
- Primary and secondary filter slot support
- Results count display in filter wrapper

## Examples

### Basic Subscription Plans
```vue
<template>
  <div class="subscription-page">
    <codex-plans 
      :title="'Choose Your Membership'"
      :group="'membership-plans'"
      :show-prices-inline="true"
      :variable-start-date="true"
    />
  </div>
</template>
```

### Plans with Custom Filtering
```vue
<template>
  <div class="filtered-plans">
    <codex-filter-context :group="'fitness-plans'" :default-values="defaultFilters">
      <template #default="{ filters }">
        <codex-plans 
          :group="'fitness-plans'"
          :title="'Fitness Plans'"
          :hide-disabled-plans="true"
        >
          <template #primary-filters="{ group }">
            <codex-contextual-filter
              :group="group"
              :definition="'plan_types'"
              :filter-key="'plan_type'"
              :label="'Plan Type'"
              :filter-type="'radio'"
              :primary-filter="true"
            />
            
            <codex-contextual-filter
              :group="group"
              :definition="'price_ranges'"
              :filter-key="'price_range'"
              :label="'Price Range'"
              :filter-type="'select'"
              :primary-filter="true"
            />
          </template>
          
          <template #secondary-filters="{ group }">
            <codex-contextual-filter
              :group="group"
              :definition="'billing_intervals'"
              :filter-key="'billing_interval'"
              :label="'Billing Frequency'"
              :filter-type="'checkbox-group'"
            />
            
            <codex-contextual-filter
              :group="group"
              :definition="'features'"
              :filter-key="'feature_ids'"
              :label="'Features'"
              :filter-type="'multi-select'"
            />
          </template>
        </codex-plans>
      </template>
    </codex-filter-context>
  </div>
</template>

<script setup>
const defaultFilters = {
  plan_type: '',
  price_range: '',
  billing_interval: [],
  feature_ids: []
}
</script>
```

### Custom Plans with Business Logic
```vue
<template>
  <div class="business-plans">
    <codex-plans 
      :group="'business-plans'"
      :disable-currently-purchased-plans="true"
      :hide-if-no-results="true"
      :days-of-week-to-show="weekdays"
    >
      <template #header="{ plans }">
        <div class="business-header">
          <h2>Business Subscription Plans</h2>
          <p>Choose the perfect plan for your growing business</p>
          
          <div class="plan-statistics">
            <div class="stat-item">
              <span class="stat-value">{{ plans?.length || 0 }}</span>
              <span class="stat-label">Available Plans</span>
            </div>
            <div class="stat-item">
              <span class="stat-value">{{ getActiveCustomerCount() }}</span>
              <span class="stat-label">Active Customers</span>
            </div>
            <div class="stat-item">
              <span class="stat-value">{{ formatCurrency(getAveragePrice(plans)) }}</span>
              <span class="stat-label">Average Price</span>
            </div>
          </div>
        </div>
      </template>
      
      <template #content="{ plans }">
        <div class="plans-showcase">
          <!-- Featured Plans Section -->
          <div v-if="featuredPlans.length" class="featured-plans">
            <h3>Featured Plans</h3>
            <div class="featured-grid">
              <codex-plan-card
                v-for="plan in featuredPlans"
                :key="plan.id"
                :product="plan"
                :enable-border="true"
                :show-price="true"
                :variable-start-date="true"
              />
            </div>
          </div>
          
          <!-- Regular Plans Section -->
          <div v-if="regularPlans.length" class="regular-plans">
            <h3>All Plans</h3>
            <div class="plans-grid">
              <codex-plan-card
                v-for="plan in regularPlans"
                :key="plan.id"
                :product="plan"
                :enable-border="false"
                :show-price="true"
              />
            </div>
          </div>
          
          <!-- Plan Comparison Table -->
          <div class="plan-comparison">
            <h3>Plan Comparison</h3>
            <plan-comparison-table :plans="plans" />
          </div>
        </div>
      </template>
      
      <template #footer="{ plans }">
        <div class="plans-footer">
          <div class="help-section">
            <h4>Need Help Choosing?</h4>
            <p>Our team can help you select the perfect plan for your needs.</p>
            <button @click="openPlanConsultation" class="consultation-btn">
              Get Free Consultation
            </button>
          </div>
          
          <div class="guarantee-section">
            <h4>30-Day Money Back Guarantee</h4>
            <p>Not satisfied? Get a full refund within 30 days.</p>
          </div>
        </div>
      </template>
      
      <template #no-results>
        <div class="no-plans-available">
          <h3>No Plans Available</h3>
          <p>We're currently updating our subscription plans. Please check back soon.</p>
          <button @click="notifyWhenAvailable" class="notify-btn">
            Notify Me When Available
          </button>
        </div>
      </template>
    </codex-plans>
  </div>
</template>

<script setup>
import { computed } from 'vue'

// Only show weekdays for business plans
const weekdays = [1, 2, 3, 4, 5] // Monday to Friday

const featuredPlans = computed(() => {
  return plans.value?.filter(plan => plan.is_featured) || []
})

const regularPlans = computed(() => {
  return plans.value?.filter(plan => !plan.is_featured) || []
})

const getActiveCustomerCount = () => {
  // Implementation would fetch real data
  return 2547
}

const getAveragePrice = (plans) => {
  if (!plans?.length) return 0
  const total = plans.reduce((sum, plan) => sum + plan.price, 0)
  return total / plans.length
}

const openPlanConsultation = () => {
  // Implementation would open consultation modal or redirect
  console.log('Opening plan consultation')
}

const notifyWhenAvailable = () => {
  // Implementation would collect email for notifications
  console.log('Setting up availability notification')
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-plans` | Main container class |
| `_c-header` | Header section styling |
| `_c-content` | Content area styling |
| `_c-footer` | Footer section styling |
| `_c-grid` | Grid layout for plan cards |
| `_c-no-results` | Empty state styling |

## Internationalization

The component uses the following translation keys:

### Core Component Keys
| Key | Usage |
|-----|-------|
| `plans.title` | Default page title when title prop is not provided |
| `plans.introduction` | Header description text |
| `plans.no_plans_available` | Empty state message when no plans are found |

### Related Component Translation Keys
The Plans component integrates with PlanCard components that use additional translation keys:

| Key | Usage |
|-----|-------|
| `plans.already_active` | Message for currently active plans |
| `product.whats_included` | Section title for plan features |
| `plans.joining_fee` | Joining fee label |
| `plans.per_separator` | Separator text between price and billing interval |
| `plans.credit` | Credit unit label |
| `plans.terms_button` | Terms and conditions button text |
| `plans.terms_and_conditions_title` | Terms modal title |
| `plans.close_terms` | Close terms modal button |
| `plans.choose_start_date` | Start date selection label |
| `product.adding` | Add to cart processing text |
| `product.add_to_cart` | Add to cart button text |
| `product.unavailable` | Unavailable plan button text |
| `product.error` | Error state button text |
| `plans.starting_date` | Start date prefix text |
| `plans.unavailable` | Plan unavailable status |

### Translation Usage Examples
```vue
<!-- Using title prop with fallback to translation -->
<codex-plans :title="false" />  <!-- Uses $t('plans.title') -->
<codex-plans title="Custom Plans Title" />  <!-- Uses custom title -->

<!-- Default introduction uses translation -->
<codex-plans />  <!-- Header shows $t('plans.introduction') -->

<!-- Custom content with translations -->
<template #header>
  <h2>{{ $t('plans.custom_header_title') }}</h2>
  <p>{{ $t('plans.custom_description') }}</p>
</template>

<!-- No results slot uses translation -->
<template #no-results>
  <div class="custom-empty-state">
    {{ $t('plan.no_plans_available') }}
    <button>{{ $t('plans.get_notified') }}</button>
  </div>
</template>
```

## Best Practices

### Plan Configuration
- Use descriptive plan names and clear descriptions
- Implement proper plan availability logic
- Consider user's current subscription status
- Provide clear pricing information
- Include comprehensive plan features

### User Experience
- Show loading states during data fetching
- Provide clear empty state messaging
- Enable plan comparison functionality
- Support plan preview and detailed views
- Implement clear call-to-action buttons

### Performance
- Implement lazy loading for large plan collections
- Cache plan data to reduce API calls
- Optimize images and assets used in plan cards
- Use skeleton loading for better perceived performance
- Consider pagination for extensive plan catalogs

### Filtering
- Provide intuitive filter options relevant to plans
- Show filter result counts
- Allow filter clearing and reset
- Persist filter state in URL when appropriate
- Group filters logically (price, features, billing)

### Accessibility
- Ensure proper heading hierarchy
- Provide descriptive alt text for plan images
- Support keyboard navigation through plans
- Use appropriate ARIA labels for interactive elements
- Test with screen readers

### Business Logic
- Handle subscription conflicts appropriately
- Implement proper plan eligibility rules
- Support plan upgrades and downgrades
- Manage billing cycle transitions
- Provide clear terms and conditions

## Component Registration
```javascript
// Global registration
app.component('CodexPlans', Plans)

// Local registration  
import Plans from '@/components/products/Plans.vue'

export default {
  components: {
    CodexPlans: Plans
  }
}
``` 