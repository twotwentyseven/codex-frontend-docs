# Subscriptions Component

## Overview
The Subscriptions component provides a comprehensive interface for managing customer subscriptions with advanced management capabilities. It features subscription card displays, cancellation and pause functionality, manage modals, pagination support, and flexible slot-based customization for subscription lifecycle management.

## Basic Usage
```vue
<codex-subscriptions
  :per-page="3"
  :type="'active'"
  :cancellation-reason-required="true"
  :noun="false"
/>
```

## Key Features
- Subscription card display with grid layout
- Subscription management (cancel, pause, resume)
- Management modal integration
- Cancellation reason collection
- Pause scheduling functionality
- Loading states with skeleton placeholders
- No results handling
- Error message display
- Pagination support with filter context
- Customer authentication integration
- Processing state indicators
- Recently cancelled tracking

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| perPage | Number | No | 3 | Number of subscriptions per page |
| type | String | No | 'active' | Type of subscriptions to display ('active' or 'inactive') |
| cancellationReasonRequired | Boolean | No | true | Whether cancellation reason is required |
| noun | String\|Boolean | No | false | Custom noun for subscription display |
| group | String | No | undefined | Filter context group for pagination and filtering |

### Common Props
All common props from `@/config/common` are supported, including:
| Prop Name | Usage |
|-----------|-------|
| enableBorder | Adds border styling to subscription cards |
| loadingItems | Number of skeleton items to show during loading |

### Common Functions
The component uses utilities from `useCommon`:
| Function | Usage |
|----------|-------|
| loadingItems | Provides skeleton loading item configuration |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| subscription-cancelled | `subscription: Object, reason: String` | Emitted when a subscription is cancelled |
| subscription-paused | `subscription: Object, pauses: Array` | Emitted when subscription pauses are set |
| subscription-resumed | `subscription: Object` | Emitted when a subscription is resumed |
| subscriptions-loaded | `subscriptions: Array` | Emitted when subscriptions are loaded |

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content>
  <!-- Custom content and subscription display -->
</template>
```

### Footer Slot
```vue
<template #footer="{ pagination }">
  <!-- Custom footer content (pagination by default) -->
</template>
```

### Slot Props
| Prop Name | Type | Description |
|-----------|------|-------------|
| pagination | Object | Pagination state information |

## States
The component has multiple states in order of priority:
1. **Unauthenticated State** - Component hidden when no customer
2. **Loading State** - Shows skeleton subscription cards during data fetch
3. **Error State** - Displays error messages
4. **No Results State** - Shows no subscriptions message
5. **Subscriptions Display State** - Normal subscription grid display
6. **Management Modal State** - Subscription management modal for cancel/pause actions
7. **Processing State** - Individual subscription processing indicators

## Subscription Management
The component supports comprehensive subscription management:

### Cancellation Flow
- **Initiate**: User clicks cancel on subscription card
- **Modal**: Management modal opens with cancellation options
- **Reason**: Collects cancellation reason (if required)
- **Confirm**: Processes cancellation request
- **Feedback**: Shows processing and success states

### Pause Management
- **Initiate**: User clicks pause on subscription card
- **Modal**: Management modal opens with pause scheduling
- **Schedule**: User sets upcoming pause periods
- **Confirm**: Processes pause schedule
- **Feedback**: Shows processing and success states

## Processing States
- **Individual Processing**: Tracks specific subscriptions being processed
- **Recently Cancelled**: Tracks recently cancelled subscriptions for feedback
- **Modal State Management**: Handles modal states for different actions

## Internationalization
The component uses the following translation keys:
- `credit.you_have_no_type_credits`: No subscriptions message (reused)
- Subscription management modal translations
- Cancellation reason translations
- Pause scheduling translations

## Examples

### Basic Implementation
```vue
<codex-subscriptions />
```

### Active Subscriptions with Custom Settings
```vue
<codex-subscriptions
  :type="'active'"
  :per-page="6"
  :cancellation-reason-required="false"
/>
```

### Inactive Subscriptions
```vue
<codex-subscriptions
  :type="'inactive'"
  noun="membership"
/>
```

### With Custom Header
```vue
<codex-subscriptions>
  <template #header>
    <div class="subscriptions-header">
      <h2>Your Active Subscriptions</h2>
      <p>Manage your subscription settings and preferences</p>
    </div>
  </template>
</codex-subscriptions>
```

### Custom Content with Additional Actions
```vue
<codex-subscriptions>
  <template #content>
    <div class="subscriptions-management">
      <div class="bulk-actions">
        <button @click="pauseAll">Pause All</button>
        <button @click="managePayment">Update Payment</button>
      </div>
      
      <div class="_c-grid _c-w-full">
        <codex-subscription-card 
          v-for="subscription in subscriptions"
          :key="subscription.id"
          :subscription="subscription"
          @cancel="handleCancel"
          @pause="handlePause"
        />
      </div>
    </div>
  </template>
</codex-subscriptions>
```

### With Event Handlers
```vue
<codex-subscriptions
  @subscription-cancelled="handleSubscriptionCancelled"
  @subscription-paused="handleSubscriptionPaused"
  @subscription-resumed="handleSubscriptionResumed"
  @subscriptions-loaded="trackSubscriptionsLoaded"
/>
```

### Custom Footer with Subscription Statistics
```vue
<codex-subscriptions>
  <template #footer="{ pagination }">
    <div class="subscriptions-footer">
      <div class="subscription-stats">
        <span>Active: {{ activeCount }}</span>
        <span>Monthly Value: {{ monthlyTotal }}</span>
        <span>Next Billing: {{ nextBillingDate }}</span>
      </div>
      <codex-pagination :group="group" />
    </div>
  </template>
</codex-subscriptions>
```

### With Filter Context
```vue
<codex-subscriptions
  :group="'filtered-subscriptions'"
  :type="'active'"
/>
```

## CSS Classes
- `codex`: Base component identifier
- `_c-card`: Main container class
- `_c-subscriptions-card`: Subscriptions-specific card styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-grid`: Subscription cards grid layout
- `_c-w-full`: Full width utility
- `_c-noresults`: No results state styling

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Implement proper loading states during subscription fetch
- Provide clear subscription management options
- Use appropriate subscription types for different contexts
- Handle subscription state changes with proper feedback
- Implement error retry mechanisms
- Use pagination for large subscription datasets
- Collect cancellation reasons for business insights

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing loading states during data fetch
- Forgetting to handle subscription state changes
- Not providing clear cancellation confirmation
- Missing error handling for failed operations
- Not updating subscription list after management actions
- Insufficient handling of payment-related errors
- Not tracking subscription processing states

### Accessibility Considerations
- Ensure subscription cards are keyboard navigable
- Provide clear labels for management actions
- Use appropriate ARIA attributes for dynamic content
- Ensure modal dialogs are accessible
- Provide screen reader friendly subscription information
- Include proper focus management for modals
- Use semantic HTML for subscription data

### Error Handling
- Display clear error messages for failed loads
- Provide retry mechanisms for network failures
- Handle subscription management errors gracefully
- Show validation errors for management actions
- Clear error states when operations succeed
- Handle authentication errors appropriately
- Provide fallback states for partial failures

### State Management
- Track subscription state properly
- Handle customer authentication changes
- Manage loading states consistently
- Update subscription list after management actions
- Handle modal state transitions
- Manage processing states for individual subscriptions
- Coordinate with pagination and filter state

### Performance Considerations
- Implement virtual scrolling for large subscription lists
- Lazy load subscription card components
- Optimize grid rendering performance
- Handle large management operations efficiently
- Consider pagination for better performance
- Implement proper cleanup for event listeners
- Cache subscription data appropriately

### Subscription Management
- Validate subscription eligibility before actions
- Provide clear management confirmation
- Handle partial management failures
- Update UI state after successful actions
- Provide management progress feedback
- Handle management cancellation properly
- Clear management state appropriately

### Cancellation Management
- Collect meaningful cancellation reasons
- Provide clear cancellation confirmation
- Handle different cancellation policies
- Track cancellation analytics
- Provide retention options when appropriate
- Handle immediate vs end-of-period cancellations
- Clear cancellation state after processing

### Pause Management
- Allow flexible pause scheduling
- Validate pause date ranges
- Handle overlapping pause periods
- Provide clear pause confirmation
- Track pause analytics
- Handle pause limitations and restrictions
- Update billing cycles appropriately

### Filter Context Integration
- Use appropriate group names for filtering
- Handle filter state changes
- Coordinate pagination with filters
- Update subscriptions when filters change
- Handle filter reset scenarios
- Provide clear filter feedback
- Maintain filter state across navigation

## Component Registration
The component is registered as `codex-subscriptions` in the application. 