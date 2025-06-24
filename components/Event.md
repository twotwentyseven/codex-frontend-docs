# Event Component

## Overview
The Event component displays comprehensive event details with booking functionality, including layout-based seating selection, credit management, and real-time availability tracking. It provides a complete booking interface with support for both simple events and complex studio layouts with individual seat selection. The component handles various event states and booking scenarios with comprehensive error handling and user feedback.

## Basic Usage
```vue
<template>
  <div class="event-details">
    <codex-event 
      :id="eventId"
      :show-image="true"
      :is-modal="false"
    />
  </div>
</template>

<script setup>
const eventId = 123
</script>
```

## Key Features
- Comprehensive event details display
- Layout-based seat selection for studios
- Credit and subscription validation
- Real-time booking capacity tracking
- Multi-slot booking support
- Booking cancellation with timing restrictions
- Loading states with skeleton cards
- Modal and page view modes
- Live stream event support
- Instructor substitution indicators

## Configuration Props

### Event Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `id` | `Number` | `null` | Event ID for loading event data |
| `eventObject` | `Object` | `null` | Pre-loaded event object |

### Display Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showImage` | `Boolean` | `true` | Display instructor photo |
| `showEventDate` | `Boolean` | `false` | Show event date in timing |
| `isModal` | `Boolean` | `false` | Whether component is in modal mode |

### URL Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `purchaseUrl` | `String` | `''` | URL for purchasing credits |
| `timetableUrl` | `String` | `''` | URL for returning to timetable |
| `eventUrl` | `String` | `''` | URL for event page |
| `accountUrl` | `String` | `''` | URL for account area |
| `videoUrl` | `String` | `''` | URL for live video |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `titleTag` | HTML tag for titles |
| `enableBorder` | Enables border styling |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatCurrency` | Formats credit costs |
| `formatDate` | Formats event dates and times |

## Computed Properties

### Event State
- `eventFullyLoaded` - Whether event data has fully loaded
- `status` - Current event status (upcoming, finished, available)
- `pieChartStyle` - CSS for capacity visualization
- `hasLayout` - Whether event has studio layout
- `editMode` - Whether in booking edit mode

### Booking Validation
- `validCreditsCount` - Available credits for booking
- `validSubscriptionsCount` - Valid subscriptions count
- `totalSlotsAvailableToCustomer` - Available booking slots
- `maxBookableSlots` - Maximum slots customer can book
- `bookableObjects` - Available seats/spots in layout

### Layout Management
- `slotWrapperSize` - CSS sizing for layout container
- `cancellableSlots` - Slots that can be cancelled
- `selectedSlots` - Currently selected slots for booking

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `booking-created` | `{ event, booking }` | Emitted when booking is successfully created |
| `booking-cancelled` | `{ event, booking }` | Emitted when booking is cancelled |
| `event-updated` | `{ event }` | Emitted when event data is updated |

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
| `footer` | `{ event }` | Footer with booking actions and payment selection |

### Error Messages Slot
| Slot | Props | Description |
|------|-------|-------------|
| `error-messages` | `{ genericErrors, error }` | Custom error message display |

## Loading States

The component provides comprehensive loading states:
- Skeleton card during event data loading
- Loading states for booking operations
- Layout loading for studio seating
- Payment processing states

## Layout Integration

For events with studio layouts, the component provides:
- Visual seat/spot selection interface
- Real-time availability updates
- Multi-slot booking capabilities
- Booking timer for cancellation deadlines
- Interactive layout with slot labels

## Examples

### Basic Event Display
```vue
<template>
  <div class="simple-event">
    <codex-event 
      :id="simpleEventId"
      :show-image="true"
    />
  </div>
</template>

<script setup>
const simpleEventId = ref(456)
</script>
```

### Event with Custom Layout
```vue
<template>
  <div class="layout-event">
    <codex-event 
      :event-object="layoutEvent"
      :is-modal="true"
    >
      <template #header="{ event }">
        <div class="custom-event-header">
          <div class="event-title-section">
            <h2>{{ event.event_type?.name }}</h2>
            <div class="event-badges">
              <span v-if="event.is_live_stream" class="live-badge">
                <i class="ri-live-line"></i>
                Live Stream
              </span>
              <span v-if="event.featured" class="featured-badge">
                <i class="ri-star-fill"></i>
                Featured
              </span>
            </div>
          </div>
          
          <div class="event-timing-enhanced">
            <div class="time-info">
              <i class="ri-time-line"></i>
              <span>{{ formatEventTime(event.start_at) }}</span>
              <span class="duration">{{ event.duration }} mins</span>
            </div>
            
            <div class="capacity-info">
              <div class="capacity-visual">
                <div class="capacity-bar">
                  <div 
                    class="capacity-fill" 
                    :style="{ width: getCapacityPercentage(event) + '%' }"
                  ></div>
                </div>
                <span class="capacity-text">
                  {{ event.occupancy }}/{{ event.capacity }} booked
                </span>
              </div>
            </div>
          </div>
          
          <div class="instructor-info-enhanced">
            <div class="instructor-avatar">
              <img :src="event.instructor?.photo" :alt="event.instructor?.full_name">
            </div>
            <div class="instructor-details">
              <h4>{{ event.instructor?.first_name }} {{ event.instructor?.last_name }}</h4>
              <p class="instructor-bio">{{ event.instructor?.bio || 'Experienced instructor' }}</p>
              
              <div v-if="event.metafields?.original_instructor_name" class="substitute-notice">
                <i class="ri-swap-line"></i>
                <span>Substitute for {{ event.metafields.original_instructor_name }}</span>
              </div>
            </div>
          </div>
        </div>
      </template>
      
      <template #content="{ event }">
        <div class="enhanced-event-content">
          <!-- Location and Studio Info -->
          <div class="location-section">
            <h3>Location Details</h3>
            <div class="location-info">
              <div class="info-item">
                <i class="ri-map-pin-line"></i>
                <div>
                  <strong>{{ event.studio?.location?.name }}</strong>
                  <p>{{ event.studio?.location?.address }}</p>
                </div>
              </div>
              
              <div class="info-item">
                <i class="ri-building-line"></i>
                <div>
                  <strong>{{ event.studio?.name }}</strong>
                  <p>Studio {{ event.studio?.number || 'Main' }}</p>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Event Description -->
          <div v-if="event.description || event.event_type?.description" class="description-section">
            <h3>About This Class</h3>
            <div 
              class="description-content" 
              v-html="event.description || event.event_type?.description"
            ></div>
          </div>
          
          <!-- Booking Requirements -->
          <div v-if="event.required_credits" class="booking-requirements">
            <h3>Booking Requirements</h3>
            <div class="requirements-grid">
              <div class="requirement-item">
                <i class="ri-money-dollar-circle-line"></i>
                <div>
                  <strong>Credits Required</strong>
                  <p>{{ event.required_credits }} credit{{ event.required_credits !== 1 ? 's' : '' }}</p>
                </div>
              </div>
              
              <div class="requirement-item">
                <i class="ri-time-line"></i>
                <div>
                  <strong>Cancellation Policy</strong>
                  <p>Cancel up to 2 hours before class</p>
                </div>
              </div>
              
              <div class="requirement-item">
                <i class="ri-user-line"></i>
                <div>
                  <strong>What to Bring</strong>
                  <p>Water bottle, towel, comfortable clothes</p>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Seat Selection for Layout Events -->
          <div v-if="hasLayout" class="seat-selection-section">
            <h3>Select Your Spot</h3>
            <div class="layout-instructions">
              <p>Click on available spots to select them for booking.</p>
              <div class="layout-legend">
                <div class="legend-item">
                  <div class="legend-spot available"></div>
                  <span>Available</span>
                </div>
                <div class="legend-item">
                  <div class="legend-spot selected"></div>
                  <span>Selected</span>
                </div>
                <div class="legend-item">
                  <div class="legend-spot booked"></div>
                  <span>Your Booking</span>
                </div>
                <div class="legend-item">
                  <div class="legend-spot taken"></div>
                  <span>Taken</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
      
      <template #footer="{ event }">
        <div class="enhanced-footer">
          <div v-if="!customer" class="login-section">
            <div class="login-message">
              <h4>Ready to Book?</h4>
              <p>Log in to your account to book this class.</p>
            </div>
            <button class="login-btn" data-codex-modal-toggle="codex-login">
              <i class="ri-login-circle-line"></i>
              Log In to Book
            </button>
          </div>
          
          <div v-else class="booking-section">
            <div class="booking-summary">
              <div class="summary-item">
                <span>Selected Spots:</span>
                <strong>{{ selectedSlots.length }}</strong>
              </div>
              <div class="summary-item">
                <span>Credits Required:</span>
                <strong>{{ event.required_credits * selectedSlots.length }}</strong>
              </div>
              <div class="summary-item">
                <span>Available Credits:</span>
                <strong>{{ validCreditsCount }}</strong>
              </div>
            </div>
            
            <div class="booking-actions">
              <button 
                v-if="needsCredits"
                @click="goToPurchase"
                class="purchase-credits-btn"
              >
                <i class="ri-shopping-cart-line"></i>
                Buy Credits
              </button>
              
              <button 
                v-else
                @click="confirmBooking"
                :disabled="!canCompleteBooking"
                class="book-now-btn"
              >
                <i class="ri-checkbox-circle-line"></i>
                Book Selected Spots
              </button>
            </div>
          </div>
        </div>
      </template>
    </codex-event>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const layoutEvent = ref({
  id: 789,
  event_type: { name: 'Hot Yoga Flow', description: 'A dynamic flowing sequence...' },
  start_at: '2024-01-15T10:00:00',
  duration: 75,
  required_credits: 1,
  occupancy: 8,
  capacity: 20,
  studio: { 
    name: 'Studio A',
    number: '1',
    location: { 
      name: 'Downtown Location',
      address: '123 Main St, City'
    },
    layout: {
      slots: [
        // Layout data would be provided by the API
      ]
    }
  },
  instructor: { 
    first_name: 'Sarah', 
    last_name: 'Johnson',
    photo: '/instructor-photos/sarah.jpg',
    bio: 'Certified yoga instructor with 10+ years experience'
  },
  is_live_stream: false,
  featured: true
})

const formatEventTime = (dateTime) => {
  return new Date(dateTime).toLocaleTimeString('en', {
    hour: 'numeric',
    minute: '2-digit',
    hour12: true
  })
}

const getCapacityPercentage = (event) => {
  return Math.round((event.occupancy / event.capacity) * 100)
}

const goToPurchase = () => {
  console.log('Redirecting to purchase credits')
}

const confirmBooking = () => {
  console.log('Confirming booking for selected slots')
}
</script>

<style scoped>
.custom-event-header {
  padding: 1.5rem;
  border-bottom: 1px solid #eee;
}

.event-title-section {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1rem;
}

.event-badges {
  display: flex;
  gap: 0.5rem;
}

.live-badge,
.featured-badge {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  padding: 0.25rem 0.5rem;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: bold;
}

.live-badge {
  background: #dc3545;
  color: white;
}

.featured-badge {
  background: #ffc107;
  color: #212529;
}

.event-timing-enhanced {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.capacity-visual {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.capacity-bar {
  width: 100px;
  height: 8px;
  background: #e9ecef;
  border-radius: 4px;
  overflow: hidden;
}

.capacity-fill {
  height: 100%;
  background: linear-gradient(90deg, #28a745, #ffc107, #dc3545);
  transition: width 0.3s ease;
}

.instructor-info-enhanced {
  display: flex;
  gap: 1rem;
  align-items: flex-start;
}

.instructor-avatar img {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  object-fit: cover;
}

.substitute-notice {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  color: #856404;
  background: #fff3cd;
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
  font-size: 0.875rem;
  margin-top: 0.5rem;
}

.enhanced-event-content {
  padding: 1.5rem;
}

.location-section,
.description-section,
.booking-requirements,
.seat-selection-section {
  margin-bottom: 2rem;
}

.requirements-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
}

.requirement-item {
  display: flex;
  gap: 0.75rem;
  align-items: flex-start;
}

.layout-legend {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.legend-spot {
  width: 20px;
  height: 20px;
  border-radius: 4px;
  border: 2px solid #ddd;
}

.legend-spot.available {
  background: #fff;
}

.legend-spot.selected {
  background: #007bff;
  border-color: #007bff;
}

.legend-spot.booked {
  background: #28a745;
  border-color: #28a745;
}

.legend-spot.taken {
  background: #6c757d;
  border-color: #6c757d;
}

.enhanced-footer {
  padding: 1.5rem;
  border-top: 1px solid #eee;
}

.booking-summary {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 6px;
}

.booking-actions {
  display: flex;
  gap: 1rem;
}

.login-btn,
.purchase-credits-btn,
.book-now-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 6px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.2s;
}

.login-btn {
  background: #007bff;
  color: white;
}

.purchase-credits-btn {
  background: #ffc107;
  color: #212529;
}

.book-now-btn {
  background: #28a745;
  color: white;
  flex: 1;
}

.book-now-btn:disabled {
  background: #6c757d;
  cursor: not-allowed;
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-card` | Main container styling |
| `_c-event` | Event-specific styling |
| `_c-event-modal` | Modal-specific styling |
| `_c-header` | Header section styling |
| `_c-content` | Content area styling |
| `_c-footer` | Footer section styling |
| `_c-event-photo` | Instructor photo styling |
| `_c-event-timing` | Event timing display |
| `_c-pie-container` | Capacity pie chart container |
| `_c-event-type` | Event type display |
| `_c-event-details` | Event details section |
| `_c-event-layout-container` | Layout container styling |
| `_c-event-layout` | Layout grid styling |
| `_c-slot` | Individual slot styling |
| `_c-slot-selected` | Selected slot styling |
| `_c-slot-booked` | User's booked slot styling |
| `_c-slot-reserved` | Reserved/taken slot styling |
| `_c-slot-instructor` | Instructor position styling |
| `_c-slot-unavailable` | Unavailable slot styling |
| `_c-slot-key` | Layout legend styling |

## Internationalization

The component uses the following translation keys:

### Core Event Information
| Key | Usage |
|-----|-------|
| `event.success_title` | Success state title after booking |
| `event.success_msg` | Success message after booking |
| `time.mins` | Minutes label for duration |
| `event.full` | Full occupancy indicator |
| `event.spaces` | Spaces label for occupancy |
| `timetable.instructor` | Instructor label |
| `event.substitute_for` | Substitute instructor prefix (commented) |
| `event.location` | Location label |
| `event.studio` | Studio label |
| `event.is_live_stream` | Live stream indicator |

### Credit and Booking Requirements
| Key | Usage |
|-----|-------|
| `event.credit_required` | Credit requirement label |
| `event.you_have` | "You have" prefix for credits/bookings |
| `booking.credits` | Credits label (with count parameter) |
| `event.available_that_can_be_used_for_this_event` | Credit availability message |
| `booking.you_have` | "You have" prefix for booking counts |
| `booking.bookings` | Bookings label (with count parameter) |
| `booking.maximum` | "Maximum" label for booking limits |
| `event.you_have_reached_the_max_bookable_spaces` | Maximum booking limit message |

### Layout and Seat Selection
| Key | Usage |
|-----|-------|
| `event.space_name` | Space/slot name label |
| `event.you_are_booked_in_for` | Current booking confirmation |
| `booking.spaces` | Spaces label (with count parameter) |
| `event.if_you_want_to_change_layout` | Layout change instruction (with layout) |
| `event.if_you_want_to_change_no_layout` | Layout change instruction (no layout) |
| `event.instructor` | Layout legend - instructor position |
| `event.taken` | Layout legend - occupied spots |
| `event.available` | Layout legend - available spots |
| `event.selected` | Layout legend - selected spots |
| `event.your_bookings` | Layout legend - user's bookings |
| `event.unavailable` | Layout legend - unavailable spots |

### Event Status and Messages
| Key | Usage |
|-----|-------|
| `event.event_finished_title` | Event finished state title |
| `event.this_event_has_finished` | Event finished message |
| `event.event_not_available_to_book_yet_title` | Future event title |
| `event.this_event_not_available_to_book_yet` | Future event message |

### Payment and Actions
| Key | Usage |
|-----|-------|
| `event.pay_with` | Payment method selection label |
| `event.pay_with_helper` | Payment method helper text |
| `event.pay_with_tooltip` | Payment method tooltip |
| `button.login` | Login button text |
| `button.view_live_event` | View live event button |
| `button.account_area` | Account area link text |
| `event.back_to_timetable` | Back to timetable button |

### Booking Management
| Key | Usage |
|-----|-------|
| `booking.bookings_can_be_managed_in_the` | Booking management instruction |
| `waitlist.waitlist_title` | Waitlist section title |
| `waitlist.event_is_fully_booked_message` | Waitlist message for full events |

### Translation Usage Examples
```vue
<!-- Event success state -->
<div v-if="showSuccess">
  <codex-title :content="title || $t('event.success_title')" />
  <codex-paragraph :content="$t('event.success_msg')" />
</div>

<!-- Event timing display -->
<div class="event-timing">
  {{ event.duration }} {{ $t('time.mins') }}
</div>

<!-- Occupancy status -->
<div class="occupancy">
  <template v-if="event.occupancy === event.capacity">
    {{ $t('event.full') }}
  </template>
  <template v-else>
    {{ event.occupancy }}/{{ event.capacity }} {{ $t('event.spaces') }}
  </template>
</div>

<!-- Credit requirements -->
<div v-if="event.required_credits">
  {{ $t('event.credit_required') }} {{ event.required_credits }}
</div>

<!-- Booking status -->
<div class="booking-status">
  {{ $t('booking.you_have') }} 
  <span>{{ customerSlots.length }} {{ $t('booking.bookings', customerSlots.length) }}</span>
  ({{ $t('booking.maximum') }} {{ maxBookableSlots }} {{ $t('booking.bookings', maxBookableSlots) }})
</div>
```

## Best Practices

### Event Display
- Show comprehensive event information clearly
- Handle various event types and configurations
- Provide clear visual hierarchy
- Display real-time availability and capacity
- Handle instructor substitutions appropriately

### Layout Management
- Provide intuitive seat/spot selection interface
- Show clear visual feedback for selections
- Handle multi-slot bookings efficiently
- Display booking restrictions clearly
- Implement proper validation for slot selection

### Booking Flow
- Validate credits and subscriptions accurately
- Provide clear booking confirmation
- Handle payment processing securely
- Show booking progress and feedback
- Implement proper error recovery

### User Experience
- Use skeleton loading during data fetching
- Provide immediate feedback for all actions
- Handle various screen sizes appropriately
- Support both modal and page modes
- Implement clear navigation patterns

### Accessibility
- Ensure proper heading hierarchy
- Provide descriptive labels for interactive elements
- Support keyboard navigation for layouts
- Use appropriate ARIA attributes
- Test with screen readers

### Performance
- Load event data efficiently
- Optimize layout rendering
- Handle real-time updates appropriately
- Cache frequently accessed data
- Implement proper state management

## Component Registration
```javascript
// Global registration
app.component('CodexEvent', Event)

// Local registration  
import Event from '@/components/bookings/Event.vue'

export default {
  components: {
    CodexEvent: Event
  }
}
``` 