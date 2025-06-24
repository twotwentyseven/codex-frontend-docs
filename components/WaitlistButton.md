# WaitlistButton Component

## Overview
The WaitlistButton component provides a simple interface for customers to join or leave event waitlists. It displays contextual button text based on waitlist status and includes optional messaging for fully booked events. The component integrates with the waitlist system to handle join/leave operations with proper error handling and success feedback.

## Basic Usage
```vue
<template>
  <div class="event-actions">
    <codex-waitlist-button 
      :event="selectedEvent"
      :show-waitlist-message="true"
    />
  </div>
</template>

<script setup>
const selectedEvent = {
  id: 123,
  is_waitlistable: true,
  is_fully_booked: true
}
</script>
```

## Key Features
- Join and leave waitlist functionality
- Dynamic button text based on waitlist status
- Event full booking message display
- Loading states during operations
- Error handling with user feedback
- Success confirmation with customizable messages
- Automatic waitlist status detection
- Event-based action handling

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `event` | `Object` | `required` | Event object containing id and waitlist properties |

### Display Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showWaitlistMessage` | `Boolean` | `true` | Show "event is fully booked" message when not on waitlist |

## Computed Properties

### Waitlist State
- `waitlist` - Current waitlist entries for the customer
- `loading` - Loading state for waitlist operations
- `error` - Error state for failed operations
- `success` - Success state with feedback message

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `waitlist-joined` | `event` | Emitted when customer joins waitlist |
| `waitlist-left` | `event` | Emitted when customer leaves waitlist |

## Slots

### Waitlist Message Slot
| Slot | Props | Description |
|------|-------|-------------|
| `waitlist-message` | `{ waitlist }` | Custom message for fully booked events |

## Loading States

The component provides loading states through:
- Button loading state during operations
- Disabled state during processing
- Loading text feedback

## Waitlist Integration

The component integrates with the waitlist system through:
- `useEventWaitlists` composition function
- Automatic initialization on mount
- Real-time waitlist status updates
- Error handling for failed operations

## Internationalization

The component uses the following translation keys:
- `waitlist.event_is_fully_booked`: Message displayed when event is fully booked
- `waitlist.join_waitlist`: Join waitlist button text
- `waitlist.leave_waitlist`: Leave waitlist button text
- `button.processing`: Processing state button text
- `waitlist.there_was_a_problem`: Error message for failed operations

### Translation Usage Examples
```vue
<!-- Waitlist status message -->
<div v-if="event.is_fully_booked && !waitlist.length">
  {{ $t('waitlist.event_is_fully_booked') }}
</div>

<!-- Dynamic button text based on waitlist status -->
<codex-button 
  :defaultText="waitlist.length ? $t('waitlist.leave_waitlist') : $t('waitlist.join_waitlist')"
  :processingText="$t('button.processing')"
  :errorText="$t('waitlist.there_was_a_problem')"
/>

<!-- Custom waitlist message with translation -->
<template #waitlist-message>
  <div class="custom-message">
    {{ $t('waitlist.custom_fully_booked_message') }}
  </div>
</template>
```

## Examples

### Basic Waitlist Button
```vue
<template>
  <div class="simple-waitlist">
    <codex-waitlist-button 
      :event="event"
    />
  </div>
</template>

<script setup>
const event = {
  id: 456,
  is_waitlistable: true,
  is_fully_booked: true
}
</script>
```

### Custom Waitlist Message
```vue
<template>
  <div class="custom-waitlist">
    <codex-waitlist-button 
      :event="fullyBookedEvent"
      :show-waitlist-message="true"
    >
      <template #waitlist-message="{ waitlist }">
        <div v-if="!waitlist.length" class="custom-full-message">
          <div class="full-indicator">
            <i class="ri-group-fill"></i>
            <span>This class is completely full!</span>
          </div>
          <div class="waitlist-info">
            <p>Join the waitlist to be notified if a space becomes available.</p>
            <div class="waitlist-benefits">
              <div class="benefit">
                <i class="ri-notification-line"></i>
                <span>Instant notifications</span>
              </div>
              <div class="benefit">
                <i class="ri-time-line"></i>
                <span>Priority booking</span>
              </div>
              <div class="benefit">
                <i class="ri-smartphone-line"></i>
                <span>SMS & email alerts</span>
              </div>
            </div>
          </div>
        </div>
      </template>
    </codex-waitlist-button>
  </div>
</template>

<script setup>
const fullyBookedEvent = {
  id: 789,
  name: 'Hot Yoga Flow',
  is_waitlistable: true,
  is_fully_booked: true,
  capacity: 20,
  occupancy: 20
}
</script>

<style scoped>
.custom-full-message {
  background: #fff3cd;
  border: 1px solid #ffeaa7;
  border-radius: 8px;
  padding: 1rem;
  margin-bottom: 1rem;
}

.full-indicator {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-weight: bold;
  color: #856404;
  margin-bottom: 0.75rem;
}

.waitlist-info p {
  margin-bottom: 1rem;
  color: #6c757d;
}

.waitlist-benefits {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.benefit {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  color: #495057;
}

.benefit i {
  color: #007bff;
}
</style>
```

### Waitlist with Event Tracking
```vue
<template>
  <div class="tracked-waitlist">
    <div class="event-header">
      <h3>{{ event.name }}</h3>
      <div class="event-status">
        <span v-if="event.is_fully_booked" class="status-full">
          <i class="ri-user-fill"></i>
          Full ({{ event.occupancy }}/{{ event.capacity }})
        </span>
        <span v-else class="status-available">
          <i class="ri-user-line"></i>
          {{ event.capacity - event.occupancy }} spaces left
        </span>
      </div>
    </div>
    
    <codex-waitlist-button 
      :event="event"
      :show-waitlist-message="false"
      @waitlist-joined="handleWaitlistJoined"
      @waitlist-left="handleWaitlistLeft"
    />
    
    <div v-if="userMessage" class="user-feedback">
      <div :class="['feedback-message', messageType]">
        <i :class="messageIcon"></i>
        {{ userMessage }}
      </div>
    </div>
    
    <div class="waitlist-stats">
      <div class="stat-item">
        <span class="stat-label">Waitlist Position:</span>
        <span class="stat-value">{{ waitlistPosition || 'Not on waitlist' }}</span>
      </div>
      <div class="stat-item">
        <span class="stat-label">Estimated Wait:</span>
        <span class="stat-value">{{ estimatedWaitTime }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const event = ref({
  id: 101112,
  name: 'Power Yoga',
  is_waitlistable: true,
  is_fully_booked: true,
  capacity: 15,
  occupancy: 15,
  start_at: '2024-01-20T10:00:00'
})

const userMessage = ref('')
const messageType = ref('success')
const waitlistPosition = ref(null)

const messageIcon = computed(() => {
  switch (messageType.value) {
    case 'success': return 'ri-checkbox-circle-line'
    case 'error': return 'ri-error-warning-line'
    case 'info': return 'ri-information-line'
    default: return 'ri-information-line'
  }
})

const estimatedWaitTime = computed(() => {
  if (!waitlistPosition.value) return 'N/A'
  
  // Simple estimation based on position
  const hoursBeforeEvent = 2
  const dropoutRate = 0.3 // 30% of people typically don't show
  
  if (waitlistPosition.value <= 3) {
    return 'Very likely'
  } else if (waitlistPosition.value <= 5) {
    return 'Possible'
  } else {
    return 'Unlikely'
  }
})

const handleWaitlistJoined = (joinedEvent) => {
  console.log('Joined waitlist for event:', joinedEvent.id)
  
  // Simulate getting waitlist position
  waitlistPosition.value = Math.floor(Math.random() * 8) + 1
  
  userMessage.value = `You're #${waitlistPosition.value} on the waitlist! We'll notify you if a space opens up.`
  messageType.value = 'success'
  
  // Clear message after 5 seconds
  setTimeout(() => {
    userMessage.value = ''
  }, 5000)
}

const handleWaitlistLeft = (leftEvent) => {
  console.log('Left waitlist for event:', leftEvent.id)
  
  waitlistPosition.value = null
  userMessage.value = 'You\'ve been removed from the waitlist.'
  messageType.value = 'info'
  
  // Clear message after 3 seconds
  setTimeout(() => {
    userMessage.value = ''
  }, 3000)
}
</script>

<style scoped>
.tracked-waitlist {
  border: 1px solid #dee2e6;
  border-radius: 8px;
  padding: 1.5rem;
  background: white;
}

.event-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.event-header h3 {
  margin: 0;
  font-size: 1.25rem;
}

.status-full {
  color: #dc3545;
  font-weight: bold;
}

.status-available {
  color: #28a745;
  font-weight: bold;
}

.user-feedback {
  margin: 1rem 0;
}

.feedback-message {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem;
  border-radius: 6px;
  font-size: 0.875rem;
}

.feedback-message.success {
  background: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
}

.feedback-message.error {
  background: #f8d7da;
  color: #721c24;
  border: 1px solid #f5c6cb;
}

.feedback-message.info {
  background: #d1ecf1;
  color: #0c5460;
  border: 1px solid #bee5eb;
}

.waitlist-stats {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid #dee2e6;
}

.stat-item {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.stat-label {
  font-size: 0.875rem;
  color: #6c757d;
}

.stat-value {
  font-weight: bold;
  color: #495057;
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-event-full` | Styling for "event is fully booked" message |

## Internationalization

The component uses the following translation keys:

| Key | Default | Usage |
|-----|---------|-------|
| `waitlist.event_is_fully_booked` | `"This event is fully booked"` | Full event message |
| `waitlist.join_waitlist` | `"Join Waitlist"` | Join button text |
| `waitlist.leave_waitlist` | `"Leave Waitlist"` | Leave button text |
| `button.processing` | `"Processing..."` | Loading button text |
| `error.there_was_a_problem` | `"There was a problem"` | Error button text |

## Best Practices

### Waitlist Management
- Clearly indicate waitlist status to users
- Provide realistic expectations about availability
- Handle concurrent waitlist operations gracefully
- Notify users of their waitlist position when possible
- Implement proper cleanup on component unmount

### User Experience
- Use clear, actionable button text
- Provide immediate feedback for all actions
- Show loading states during operations
- Display helpful messages for fully booked events
- Consider mobile touch targets for buttons

### Error Handling
- Display meaningful error messages
- Handle network failures gracefully
- Provide retry mechanisms for failed operations
- Log errors for debugging purposes
- Implement fallback states for edge cases

### Performance
- Initialize waitlist data efficiently
- Avoid unnecessary API calls
- Implement proper state management
- Handle rapid button clicks gracefully
- Cache waitlist status when appropriate

### Accessibility
- Ensure button has proper labels
- Support keyboard navigation
- Use appropriate ARIA attributes
- Provide screen reader feedback
- Test with assistive technologies

### Mobile Considerations
- Ensure touch targets are adequate size
- Test on various screen sizes
- Handle touch events properly
- Consider offline scenarios
- Implement proper error recovery

## Component Registration
```javascript
// Global registration
app.component('CodexWaitlistButton', WaitlistButton)

// Local registration  
import WaitlistButton from '@/components/bookings/WaitlistButton.vue'

export default {
  components: {
    CodexWaitlistButton: WaitlistButton
  }
}
``` 