# SubscriptionCard Component

## Overview
The SubscriptionCard component provides a detailed card interface for displaying individual subscription entries with comprehensive plan information, billing details, and management actions. It features subscription pricing, credit tracking, billing cycle information, pause/cancel functionality, and status management for effective subscription display and management.

## Basic Usage
```vue
<codex-subscription-card
  :subscription="subscriptionData"
  :loading="false"
  @cancel="handleCancelSubscription"
  @pause="handlePauseSubscription"
/>
```

## Key Features
- Comprehensive subscription plan information
- Pricing display with billing intervals
- Credit tracking and remaining balance
- Start and end date management
- Trial period tracking
- Subscription status indicators
- Pause and cancel functionality
- Billing cycle information
- Contract end date tracking
- Loading skeleton support
- Responsive layout design

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| subscription | Object\|Number | Yes | - | Subscription data object or ID |
| loading | Boolean | No | false | Whether the card is in loading state |
| isProcessing | Boolean | No | false | Whether subscription actions are processing |
| isRecentlyCancelled | Boolean | Yes | - | Whether subscription was recently cancelled |
| enableBorder | Boolean | No | false | Whether to enable card border styling |

### Common Props
All common props from `@/config/common` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| cancel | `subscription: Object` | Emitted when cancel subscription button is clicked |
| pause | `subscription: Object` | Emitted when pause subscription button is clicked |

## Slots

### Header Slot
```vue
<template #header="{ subscription }">
  <!-- Custom header content -->
</template>
```

### Footer Slot
```vue
<template #footer="{ subscription }">
  <!-- Custom footer content -->
</template>
```

## Subscription Object Structure
The subscription prop expects an object with the following structure:
```javascript
{
  id: String|Number,          // Unique subscription identifier
  name: String,               // Subscription name
  status: String,             // Subscription status ('active', 'trialing', 'cancelling', etc.)
  status_detail: String,      // Detailed status description
  can_cancel: Boolean,        // Whether subscription can be cancelled
  created_at: Date,           // Subscription start date
  ends_at: Date,              // Subscription end date (if applicable)
  contract_end_date: Date,    // Contract end date (if applicable)
  period_start: Date,         // Current billing period start
  period_end: Date,           // Current billing period end / renewal date
  trial_ends_at: Date,        // Trial end date (if trialing)
  billing_interval: String,   // Billing frequency ('monthly', 'weekly', etc.)
  booking_interval: String,   // Booking period interval
  max_bookings: Number|null,  // Maximum bookings per period (null for unlimited)
  bookings_made: Number,      // Bookings made in current period
  plan: {                     // Associated plan information
    id: String|Number,        // Plan ID
    name: String,             // Plan name
    description: String,      // Plan description
    price: Number,            // Plan price
    billing_interval: String, // Plan billing interval
    max_bookings_per_period: Number  // Credits per billing period
  }
}
```

## Subscription Status Handling
The component handles different subscription statuses:

### Active Subscriptions
- Shows renewal date
- Displays cancel and pause options
- Shows current credit usage

### Trialing Subscriptions
- Displays trial end date
- Shows when trial converts to paid
- Indicates trial status

### Cancelling Subscriptions
- Hides renewal date
- Shows end date instead
- Indicates cancellation status

## Credit Tracking
The component displays comprehensive credit information:
- **Remaining Credits**: Current available credits
- **Total Credits**: Maximum credits per period
- **Usage Display**: Credits used vs available
- **Unlimited Handling**: Special display for unlimited subscriptions

## Billing Information
Displays detailed billing cycle information:
- **Price Display**: Formatted subscription price
- **Billing Interval**: Frequency of billing
- **Per Credit Cost**: Calculated cost per credit
- **Period Dates**: Current billing period boundaries

## Date Management
Handles various subscription dates:
- **Start Date**: When subscription began
- **End Date**: When subscription ends (if applicable)
- **Contract End**: Contract termination date
- **Renewal Date**: Next billing date
- **Trial End**: When trial period expires

## Loading State
The component supports skeleton loading with:
- **Structured Layout**: Organized placeholder elements
- **Complete Structure**: Mimics full subscription card layout
- **Responsive Design**: Adapts to different screen sizes

## Internationalization

The component uses the following translation keys:

### Core Subscription Information
| Key | Usage |
|-----|-------|
| `subscription.title` | Default subscription title when title prop is not provided |
| `subscription.subscription_details` | Subscription details section header |
| `subscription.active` | Active status display |
| `subscription.inactive` | Inactive status display |
| `subscription.cancelled` | Cancelled status display |
| `subscription.paused` | Paused status display |
| `subscription.trialing` | Trial status display |

### Credit and Usage Information
| Key | Usage |
|-----|-------|
| `subscription.credits_remaining` | Credits remaining label |
| `subscription.unlimited_access` | Unlimited subscription display |
| `subscription.credit` | Single credit label |
| `subscription.credits` | Multiple credits label (with count parameter) |
| `subscription.used` | Credits used indicator |
| `subscription.of` | "of" separator for X of Y display |
| `subscription.remaining_credits` | Alternative credits remaining label |

### Date and Billing Information
| Key | Usage |
|-----|-------|
| `subscription.start_date` | Start date label |
| `subscription.end_date` | End date label |
| `subscription.ends_at` | Alternative end date label |
| `subscription.contract_ends_on` | Contract end date label |
| `subscription.renews_on` | Renewal date label |
| `subscription.trial_ends` | Trial end date label |
| `subscription.period_end` | Period end date label |
| `subscription.next_billing_date` | Next billing date label |

### Pricing and Billing Intervals
| Key | Usage |
|-----|-------|
| `subscription.per_separator` | "per" separator text for pricing |
| `subscription.price_per_credit` | Price per credit display |
| `plan.monthly` | Monthly billing interval |
| `plan.weekly` | Weekly billing interval |
| `plan.yearly` | Yearly billing interval |
| `plan.daily` | Daily billing interval |
| `plan.every_3_months` | Quarterly billing interval |
| `plan.every_6_months` | Semi-annual billing interval |

### Action Buttons and States
| Key | Usage |
|-----|-------|
| `button.pause` | Pause subscription button |
| `button.cancel` | Cancel subscription button |
| `button.resume` | Resume subscription button |
| `button.manage` | Manage subscription button |
| `button.view_details` | View details button |
| `button.processing` | Processing state button text |

### Status Messages and Notifications
| Key | Usage |
|-----|-------|
| `subscription.paused_until` | Paused until date message (with date parameter) |
| `subscription.cancelled_message` | Cancellation confirmation message |
| `subscription.will_expire_on` | Expiration notice (with date parameter) |
| `subscription.auto_renews_on` | Auto renewal notice (with date parameter) |
| `subscription.trial_expires_on` | Trial expiration notice (with date parameter) |

### Error and Warning Messages
| Key | Usage |
|-----|-------|
| `subscription.payment_failed` | Payment failure warning |
| `subscription.expiring_soon` | Expiring soon warning |
| `subscription.action_in_progress` | Action in progress message |
| `subscription.unable_to_process` | Processing error message |

### Translation Usage Examples
```vue
<!-- Subscription status display -->
<div class="subscription-status">
  <span v-if="subscription.status === 'active'">
    {{ $t('subscription.active') }}
  </span>
  <span v-else-if="subscription.status === 'trialing'">
    {{ $t('subscription.trialing') }}
  </span>
</div>

<!-- Credits remaining display -->
<div class="credits-info">
  <template v-if="subscription.max_bookings">
    {{ remainingCredits }} {{ $t('subscription.credits', remainingCredits) }} 
    {{ $t('subscription.remaining_credits') }}
  </template>
  <template v-else>
    {{ $t('subscription.unlimited_access') }}
  </template>
</div>

<!-- Billing period display -->
<div class="billing-period">
  {{ formatCurrency(subscription.price) }} 
  {{ $t('subscription.per_separator') }} 
  {{ $t(`plan.${subscription.billing_interval}`) }}
</div>

<!-- Trial end notice -->
<div v-if="subscription.trial_ends_at" class="trial-notice">
  {{ $t('subscription.trial_expires_on', { date: formatDate(subscription.trial_ends_at) }) }}
</div>

<!-- Action buttons with state -->
<codex-button 
  :default-text="isProcessing ? $t('button.processing') : $t('button.pause')"
  :disabled="isProcessing"
  @click="$emit('pause', subscription)"
/>
```

## Examples

### Basic Implementation
```vue
<codex-subscription-card
  :subscription="subscription"
  @cancel="handleCancel"
  @pause="handlePause"
/>
```

### With Loading State
```vue
<codex-subscription-card
  :subscription="subscription"
  :loading="isLoading"
  :enable-border="true"
/>
```

### With Processing State
```vue
<codex-subscription-card
  :subscription="subscription"
  :is-processing="isProcessing"
  :is-recently-cancelled="recentlyCancelled"
/>
```

### Custom Header Content
```vue
<codex-subscription-card
  :subscription="subscription"
  @cancel="handleCancel"
>
  <template #header="{ subscription }">
    <div class="subscription-header">
      <div class="subscription-status" :class="getStatusClass(subscription.status)">
        {{ subscription.status_detail }}
      </div>
      <h3 class="subscription-name">{{ subscription.name }}</h3>
      <p class="subscription-description">{{ subscription.plan.description }}</p>
      <div class="subscription-badges">
        <span v-if="subscription.status === 'trialing'" class="trial-badge">
          Trial Period
        </span>
      </div>
    </div>
  </template>
</codex-subscription-card>
```

### Custom Footer with Additional Actions
```vue
<codex-subscription-card
  :subscription="subscription"
  @cancel="handleCancel"
  @pause="handlePause"
>
  <template #footer="{ subscription }">
    <div class="subscription-actions">
      <div class="action-buttons">
        <button @click="viewBillingHistory(subscription)">View Billing</button>
        <button @click="modifySubscription(subscription)">Modify Plan</button>
        <button @click="transferCredits(subscription)">Transfer Credits</button>
      </div>
      
      <div class="default-actions">
        <codex-button 
          variant="secondary"
          :default-text="'Pause'"
          @click="$emit('pause', subscription)"
        />
        <codex-button 
          variant="primary"
          :disabled="!subscription.can_cancel"
          :default-text="'Cancel'"
          @click="$emit('cancel', subscription)"
        />
      </div>
    </div>
  </template>
</codex-subscription-card>
```

### In Subscriptions List
```vue
<div class="subscriptions-container">
  <codex-subscription-card
    v-for="subscription in subscriptions"
    :key="subscription.id"
    :subscription="subscription"
    :is-processing="processingIds.includes(subscription.id)"
    :is-recently-cancelled="cancelledIds.includes(subscription.id)"
    @cancel="handleCancelSubscription"
    @pause="handlePauseSubscription"
  />
</div>
```

### With Event Tracking
```vue
<codex-subscription-card
  :subscription="subscription"
  @cancel="trackCancelSubscription"
  @pause="trackPauseSubscription"
/>

<script setup>
const trackCancelSubscription = (subscription) => {
  analytics.track('subscription_cancel_initiated', {
    subscription_id: subscription.id,
    plan_id: subscription.plan.id,
    billing_interval: subscription.billing_interval,
    credits_remaining: subscription.max_bookings - subscription.bookings_made
  })
  emit('cancel', subscription)
}

const trackPauseSubscription = (subscription) => {
  analytics.track('subscription_pause_initiated', {
    subscription_id: subscription.id,
    plan_id: subscription.plan.id,
    period_remaining_days: calculateRemainingDays(subscription.period_end)
  })
  emit('pause', subscription)
}
</script>
```

### Trial Subscription Display
```vue
<codex-subscription-card
  :subscription="trialSubscription"
/>

<!-- 
Automatically displays trial-specific information when 
subscription.status === 'trialing' and trial_ends_at is present
-->
```

### Unlimited Subscription Display
```vue
<codex-subscription-card
  :subscription="unlimitedSubscription"
/>

<!-- 
Handles unlimited subscriptions when 
subscription.max_bookings === null
-->
```

## CSS Classes
- `_c-card`: Main container class
- `_c-subscription-card`: Subscription card specific styling
- `_c-border`: Border styling (when enabled)
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-title`: Title styling
- `_c-desc`: Description text styling
- `_c-focal-text`: Focal/prominent text styling
- `_c-plan-price`: Plan price styling
- `_c-per-billing`: Billing period text styling
- `_c-product-credits`: Product credits information
- `_c-price-per-credit`: Per credit price styling
- `_c-subtitle`: Subtitle styling
- `_c-mt-lg`: Large top margin utility
- `_c-mb-sm`: Small bottom margin utility
- `_c-plan-info`: Plan information row styling
- `_c-remaining-credits`: Remaining credits container
- `_c-start-date`: Start date container
- `_c-end-date`: End date container
- `_c-renewal-date`: Renewal date container
- `_c-trial-ends`: Trial end date container
- `_c-info`: Information label styling
- `_c-data`: Data value styling
- `_c-btn-container`: Button container styling
- `_c-fill`: Fill container utility
- `_c-btn`: Button styling

## Best Practices

### Recommended Usage Patterns
- Always provide proper subscription object structure
- Handle loading states for better UX
- Implement proper cancellation and pause confirmation flows
- Display subscription status clearly
- Handle trial periods appropriately
- Provide clear billing information
- Track subscription management actions
- Handle unlimited subscriptions properly

### Common Pitfalls to Avoid
- Not handling missing subscription data gracefully
- Missing loading state implementation
- Forgetting to handle different subscription statuses
- Not providing proper confirmation for management actions
- Missing responsive considerations
- Insufficient handling of trial periods
- Not tracking management action states properly
- Missing accessibility considerations

### Accessibility Considerations
- Ensure cards are keyboard navigable
- Provide clear labels for interactive elements
- Use appropriate ARIA attributes for status indicators
- Ensure adequate color contrast for status displays
- Provide screen reader friendly subscription information
- Include proper focus management
- Use semantic HTML for subscription data
- Handle disabled states properly for screen readers

### Error Handling
- Handle missing subscription data gracefully
- Provide fallbacks for malformed plan objects
- Handle management action failures appropriately
- Display meaningful error states
- Clear error states when data updates
- Handle billing calculation errors
- Provide user feedback for failed operations

### State Management
- Track subscription status properly
- Handle status changes dynamically
- Manage loading states consistently
- Update UI after management actions
- Handle concurrent state changes
- Coordinate with parent component state
- Manage processing states appropriately

### Performance Considerations
- Optimize subscription data rendering
- Minimize re-renders during state changes
- Handle large subscription lists efficiently
- Implement proper cleanup for event listeners
- Cache formatted subscription data appropriately
- Optimize billing calculations
- Handle real-time subscription updates efficiently

### Subscription Management
- Validate management actions based on subscription status
- Handle cancellation and pause policies properly
- Provide clear action feedback
- Handle partial management failures
- Update subscription state after successful actions
- Provide management progress feedback
- Handle management action cancellation properly

### Billing Display
- Format currency consistently
- Handle different billing intervals properly
- Calculate per-credit costs accurately
- Display billing dates in user's timezone
- Handle billing calculation edge cases
- Provide clear billing period information
- Format billing intervals appropriately

### Status Management
- Display subscription status clearly
- Handle status transitions properly
- Update visual states immediately
- Provide clear status feedback
- Handle edge cases in status logic
- Coordinate status with parent components
- Track status changes for analytics

### Trial Management
- Display trial periods clearly
- Handle trial-to-paid conversions
- Provide clear trial end notifications
- Handle trial extension scenarios
- Display trial-specific information
- Coordinate trial status with billing
- Track trial engagement metrics

## Component Registration
The component is registered as `codex-subscription-card` in the application. 