# WaitlistConfirmation Component

## Overview
The WaitlistConfirmation component displays a booking confirmation interface when a customer has successfully joined a waitlist and a spot becomes available. It provides a complete event booking flow with credit validation, payment method selection, and booking confirmation. The component handles waitlist-to-booking conversion with comprehensive state management and user feedback.

## Basic Usage
```vue
<template>
  <div class="waitlist-confirmation">
    <codex-waitlist-confirmation 
      :event-id="selectedEventId"
    />
  </div>
</template>

<script setup>
const selectedEventId = 123
</script>
```

## Key Features
- Waitlist confirmation and booking conversion
- Credit and subscription validation
- Payment method selection with multiple options
- Event details display with timing and capacity
- Loading states with skeleton cards
- Booking status management and validation
- Error handling with detailed messages
- Success confirmation with visual feedback
- Instructor and location information display
- Live stream event support

## Configuration Props

### Event Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `eventId` | `Number` | `null` | Event ID for loading event data |
| `eventObject` | `Object` | `null` | Pre-loaded event object |

### Display Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showImage` | `Boolean` | `true` | Display instructor photo |
| `showEventDate` | `Boolean` | `false` | Show event date in timing |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `titleTag` | HTML tag for success title element |
| `title` | Custom success title override |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatCurrency` | Formats payment amounts |
| `formatDate` | Formats event dates and times |

## Computed Properties

### Event State
- `eventFullyLoaded` - Whether event data has fully loaded
- `booked` - Whether customer is successfully booked
- `status` - Current event status (upcoming, finished, etc.)
- `pieChartStyle` - CSS for occupancy pie chart

### Booking Validation
- `validCreditsCount` - Available credits for booking
- `validSubscriptionsCount` - Valid subscriptions count
- `isAvailableToBook` - Whether event can be booked
- `selectOptions` - Payment method options array

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `booking-created` | `{ event, booking }` | Emitted when booking is successfully created |
| `waitlist-confirmed` | `{ event }` | Emitted when waitlist confirmation is processed |

## Slots

### Image Slot
| Slot | Props | Description |
|------|-------|-------------|
| `image` | `{ event }` | Custom instructor image display |

### Header Slot
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ event }` | Custom header content with event details |

### Content Slot
| Slot | Props | Description |
|------|-------|-------------|
| `content` | `{ event }` | Main content area with booking interface |

### Footer Slot
| Slot | Props | Description |
|------|-------|-------------|
| `footer` | `{ event }` | Footer with payment selection and booking button |

### Error Messages Slot
| Slot | Props | Description |
|------|-------|-------------|
| `error-messages` | `{ genericErrors, error }` | Custom error message display |

## Loading States

The component provides comprehensive loading states:
- Skeleton card during event data loading
- Loading states for booking operations
- Payment method selection loading
- Success state transitions

## Payment Integration

The component integrates with the payment system through:
- Credit and subscription validation
- Payment method selection dropdown
- Multiple booking limit support
- Purchase requirement handling

## Examples

### Basic Waitlist Confirmation
```vue
<template>
  <div class="waitlist-booking">
    <codex-waitlist-confirmation 
      :event-id="waitlistEventId"
      :show-image="true"
    />
  </div>
</template>

<script setup>
const waitlistEventId = ref(456)
</script>
```

### Custom Success Message
```vue
<template>
  <div class="custom-waitlist-confirmation">
    <codex-waitlist-confirmation 
      :event-object="selectedEvent"
      :title="'Great News!'"
    >
      <template #content="{ event }">
        <div v-if="booked" class="custom-success">
          <div class="success-animation">
            <i class="ri-checkbox-circle-fill"></i>
          </div>
          
          <h3>{{ title || 'You\'re Booked In!' }}</h3>
          
          <div class="booking-details">
            <p>A space opened up and we've secured it for you!</p>
            <div class="event-summary">
              <h4>{{ event.event_type?.name }}</h4>
              <p>{{ formatDateTime(event.start_at) }}</p>
              <p>{{ event.studio?.name }}</p>
            </div>
          </div>
          
          <div class="next-steps">
            <h4>What's Next?</h4>
            <ul>
              <li>Check your email for confirmation details</li>
              <li>Arrive 10 minutes early</li>
              <li>Bring water and a towel</li>
            </ul>
          </div>
        </div>
        
        <div v-else-if="event?.is_fully_booked" class="space-taken">
          <div class="disappointment-icon">
            <i class="ri-emotion-sad-line"></i>
          </div>
          <h3>Sorry, Someone Took Your Space</h3>
          <p>The space that opened up has been taken by another customer.</p>
          <p>You'll remain on the waitlist for future openings.</p>
        </div>
        
        <div v-else class="booking-interface">
          <!-- Custom booking interface -->
          <div class="event-info">
            <h3>{{ event.event_type?.name }}</h3>
            <div class="event-meta">
              <div class="timing">
                <i class="ri-time-line"></i>
                {{ formatTime(event.start_at) }} • {{ event.duration }} mins
              </div>
              <div class="capacity">
                <i class="ri-group-line"></i>
                {{ event.occupancy }}/{{ event.capacity }} spaces
              </div>
            </div>
          </div>
          
          <div class="booking-requirements">
            <h4>Booking Requirements</h4>
            <div class="credits-required">
              <strong>Credits Required:</strong> {{ event.required_credits }}
            </div>
            <div class="credits-available">
              <strong>You Have:</strong> {{ validCreditsCount }} credits available
            </div>
          </div>
        </div>
      </template>
      
      <template #footer="{ event }">
        <div class="custom-footer">
          <div v-if="!customer" class="login-required">
            <button class="login-btn" data-codex-modal-toggle="codex-login">
              Login to Complete Booking
            </button>
          </div>
          
          <div v-else-if="needsCredits" class="purchase-credits">
            <button @click="goToPurchase" class="purchase-btn">
              Buy Credits ({{ event.required_credits }} needed)
            </button>
          </div>
          
          <div v-else class="booking-actions">
            <div class="payment-selection">
              <label>Pay With:</label>
              <select v-model="selectedPayment" class="payment-select">
                <option value="">Select payment method</option>
                <option v-for="option in paymentOptions" :key="option.value" :value="option.value">
                  {{ option.label }}
                </option>
              </select>
            </div>
            
            <button 
              @click="confirmBooking"
              :disabled="!selectedPayment || processing"
              class="confirm-btn"
            >
              <span v-if="processing">
                <i class="ri-loader-4-line spinning"></i>
                Confirming Booking...
              </span>
              <span v-else>
                Yes, I'd Like This Space!
              </span>
            </button>
          </div>
        </div>
      </template>
    </codex-waitlist-confirmation>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const selectedEvent = ref({
  id: 789,
  event_type: { name: 'Yoga Flow' },
  start_at: '2024-01-15T09:00:00',
  duration: 60,
  required_credits: 1,
  occupancy: 11,
  capacity: 12,
  studio: { name: 'Main Studio' },
  instructor: { 
    first_name: 'Sarah', 
    last_name: 'Johnson',
    photo: '/instructor-photos/sarah.jpg'
  }
})

const selectedPayment = ref('')
const processing = ref(false)

const paymentOptions = computed(() => [
  { value: 'credits', label: '1 Class Credit' },
  { value: 'subscription', label: 'Monthly Membership' },
  { value: 'package', label: '10-Class Package' }
])

const formatDateTime = (dateTime) => {
  return new Date(dateTime).toLocaleString()
}

const formatTime = (dateTime) => {
  return new Date(dateTime).toLocaleTimeString([], { 
    hour: '2-digit', 
    minute: '2-digit' 
  })
}

const confirmBooking = async () => {
  processing.value = true
  try {
    // Implementation would handle booking confirmation
    await new Promise(resolve => setTimeout(resolve, 2000))
    console.log('Booking confirmed with:', selectedPayment.value)
  } finally {
    processing.value = false
  }
}

const goToPurchase = () => {
  console.log('Redirecting to purchase credits')
}
</script>

<style scoped>
.custom-success {
  text-align: center;
  padding: 2rem;
}

.success-animation {
  font-size: 3rem;
  color: #28a745;
  margin-bottom: 1rem;
}

.booking-details {
  margin: 2rem 0;
}

.event-summary {
  background: #f8f9fa;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
}

.next-steps ul {
  text-align: left;
  max-width: 300px;
  margin: 0 auto;
}

.space-taken {
  text-align: center;
  padding: 2rem;
}

.disappointment-icon {
  font-size: 3rem;
  color: #ffc107;
  margin-bottom: 1rem;
}

.booking-interface {
  padding: 1.5rem;
}

.event-meta {
  display: flex;
  gap: 1rem;
  margin: 1rem 0;
}

.custom-footer {
  padding: 1rem;
}

.payment-selection {
  margin-bottom: 1rem;
}

.payment-select {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.confirm-btn {
  width: 100%;
  padding: 1rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.confirm-btn:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.spinning {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-card` | Main container styling |
| `_c-event` | Event-specific styling |
| `_c-waitlist-confirmation` | Component-specific styling |
| `_c-header` | Header section styling |
| `_c-content` | Content area styling |
| `_c-footer` | Footer section styling |
| `_c-event-photo` | Instructor photo styling |
| `_c-event-timing` | Event timing display |
| `_c-pie-container` | Capacity pie chart container |
| `_c-pie` | Pie chart styling |
| `_c-event-type` | Event type display |
| `_c-event-details` | Event details section |
| `_c-success-container` | Success state container |
| `_c-success-icon` | Success icon styling |
| `_c-success-title` | Success title styling |
| `_c-success-desc` | Success description styling |
| `_c-btn-container` | Button container styling |

## Internationalization

The component uses the following translation keys:

### Core Waitlist Confirmation
| Key | Usage |
|-----|-------|
| `waitlist.title` | Default component title when title prop is not provided |
| `waitlist.youre_booked_in` | Success confirmation title |
| `waitlist.waitlist_booked_in_msg` | Success confirmation message |
| `waitlist.space_available` | Space availability notification |
| `waitlist.confirm_booking` | Booking confirmation instruction |

### Event Information Display
| Key | Usage |
|-----|-------|
| `event.title` | Event title display |
| `event.date` | Event date label |
| `event.time` | Event time label |
| `event.duration` | Event duration label |
| `time.mins` | Minutes label for duration |
| `event.location` | Event location label |
| `event.studio` | Event studio label |
| `timetable.instructor` | Instructor label |

### Space and Booking Status
| Key | Usage |
|-----|-------|
| `waitlist.space_secured` | Space secured message |
| `waitlist.limited_time_offer` | Time-limited offer notice |
| `waitlist.expires_in` | Offer expiry countdown |
| `waitlist.sorry_someone_took_your_space` | Space unavailable message |
| `waitlist.space_no_longer_available` | Alternative unavailability message |
| `waitlist.try_again_later` | Retry instruction |

### Payment and Credit Information
| Key | Usage |
|-----|-------|
| `event.credit_required` | Credit requirement label |
| `event.credits_needed` | Credits needed display |
| `event.you_have` | "You have" prefix for credits |
| `event.pay_with` | Payment method selection label |
| `event.choose_payment_method` | Payment method instruction |
| `event.insufficient_credits` | Insufficient credits warning |

### Action Buttons and States
| Key | Usage |
|-----|-------|
| `waitlist.yes_id_like_this_space` | Primary confirmation button |
| `waitlist.confirm_booking` | Alternative confirmation button |
| `waitlist.decline_space` | Decline space button |
| `button.booking` | Processing state button text |
| `button.confirming` | Confirming state button text |
| `button.processing` | Generic processing state |
| `button.login` | Login required button |

### Success and Completion States
| Key | Usage |
|-----|-------|
| `waitlist.booking_confirmed` | Booking confirmation title |
| `waitlist.see_you_there` | Friendly confirmation message |
| `waitlist.booking_details_sent` | Email confirmation notice |
| `waitlist.manage_booking` | Manage booking action |
| `waitlist.add_to_calendar` | Calendar integration action |

### Error and Alternative Flows
| Key | Usage |
|-----|-------|
| `waitlist.booking_failed` | Booking failure message |
| `waitlist.please_try_again` | Retry instruction |
| `waitlist.contact_support` | Support contact message |
| `error.something_went_wrong` | Generic error message |
| `error.network_error` | Network connectivity error |

### Time and Urgency Messaging
| Key | Usage |
|-----|-------|
| `waitlist.hurry_limited_time` | Urgency messaging |
| `waitlist.offer_expires_soon` | Expiry warning |
| `waitlist.minutes_remaining` | Countdown display |
| `waitlist.seconds_remaining` | Final countdown display |

### Alternative Actions
| Key | Usage |
|-----|-------|
| `waitlist.stay_on_waitlist` | Remain on waitlist option |
| `waitlist.leave_waitlist` | Leave waitlist option |
| `waitlist.find_alternative` | Find alternative events |
| `button.purchase_credits` | Purchase credits action |
| `button.back_to_timetable` | Return to timetable |

### Translation Usage Examples
```vue
<!-- Success state -->
<div v-if="bookingConfirmed" class="success-state">
  <h2>{{ $t('waitlist.youre_booked_in') }}</h2>
  <p>{{ $t('waitlist.waitlist_booked_in_msg') }}</p>
  <p>{{ $t('waitlist.booking_details_sent') }}</p>
</div>

<!-- Space availability with countdown -->
<div v-if="spaceAvailable" class="confirmation-state">
  <h3>{{ $t('waitlist.space_available') }}</h3>
  <div class="countdown">
    {{ $t('waitlist.expires_in') }} {{ timeRemaining }} {{ $t('waitlist.minutes_remaining') }}
  </div>
</div>

<!-- Payment method selection -->
<div v-if="requiresPayment" class="payment-selection">
  <label>{{ $t('event.pay_with') }}</label>
  <select v-model="selectedPaymentMethod">
    <option value="">{{ $t('event.choose_payment_method') }}</option>
    <option v-for="method in paymentMethods" :key="method.id" :value="method.id">
      {{ method.label }}
    </option>
  </select>
</div>

<!-- Confirmation button with states -->
<button 
  @click="confirmBooking"
  :disabled="processing || !canConfirm"
  class="confirm-btn"
>
  <template v-if="processing">
    {{ $t('button.confirming') }}
  </template>
  <template v-else>
    {{ $t('waitlist.yes_id_like_this_space') }}
  </template>
</button>

<!-- Credit requirements -->
<div v-if="event.required_credits" class="credit-info">
  {{ $t('event.credit_required') }} {{ event.required_credits }}
  <div v-if="insufficientCredits" class="warning">
    {{ $t('event.insufficient_credits') }}
  </div>
</div>

<!-- Space unavailable state -->
<div v-if="spaceUnavailable" class="unavailable-state">
  <h3>{{ $t('waitlist.sorry_someone_took_your_space') }}</h3>
  <p>{{ $t('waitlist.try_again_later') }}</p>
  <button @click="stayOnWaitlist">
    {{ $t('waitlist.stay_on_waitlist') }}
  </button>
</div>
```

## Best Practices

### Waitlist Management
- Handle waitlist confirmations promptly
- Provide clear time limits for responses
- Show comprehensive event details
- Validate payment methods before confirmation
- Handle concurrent booking scenarios gracefully

### User Experience
- Use skeleton loading during data fetching
- Provide immediate feedback for all actions
- Show clear success and error states
- Handle edge cases like expired confirmations
- Implement proper timeout handling

### Payment Integration
- Validate credits and subscriptions accurately
- Support multiple payment methods
- Handle payment failures gracefully
- Provide clear purchase paths for insufficient credits
- Implement secure payment processing

### Performance
- Load event data efficiently
- Cache payment method options
- Optimize state transitions
- Handle concurrent user actions
- Implement proper error recovery

### Accessibility
- Ensure proper heading hierarchy
- Provide descriptive labels for all inputs
- Support keyboard navigation
- Use appropriate ARIA attributes
- Test with screen readers

### Error Handling
- Display meaningful error messages
- Handle network failures gracefully
- Provide retry mechanisms
- Log errors for debugging
- Implement fallback states

## Component Registration
```javascript
// Global registration
app.component('CodexWaitlistConfirmation', WaitlistConfirmation)

// Local registration  
import WaitlistConfirmation from '@/components/bookings/WaitlistConfirmation.vue'

export default {
  components: {
    CodexWaitlistConfirmation: WaitlistConfirmation
  }
}
``` 