# SubscriptionsOverview Component

## Overview
The SubscriptionsOverview component provides a dashboard interface for displaying active customer subscriptions with visual progress indicators, pricing information, and navigation to detailed subscription management. It features subscription filtering, pie chart progress visualization, credit tracking, renewal information, and loading states for effective subscription overview display.

## Basic Usage
```vue
<codex-subscriptions-overview
  :type="'active'"
  :default-target-tab="'memberships'"
  :noun="false"
/>
```

## Key Features
- Active subscription display with filtering
- Subscription progress visualization with pie charts
- Credit tracking and remaining bookings display
- Subscription pricing and billing information
- Renewal date tracking and display
- Loading states with skeleton placeholders
- Error handling and display
- No results state management
- Customer authentication integration
- Navigation to detailed subscription management

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| type | String | No | 'active' | Type of subscriptions to display |
| noun | String\|Boolean | No | false | Custom noun for subscription display |
| defaultTargetTab | String | No | 'memberships' | Target tab when navigating to detailed view |

### Common Props
All common props from `@/config/common` are supported.

### Common Functions
The component uses utilities from `useCommon`:
| Function | Usage |
|----------|-------|
| handleClick | Handles navigation clicks with proper event handling |
| formatCurrency | Formats subscription prices with currency symbols |

## Events
Currently, the SubscriptionsOverview component does not emit custom events. It serves as a dashboard widget with internal navigation.

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content -->
</template>
```

### Error Messages Slot
```vue
<template #error-messages="{ genericErrors }">
  <!-- Custom error display -->
</template>
```

### No Results Slot
```vue
<template #no-results="{ noun }">
  <!-- Custom no results message -->
</template>
```

## Subscription Management Integration
The component integrates with the subscription management system:
- **Subscription Loading**: Uses `useSubscriptions` composable for data management
- **Real-time Updates**: Automatically updates when subscription data changes
- **Authentication**: Requires authenticated customer for display
- **Status Filtering**: Filters subscriptions by status (active, cancelling, trial, paused)

## Subscription Status Filtering
The component displays subscriptions with specific statuses:
- **Active**: Currently active subscriptions
- **Cancelling**: Subscriptions in cancellation process
- **Trial**: Trial period subscriptions
- **Paused**: Temporarily paused subscriptions

## Progress Visualization
The component includes comprehensive progress tracking:

### Pie Chart Display
- **Visual Indicator**: Circular progress chart showing booking usage
- **Percentage Calculation**: Shows remaining vs used bookings
- **Color Coding**: Dynamic colors based on remaining booking percentage
- **Unlimited Handling**: Special display for unlimited subscriptions

### Color System
- **Green**: High remaining bookings (>40%)
- **Yellow**: Medium remaining bookings (10-40%)
- **Red**: Low remaining bookings (<10%)

## Subscription Information Display
Each subscription shows:
- **Subscription Name**: Plan name and title
- **Description**: Plan description information
- **Progress Chart**: Visual booking usage indicator
- **Remaining Bookings**: Current available bookings vs total
- **Price Display**: Formatted subscription price
- **Billing Information**: Credits per billing period
- **Renewal Date**: Next billing/renewal date

## Loading States
The component supports skeleton loading with:
- **Structured Layout**: Organized placeholder elements
- **Multi-row Display**: Comprehensive skeleton structure
- **Smooth Transitions**: Seamless transition to loaded state

## Internationalization

The `SubscriptionsOverview` component uses translation keys for subscription management interface:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `subscription.your` | Prefix for subscription section title | "Your subscriptions" |
| `subscription.subscription_noun` | Default noun for subscription type | Fallback subscription terminology |
| `subscription.bookings_remaining` | Remaining bookings label with billing interval parameter | Credits available this billing period |
| `subscription.unlimited` | Unlimited bookings indicator | When subscription has no booking limits |
| `subscription.per_separator` | Separator between credits and billing interval | "X credits per month" |
| `subscription.booking_renews` | Renewal date label | When booking allowance resets |
| `subscription.view_all` | Button to view all subscriptions | Navigation to detailed subscription management |
| `account.you_have_no` | No results prefix | When customer has no subscriptions |

### Billing Interval Translation

The component includes dynamic billing interval translation:

```javascript
const translatedBillingInterval = (billingInterval) => {
    return t('subscription.' + billingInterval);
};
```

This requires translation keys for each billing interval:
- `subscription.week`
- `subscription.month` 
- `subscription.year`
- etc.

### Implementation Examples

```vue
<!-- Section header -->
<div class="_c-subtitle">
    {{ $t('subscription.your') }} {{ noun || $t('subscription.subscription_noun') }}s
</div>

<!-- Remaining bookings display -->
<template v-if="subscription.max_bookings && subscription.max_bookings - subscription.bookings_made >= 0">
    {{ subscription.max_bookings - subscription.bookings_made }}/{{ subscription.max_bookings }} 
    {{ $t('subscription.bookings_remaining', { value: translatedBillingInterval(subscription.billing_interval) }) }}
</template>

<!-- Unlimited bookings -->
<template v-else>
    {{ $t('subscription.unlimited') }}
</template>

<!-- Credits per billing period -->
<div class="_c-desc _c-product-credits">
    {{ subscription.max_bookings }} {{ $t("subscription.per_separator") }} {{ translatedBillingInterval(subscription.billing_interval) }}
</div>

<!-- Renewal information -->
<div class="_c-desc _c-product-credits">
    {{ $t("subscription.booking_renews") }} {{ $filters.dateFormat(subscription.period_end, 'DD/MM/YYYY') }}
</div>

<!-- No results state -->
<div class="_c-noresults">
    {{ $t('account.you_have_no') }} {{ noun || $t('subscription.subscription_noun') }}s.
</div>

<!-- View all button -->
<codex-button 
    :defaultText="$t('subscription.view_all')"
    @click="handleClick"
/>
```

### Notes
- Dynamic billing interval translation for flexible subscription periods
- Parameterized translations for remaining bookings with billing context
- Customizable subscription noun through props
- Integration with date formatting for renewal dates
- Progress visualization with color-coded pie charts based on usage

## Examples

### Basic Implementation
```vue
<codex-subscriptions-overview />
```

### Custom Subscription Type
```vue
<codex-subscriptions-overview
  :type="'trial'"
  :noun="'membership'"
  :default-target-tab="'trial-memberships'"
/>
```

### Custom Header Content
```vue
<codex-subscriptions-overview>
  <template #header>
    <div class="subscriptions-header">
      <h3>Your Active Memberships</h3>
      <p>Manage your subscription benefits and track your usage</p>
      <div class="subscription-summary">
        <span>Active Plans: {{ activeSubscriptionCount }}</span>
        <span>Total Value: {{ totalSubscriptionValue }}</span>
      </div>
    </div>
  </template>
</codex-subscriptions-overview>
```

### Custom No Results State
```vue
<codex-subscriptions-overview>
  <template #no-results="{ noun }">
    <div class="no-subscriptions">
      <h4>No Active {{ noun || 'Subscriptions' }}</h4>
      <p>You don't have any active subscriptions. Explore our membership options to get started.</p>
      <codex-button 
        :default-text="'Browse Memberships'"
        @click="navigateToPlans"
        variant="primary"
      />
    </div>
  </template>
</codex-subscriptions-overview>
```

### Custom Error Handling
```vue
<codex-subscriptions-overview>
  <template #error-messages="{ genericErrors }">
    <div class="subscription-errors">
      <i class="error-icon" />
      <div>
        <h4>Unable to Load Subscriptions</h4>
        <ul>
          <li v-for="error in genericErrors" :key="error">
            {{ error }}
          </li>
        </ul>
        <button @click="retryLoadSubscriptions">Retry</button>
      </div>
    </div>
  </template>
</codex-subscriptions-overview>
```

### Dashboard Integration
```vue
<div class="account-dashboard">
  <div class="subscription-widgets">
    <codex-subscriptions-overview 
      class="active-subscriptions"
      :type="'active'"
    />
    
    <codex-subscriptions-overview 
      class="trial-subscriptions" 
      :type="'trial'"
      :noun="'trial'"
    />
  </div>
</div>
```

### With Subscription Analytics
```vue
<codex-subscriptions-overview
  @mounted="trackSubscriptionView"
/>

<script setup>
const trackSubscriptionView = () => {
  analytics.track('subscriptions_overview_viewed', {
    subscription_count: subscriptions.value.length,
    customer_id: customer.value?.id
  })
}
</script>
```

### Custom Progress Display
```vue
<codex-subscriptions-overview>
  <template #default>
    <div class="enhanced-subscriptions">
      <div v-for="subscription in subscriptions" :key="subscription.id" class="subscription-card">
        <div class="subscription-header">
          <h4>{{ subscription.name }}</h4>
          <span class="status-badge" :class="subscription.status">
            {{ subscription.status.toUpperCase() }}
          </span>
        </div>
        
        <div class="progress-section">
          <div class="pie-chart" :style="getPieChartStyle(subscription)"></div>
          <div class="usage-stats">
            <span class="remaining">{{ subscription.max_bookings - subscription.bookings_made }}</span>
            <span class="separator">/</span>
            <span class="total">{{ subscription.max_bookings }}</span>
            <span class="label">bookings remaining</span>
          </div>
        </div>
        
        <div class="subscription-details">
          <div class="price">{{ formatCurrency(subscription.plan.price) }}</div>
          <div class="billing">per {{ subscription.billing_interval }}</div>
          <div class="renewal">Renews {{ formatDate(subscription.period_end) }}</div>
        </div>
      </div>
    </div>
  </template>
</codex-subscriptions-overview>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-subscriptions-overview-card`: Subscriptions overview specific styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-subtitle`: Subtitle styling
- `_c-subscriptions-list`: Subscriptions list container
- `_c-subscription`: Individual subscription card
- `_c-product-title`: Product title styling
- `_c-product-desc`: Product description styling
- `_c-pie-container`: Pie chart container
- `_c-pie`: Pie chart styling
- `_c-pie-sm`: Small pie chart variant
- `animate`: Animation class for charts
- `_c-pie-info`: Pie chart information
- `_c-text-uppercase`: Uppercase text utility
- `_c-text-bold`: Bold text utility
- `_c-focal-text`: Focal/prominent text styling
- `_c-product-price`: Product price styling
- `_c-per-billing`: Billing period text styling
- `_c-desc`: Description text styling
- `_c-product-credits`: Product credits information
- `_c-noresults`: No results state styling
- `_c-btn-container`: Button container styling
- `_c-btn`: Button styling

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Provide clear subscription status visualization
- Handle loading states during subscription fetch
- Use appropriate subscription type filtering
- Display subscription progress clearly
- Implement proper navigation to management
- Track subscription viewing analytics
- Handle subscription state changes appropriately

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing loading states during data fetch
- Forgetting to handle different subscription statuses
- Not providing clear progress visualization
- Missing responsive considerations
- Insufficient handling of edge cases (no subscriptions)
- Not optimizing pie chart calculations
- Missing accessibility considerations

### Accessibility Considerations
- Provide meaningful chart descriptions
- Use appropriate ARIA attributes for progress indicators
- Ensure keyboard navigation for interactive elements
- Provide screen reader friendly subscription information
- Include proper semantic HTML structure
- Ensure adequate color contrast for chart elements
- Handle focus management appropriately
- Use semantic markup for subscription data

### Error Handling
- Handle missing subscription data gracefully
- Provide fallbacks for malformed subscription objects
- Display meaningful error messages
- Clear error states when data updates
- Handle chart rendering errors
- Provide retry mechanisms for failed loads
- Handle edge cases in progress calculations

### State Management
- Track subscription data changes
- Handle authentication state changes
- Manage loading states consistently
- Update charts when data changes
- Handle subscription status transitions
- Coordinate with subscription management systems
- Manage component lifecycle properly

### Performance Considerations
- Optimize subscription data rendering
- Minimize re-calculations of progress
- Handle large subscription datasets efficiently
- Implement proper cleanup for chart animations
- Cache subscription calculations appropriately
- Optimize responsive layout transitions
- Handle frequent subscription updates efficiently

### Chart Optimization
- Use CSS transforms for smooth animations
- Optimize progress percentage calculations
- Handle chart color calculations efficiently
- Minimize chart re-renders
- Use efficient progress display logic
- Implement proper chart accessibility
- Handle edge cases in chart display

### Subscription Management Integration
- Coordinate with subscription loading systems
- Handle subscription status changes properly
- Track subscription viewing analytics
- Handle subscription cancellation states
- Provide clear navigation to management
- Coordinate with subscription modification systems
- Handle subscription pause states appropriately

### Progress Tracking
- Calculate booking progress accurately
- Handle unlimited subscription display
- Provide clear progress visualization
- Color code progress appropriately
- Handle zero remaining bookings
- Display progress information clearly
- Track progress viewing analytics

### Navigation Integration
- Provide clear navigation paths
- Handle navigation state properly
- Coordinate with routing systems
- Track navigation analytics
- Handle navigation errors gracefully
- Provide appropriate navigation feedback
- Ensure navigation accessibility

## Component Registration
The component is registered as `codex-subscriptions-overview` in the application. 