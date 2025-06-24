# SubscriptionManageModal Component

## Overview
The SubscriptionManageModal component provides a comprehensive modal interface for managing subscription actions including cancellation, pausing, and general management. It features multi-mode operation, cancellation reason selection, pause period scheduling, confirmation workflows, and processing states for secure subscription management operations.

## Basic Usage
```vue
<codex-subscription-manage-modal
  :subscription="subscriptionData"
  :is-processing="false"
  :is-recently-cancelled="false"
  :cancellation-reasons="cancellationReasons"
  :modal-name="'subscription-manage'"
  :state="'manage'"
  @cancel="handleCancelSubscription"
  @setpauses="handleSetPauses"
  @close="closeModal"
/>
```

## Key Features
- Multi-mode subscription management interface
- Subscription cancellation with reason selection
- Pause scheduling with billing period selection
- Processing states and user feedback
- Confirmation workflows for actions
- Modal-based interface with proper state management
- Cancellation reason validation
- Pause limit enforcement
- Success state handling
- Error state management and display

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| subscription | Object | Yes | - | Subscription object to manage |
| isProcessing | Boolean | No | false | Whether subscription actions are processing |
| isRecentlyCancelled | Boolean | Yes | - | Whether subscription was recently cancelled |
| cancellationReasons | Object | Yes | - | Available cancellation reasons |
| cancellationReasonRequired | Boolean | No | true | Whether cancellation reason is required |
| upcomingPausesSuccess | Boolean | No | false | Whether pause operation was successful |
| modalName | String | Yes | - | Unique modal identifier |
| state | String | No | '' | Initial modal state ('manage', 'cancel', 'pause') |

### Common Props
All common props from `@/config/common` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| cancel | `subscriptionId: String, cancellationReason: String` | Emitted when subscription cancellation is confirmed |
| setpauses | `subscriptionId: String, pauses: Array` | Emitted when pause periods are set |
| close | - | Emitted when modal should be closed |

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content -->
</template>
```

### Footer Slot
```vue
<template #footer>
  <!-- Custom footer content -->
</template>
```

## Modal States
The component operates in multiple states based on the `mode` reactive property:

### Manage Mode (`mode === 'manage'`)
- **Purpose**: Main management interface
- **Display**: Shows cancel and pause options
- **Actions**: Navigate to cancel or pause modes
- **Conditions**: Varies based on subscription capabilities

### Cancel Mode (`mode === 'cancel'`)
- **Purpose**: Subscription cancellation workflow
- **Display**: Cancellation confirmation and reason selection
- **Actions**: Confirm cancellation or return to manage
- **Validation**: Requires cancellation reason if configured

### Pause Mode (`mode === 'pause'`)
- **Purpose**: Subscription pause scheduling
- **Display**: Billing period selection with checkboxes
- **Actions**: Schedule pauses or return to manage
- **Limits**: Enforces maximum pause limits

### Success States
- **Cancelled State**: Shows cancellation confirmation
- **Pause Success**: Shows pause operation success

## Subscription Object Structure
The subscription prop expects an object with the following structure:
```javascript
{
  id: String|Number,              // Unique subscription identifier
  name: String,                   // Subscription name
  can_cancel: Boolean,            // Whether subscription can be cancelled
  can_pause: Boolean,             // Whether subscription can be paused
  has_scheduled_pause: Boolean,   // Whether subscription has scheduled pauses
  period_end: Date,               // Current period end date
  remaining_pause_count: Number,  // Remaining pause periods allowed
  upcoming_billing_periods: [     // Array of upcoming billing periods
    {
      start: Date,               // Period start date
      end: Date,                 // Period end date
      pausable: Boolean          // Whether period can be paused
    }
  ]
}
```

## Cancellation Management
The component provides comprehensive cancellation workflows:

### Cancellation Reason Selection
- **Dropdown Field**: Selects from available cancellation reasons
- **Validation**: Ensures reason is selected if required
- **Dynamic Display**: Shows only when cancellation reason is required

### Cancellation Process
- **Confirmation Required**: Uses confirmation dialog for cancellation
- **Processing States**: Shows processing feedback during cancellation
- **Success Handling**: Displays success state after cancellation
- **Button States**: Dynamic button text based on operation state

## Pause Management
The component includes sophisticated pause scheduling:

### Billing Period Selection
- **Checkbox Interface**: Select multiple billing periods to pause
- **Period Display**: Shows formatted date ranges for each period
- **Pause Limits**: Enforces maximum pause count restrictions
- **Dynamic Disabling**: Disables selection when limit reached

### Pause Validation
- **Limit Enforcement**: Prevents exceeding maximum pause count
- **Pausable Periods**: Only shows pausable billing periods
- **Current Pauses**: Tracks currently selected pause periods

## Modal State Management
The component manages modal state through multiple mechanisms:

### External State Control
- **State Prop**: Accepts initial state from parent
- **State Watching**: Reacts to state changes from parent
- **Modal Auto-open**: Opens modal when state is provided

### Internal State Transitions
- **Mode Switching**: Internal navigation between modes
- **State Persistence**: Maintains state during modal operations
- **Reset Logic**: Clears state on modal close

## Processing States
The component handles multiple processing states:

### Loading Indicators
- **Processing Prop**: External processing state
- **Button States**: Dynamic button text during processing
- **Success States**: Success feedback for operations

### User Feedback
- **Processing Text**: Clear processing indication
- **Success Messages**: Confirmation of successful operations
- **Error Handling**: Graceful error state management

## Internationalization
The component uses the following translation keys:
- `subscription.membership_cancelled`: Cancelled state title
- `subscription.manage_your_subscription`: Management mode title
- `subscription.cancel_your_membership`: Cancellation mode title
- `subscription.pause_your_membership`: Pause mode title
- `subscription.membership_cancelled_on`: Cancellation date message
- `subscription.you_can_pause_or_cancel`: Management mode description
- `subscription.you_are_currently_unable_to_cancel_or_pause`: Disabled management description
- `subscription.are_you_sure_you_want_to_cancel`: Cancellation confirmation
- `subscription.are_you_sure_you_want_to_pause`: Pause confirmation
- `subscription.reason_for_cancelling`: Cancellation reason label
- `subscription.back`: Back button text
- `subscription.cancel`: Cancel button text
- `subscription.pause`: Pause button text
- `subscription.cancelling`: Cancelling processing text
- `subscription.pausing`: Pausing processing text
- `subscription.pause_was_updated`: Pause success message
- `button.close`: Close button text

## Examples

### Basic Implementation
```vue
<codex-subscription-manage-modal
  :subscription="subscription"
  :is-processing="processing"
  :is-recently-cancelled="cancelled"
  :cancellation-reasons="reasons"
  :modal-name="'manage-subscription'"
  @cancel="handleCancel"
  @setpauses="handlePauses"
  @close="closeModal"
/>
```

### With State Control
```vue
<codex-subscription-manage-modal
  :subscription="subscription"
  :is-processing="isProcessing"
  :is-recently-cancelled="recentlyCancelled"
  :cancellation-reasons="cancellationReasons"
  :upcoming-pauses-success="pauseSuccess"
  :modal-name="'subscription-manage'"
  :state="modalState"
  @cancel="handleCancelSubscription"
  @setpauses="handleSchedulePauses"
  @close="handleCloseModal"
/>
```

### Custom Header Content
```vue
<codex-subscription-manage-modal
  :subscription="subscription"
  :is-processing="isProcessing"
  :is-recently-cancelled="recentlyCancelled"
  :cancellation-reasons="cancellationReasons"
  :modal-name="'manage-subscription'"
>
  <template #header>
    <div class="custom-header">
      <div class="subscription-info">
        <h3>Manage {{ subscription.name }}</h3>
        <div class="subscription-meta">
          <span>Status: {{ subscription.status }}</span>
          <span>Next Billing: {{ formatDate(subscription.period_end) }}</span>
        </div>
      </div>
      
      <div class="action-context" v-if="mode === 'cancel'">
        <div class="warning-notice">
          <i class="warning-icon"></i>
          <p>Cancelling will end your subscription benefits at the end of the current billing period.</p>
        </div>
      </div>
      
      <div class="pause-info" v-if="mode === 'pause'">
        <div class="pause-limits">
          <span>Pauses Remaining: {{ subscription.remaining_pause_count }}</span>
          <span>Current Selection: {{ currentPauses }}</span>
        </div>
      </div>
    </div>
  </template>
</codex-subscription-manage-modal>
```

### Custom Footer Actions
```vue
<codex-subscription-manage-modal
  :subscription="subscription"
  :is-processing="isProcessing"
  :is-recently-cancelled="recentlyCancelled"
  :cancellation-reasons="cancellationReasons"
  :modal-name="'manage-subscription'"
>
  <template #footer>
    <div class="custom-footer">
      <div class="help-section" v-if="mode === 'manage'">
        <p>Need help? <a href="/support">Contact Support</a></p>
      </div>
      
      <div class="action-buttons">
        <template v-if="isRecentlyCancelled">
          <div class="cancellation-info">
            <p>Your subscription will remain active until {{ formatDate(subscription.period_end) }}</p>
          </div>
          <codex-button 
            :default-text="'Close'"
            @click="$emit('close')"
          />
        </template>
        
        <template v-else-if="mode === 'manage'">
          <codex-button 
            variant="secondary"
            :default-text="'Contact Support'"
            @click="contactSupport"
          />
          
          <codex-button 
            variant="secondary"
            :default-text="'Pause'"
            :disabled="!showPause"
            @click="mode = 'pause'"
          />
          
          <codex-button 
            variant="primary"
            :default-text="'Cancel Subscription'"
            :disabled="!subscription.can_cancel"
            @click="mode = 'cancel'"
          />
        </template>
        
        <template v-else>
          <codex-button 
            variant="secondary"
            :default-text="'Back'"
            @click="mode = 'manage'"
          />
          
          <codex-button 
            variant="primary"
            :default-text="mode === 'cancel' ? 'Confirm Cancellation' : 'Schedule Pauses'"
            :disabled="!canProceed"
            :processing="isProcessing"
            @click="handleAction"
          />
        </template>
      </div>
    </div>
  </template>
</codex-subscription-manage-modal>
```

### In Subscription Management Interface
```vue
<div class="subscription-management">
  <div class="subscription-list">
    <div v-for="subscription in subscriptions" :key="subscription.id" class="subscription-item">
      <div class="subscription-details">
        <h4>{{ subscription.name }}</h4>
        <p>{{ subscription.description }}</p>
      </div>
      
      <div class="subscription-actions">
        <button @click="openManageModal(subscription)">
          Manage Subscription
        </button>
      </div>
    </div>
  </div>
  
  <codex-subscription-manage-modal
    v-if="selectedSubscription"
    :subscription="selectedSubscription"
    :is-processing="managementProcessing"
    :is-recently-cancelled="cancelledSubscriptions.includes(selectedSubscription.id)"
    :cancellation-reasons="cancellationReasons"
    :upcoming-pauses-success="pauseOperationSuccess"
    :modal-name="'subscription-management'"
    :state="managementState"
    @cancel="handleSubscriptionCancel"
    @setpauses="handleSubscriptionPause"
    @close="closeManagementModal"
  />
</div>
```

### With Event Tracking
```vue
<codex-subscription-manage-modal
  :subscription="subscription"
  :is-processing="isProcessing"
  :is-recently-cancelled="recentlyCancelled"
  :cancellation-reasons="cancellationReasons"
  :modal-name="'manage-subscription'"
  @cancel="trackCancelSubscription"
  @setpauses="trackPauseSubscription"
  @close="trackModalClose"
/>

<script setup>
const trackCancelSubscription = (subscriptionId, cancellationReason) => {
  analytics.track('subscription_cancellation_confirmed', {
    subscription_id: subscriptionId,
    cancellation_reason: cancellationReason,
    subscription_type: subscription.value.name
  })
  
  emit('cancel', subscriptionId, cancellationReason)
}

const trackPauseSubscription = (subscriptionId, pauses) => {
  analytics.track('subscription_pause_scheduled', {
    subscription_id: subscriptionId,
    pause_count: pauses.filter(Boolean).length,
    subscription_type: subscription.value.name
  })
  
  emit('setpauses', subscriptionId, pauses)
}

const trackModalClose = () => {
  analytics.track('subscription_manage_modal_closed', {
    subscription_id: subscription.value.id,
    mode: mode.value
  })
  
  emit('close')
}
</script>
```

### Advanced Pause Management
```vue
<codex-subscription-manage-modal
  :subscription="subscriptionWithPauses"
  :is-processing="isProcessing"
  :is-recently-cancelled="false"
  :cancellation-reasons="cancellationReasons"
  :modal-name="'advanced-pause-management'"
>
  <template #default="{ close }">
    <div class="advanced-pause-modal" v-if="mode === 'pause'">
      <div class="pause-header">
        <h3>Schedule Subscription Pauses</h3>
        <div class="pause-summary">
          <span>Available Pauses: {{ subscription.remaining_pause_count }}</span>
          <span>Selected: {{ currentPauses }}</span>
        </div>
      </div>
      
      <div class="billing-periods">
        <div class="period-explanation">
          <p>Select the billing periods you'd like to pause. Your subscription will be temporarily suspended during these periods.</p>
        </div>
        
        <div class="periods-grid">
          <div 
            v-for="(period, index) in subscription.upcoming_billing_periods" 
            :key="index"
            class="period-item"
            :class="{ 'selected': pauses[index], 'disabled': !period.pausable }"
          >
            <codex-checkbox-field
              v-model="pauses[index]"
              :label="`${formatDateRange(period.start, period.end)}`"
              :id="`pause_${subscription.id}_${index}`"
              :disabled="currentPauses >= maxPauses && !pauses[index]"
            />
            
            <div class="period-details">
              <span class="period-type">{{ getPeriodType(period) }}</span>
              <span class="period-savings">Save: {{ formatCurrency(subscription.plan.price) }}</span>
            </div>
          </div>
        </div>
      </div>
      
      <div class="pause-actions">
        <codex-button 
          variant="secondary"
          :default-text="'Cancel'"
          @click="close"
        />
        
        <codex-button 
          variant="primary"
          :default-text="'Schedule Pauses'"
          :disabled="currentPauses === 0"
          @click="confirmPauses"
        />
      </div>
    </div>
  </template>
</codex-subscription-manage-modal>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-subscription-manage`: Subscription manage specific styling
- `_c-cancel`: Cancel mode styling
- `_c-pause`: Pause mode styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-title`: Title styling
- `_c-description`: Description styling
- `_c-btn-container`: Button container styling
- `_c-fill`: Fill container utility
- `_c-btn`: Button styling
- `_c-btn--cancel`: Cancel button specific styling
- `_c-btn--pause`: Pause button specific styling

## Best Practices

### Recommended Usage Patterns
- Always provide proper subscription object structure
- Handle modal state transitions appropriately
- Implement proper confirmation workflows
- Provide clear user feedback during processing
- Track subscription management actions
- Handle cancellation and pause limits properly
- Use appropriate validation for required fields
- Implement proper modal cleanup

### Common Pitfalls to Avoid
- Not handling modal state properly
- Missing confirmation requirements for destructive actions
- Forgetting to validate cancellation reasons
- Not enforcing pause limits correctly
- Missing processing state feedback
- Insufficient error handling for failed operations
- Not tracking management analytics
- Missing accessibility considerations

### Accessibility Considerations
- Ensure modal is keyboard navigable
- Provide clear labels for all form fields
- Use appropriate ARIA attributes for modal content
- Ensure adequate color contrast for all elements
- Provide screen reader friendly state information
- Include proper focus management
- Use semantic HTML for modal structure
- Handle disabled states properly for screen readers

### Error Handling
- Handle missing subscription data gracefully
- Provide fallbacks for malformed subscription objects
- Display meaningful error messages for failed operations
- Clear error states when operations succeed
- Handle validation errors appropriately
- Provide retry mechanisms for failed operations
- Handle edge cases in pause/cancel logic

### State Management
- Track modal state properly
- Handle state transitions cleanly
- Manage processing states consistently
- Update UI state after successful operations
- Handle concurrent state changes
- Coordinate with parent component state
- Manage modal lifecycle properly

### Performance Considerations
- Optimize modal rendering performance
- Minimize re-renders during state changes
- Handle large subscription datasets efficiently
- Implement proper cleanup for modal components
- Cache validation results when appropriate
- Optimize state transition animations
- Handle frequent modal operations efficiently

### Modal Management
- Implement proper modal opening/closing
- Handle modal state persistence appropriately
- Coordinate with modal system properly
- Track modal interaction analytics
- Handle modal escape and backdrop clicks
- Provide clear modal navigation
- Manage modal focus appropriately

### Subscription Operations
- Validate subscription actions before execution
- Provide clear action confirmation
- Handle partial operation failures
- Update subscription state after successful operations
- Provide operation progress feedback
- Handle operation cancellation properly
- Clear operation state appropriately

### Validation Management
- Implement proper form validation
- Handle required field validation
- Validate pause period selections
- Ensure cancellation reason compliance
- Provide real-time validation feedback
- Handle validation edge cases
- Clear validation state appropriately

### Confirmation Workflows
- Require confirmation for destructive actions
- Provide clear confirmation messaging
- Handle confirmation cancellation
- Track confirmation interactions
- Implement multi-step confirmations when needed
- Provide confirmation success feedback
- Handle confirmation timeouts appropriately

## Component Registration
The component is registered as `codex-subscription-manage-modal` in the application. 