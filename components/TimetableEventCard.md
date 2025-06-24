# TimetableEventCard Component

## Overview
The TimetableEventCard component displays individual event information in a compact card format suitable for timetables and event listings. It provides essential event details including timing, instructor, location, capacity, and booking status with integrated action buttons for booking, waitlist management, and bookmarking. The component supports various event states and user contexts with comprehensive accessibility features.

## Basic Usage
```vue
<template>
  <div class="event-card">
    <codex-timetable-event-card 
      :event="eventData"
      :customer="currentCustomer"
      :is-bookmarked="isBookmarked"
      :toggle-bookmark="toggleBookmark"
      :bookmark-identifier="bookmarkId"
      :go-to-event="handleEventClick"
      :cancel-booking="handleCancelBooking"
      :has-booking="userHasBooking"
    />
  </div>
</template>

<script setup>
const eventData = {
  id: 123,
  event_type: { name: 'Yoga Flow' },
  start_at: '2024-01-15T10:00:00',
  duration: 60,
  occupancy: 8,
  capacity: 12,
  instructor: { first_name: 'Sarah', last_name: 'Johnson' },
  studio: { name: 'Studio A', location: { name: 'Downtown' } }
}
</script>
```

## Key Features
- Compact event information display
- Real-time capacity and availability tracking
- Booking status indicators (booked, waitlist, available)
- Integrated booking and waitlist buttons
- Bookmark functionality with visual feedback
- Instructor substitution notifications
- Loading states with skeleton support
- Mobile-responsive design
- Accessibility-compliant interactions

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `event` | `Object` | `required` | Event data object |
| `customer` | `Object|Boolean|null` | `required` | Customer object or authentication state |
| `isBookmarked` | `Function` | `required` | Function to check bookmark status |
| `toggleBookmark` | `Function` | `required` | Function to toggle bookmark state |
| `bookmarkIdentifier` | `String` | `required` | Unique bookmark identifier |
| `goToEvent` | `Function` | `required` | Function to navigate to event details |
| `cancelBooking` | `Function` | `required` | Function to cancel booking |

### Display Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showEventDate` | `Boolean` | `false` | Show event date in timing display |
| `loading` | `Boolean` | `false` | Show skeleton loading state |

### Booking Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `hasBooking` | `Boolean` | `false` | Whether user has booking for this event |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Enables border styling on card |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatDate` | Formats event dates and times |
| `formatTime` | Formats time display |

## Computed Properties

### Event Display
- `displayEvent` - Processed event data for display
- `pieChartStyle` - CSS for capacity pie chart visualization
- `isCustomerBooked` - Whether the customer has a booking
- `isOnWaitlist` - Whether the customer is on the waitlist

### Refresh Functionality
- `reloadEventData` - Function to refresh event data
- `freshEventData` - Updated event data from API

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `booking-cancelled` | `{ event, booking }` | Emitted when booking is cancelled |
| `event-updated` | `{ event }` | Emitted when event data is refreshed |

## Slots

### Header Slot
| Slot | Props | Description |
|------|-------|-------------|
| `header` | N/A | Custom header content |

### Content Slot
| Slot | Props | Description |
|------|-------|-------------|
| `content` | N/A | Main content area with event details and actions |

### Footer Slot
| Slot | Props | Description |
|------|-------|-------------|
| `footer` | N/A | Footer content area |

## Loading States

The component provides loading states through:
- Skeleton card during initial loading
- Loading states for booking operations
- Refresh indicators for data updates

## Event State Management

The component handles various event states:
- Available for booking
- Fully booked with waitlist option
- Customer already booked
- Customer on waitlist
- Event not available/past

## Examples

### Basic Event Card
```vue
<template>
  <div class="simple-card">
    <codex-timetable-event-card 
      :event="basicEvent"
      :customer="user"
      :is-bookmarked="checkBookmark"
      :toggle-bookmark="handleBookmarkToggle"
      :bookmark-identifier="`event-${basicEvent.id}`"
      :go-to-event="openEventDetails"
      :cancel-booking="cancelEventBooking"
      :has-booking="false"
      :enable-border="true"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'

const basicEvent = ref({
  id: 456,
  event_type: { name: 'Power Yoga' },
  start_at: '2024-01-16T18:00:00',
  duration: 75,
  occupancy: 15,
  capacity: 20,
  instructor: { 
    first_name: 'Mike', 
    last_name: 'Chen',
    photo: '/instructors/mike-chen.jpg'
  },
  studio: { 
    name: 'Studio B',
    location: { name: 'Uptown Location' }
  },
  is_live_stream: false,
  status: 'upcoming'
})

const user = ref({
  id: 123,
  name: 'John Doe'
})

const checkBookmark = (type, identifier) => {
  // Implementation would check bookmarks
  return false
}

const handleBookmarkToggle = (type, identifier) => {
  console.log('Toggling bookmark for:', type, identifier)
}

const openEventDetails = (event, action = 'view') => {
  console.log('Opening event:', event.id, 'Action:', action)
}

const cancelEventBooking = (event) => {
  console.log('Cancelling booking for event:', event.id)
}
</script>
```

### Fully Booked Event with Waitlist
```vue
<template>
  <div class="waitlist-card">
    <codex-timetable-event-card 
      :event="fullEvent"
      :customer="customer"
      :is-bookmarked="isBookmarked"
      :toggle-bookmark="toggleBookmark"
      :bookmark-identifier="`event-${fullEvent.id}`"
      :go-to-event="goToEvent"
      :cancel-booking="cancelBooking"
      :has-booking="false"
      :show-event-date="true"
    />
  </div>
</template>

<script setup>
const fullEvent = ref({
  id: 789,
  event_type: { name: 'Hot Yoga' },
  start_at: '2024-01-17T12:00:00',
  duration: 90,
  occupancy: 16,
  capacity: 16,
  is_fully_booked: true,
  is_waitlistable: true,
  instructor: { 
    first_name: 'Lisa', 
    last_name: 'Wang',
    photo: '/instructors/lisa-wang.jpg'
  },
  studio: { 
    name: 'Hot Studio',
    location: { name: 'Central Location' }
  },
  metafields: {
    original_instructor_name: 'David Smith' // Substitute instructor
  }
})
</script>
```

### Booked Event Card
```vue
<template>
  <div class="booked-card">
    <codex-timetable-event-card 
      :event="bookedEvent"
      :customer="customer"
      :is-bookmarked="isBookmarked"
      :toggle-bookmark="toggleBookmark"
      :bookmark-identifier="`event-${bookedEvent.id}`"
      :go-to-event="goToEvent"
      :cancel-booking="cancelBooking"
      :has-booking="true"
      :show-event-date="true"
    />
  </div>
</template>

<script setup>
const bookedEvent = ref({
  id: 101112,
  event_type: { name: 'Pilates Reformer' },
  start_at: '2024-01-18T08:00:00',
  duration: 50,
  occupancy: 6,
  capacity: 8,
  instructor: { 
    first_name: 'Emma', 
    last_name: 'Rodriguez'
  },
  studio: { 
    name: 'Reformer Studio',
    location: { name: 'Westside Location' }
  }
})
</script>
```

### Custom Enhanced Event Card
```vue
<template>
  <div class="enhanced-card">
    <codex-timetable-event-card 
      :event="enhancedEvent"
      :customer="customer"
      :is-bookmarked="isBookmarked"
      :toggle-bookmark="toggleBookmark"
      :bookmark-identifier="`event-${enhancedEvent.id}`"
      :go-to-event="goToEvent"
      :cancel-booking="cancelBooking"
      :has-booking="hasBooking"
      :enable-border="true"
    >
      <template #content>
        <div class="custom-event-content">
          <!-- Timing and Capacity Section -->
          <div class="event-header-section">
            <div class="timing-info">
              <i class="ri-time-line"></i>
              <span class="event-time">
                {{ formatTime(enhancedEvent.start_at) }}
              </span>
              <span class="event-duration">
                {{ enhancedEvent.duration }}min
              </span>
            </div>
            
            <div class="capacity-indicator">
              <div v-if="!isCustomerBooked && !isOnWaitlist" class="capacity-display">
                <div class="capacity-circle" :class="getCapacityClass(enhancedEvent)">
                  <span class="capacity-text">
                    {{ enhancedEvent.capacity - enhancedEvent.occupancy }}
                  </span>
                </div>
                <span class="spaces-label">spaces left</span>
              </div>
              
              <div v-else-if="isCustomerBooked" class="booking-status booked">
                <i class="ri-checkbox-circle-fill"></i>
                <span>You're Booked</span>
              </div>
              
              <div v-else-if="isOnWaitlist" class="booking-status waitlist">
                <i class="ri-time-line"></i>
                <span>On Waitlist</span>
              </div>
            </div>
          </div>
          
          <!-- Event Type and Details -->
          <div class="event-details-section">
            <h3 class="event-title">{{ enhancedEvent.event_type?.name }}</h3>
            
            <div class="event-meta">
              <div class="instructor-info">
                <i class="ri-user-line"></i>
                <span>{{ enhancedEvent.instructor?.first_name }} {{ enhancedEvent.instructor?.last_name }}</span>
                
                <div v-if="enhancedEvent.metafields?.original_instructor_name" class="substitute-indicator">
                  <i class="ri-information-line" title="Substitute instructor"></i>
                </div>
              </div>
              
              <div class="location-info">
                <i class="ri-map-pin-line"></i>
                <span>{{ enhancedEvent.studio?.location?.name }}</span>
              </div>
              
              <div class="studio-info">
                <i class="ri-building-line"></i>
                <span>{{ enhancedEvent.studio?.name }}</span>
              </div>
              
              <div v-if="enhancedEvent.is_live_stream" class="livestream-info">
                <i class="ri-live-line"></i>
                <span>Live Stream Available</span>
              </div>
            </div>
          </div>
          
          <!-- Action Buttons -->
          <div class="event-actions-section">
            <div class="primary-actions">
              <!-- Waitlist Button for Full Events -->
              <codex-waitlist-button 
                v-if="customer && enhancedEvent.is_waitlistable && enhancedEvent.is_fully_booked"
                :event="enhancedEvent"
                :show-waitlist-message="false"
                @waitlist-joined="handleWaitlistJoined"
                @waitlist-left="handleWaitlistLeft"
              />
              
              <!-- Edit Booking Button -->
              <button
                v-else-if="customer && enhancedEvent.is_fully_booked && !enhancedEvent.is_waitlistable"
                @click="goToEvent(enhancedEvent, 'edit')"
                class="action-btn edit-btn"
              >
                <i class="ri-edit-line"></i>
                Edit Booking
              </button>
              
              <!-- Event Unavailable -->
              <button
                v-else-if="customer && enhancedEvent.status !== 'upcoming'"
                disabled
                class="action-btn unavailable-btn"
              >
                <i class="ri-close-circle-line"></i>
                Event Unavailable
              </button>
              
              <!-- Book Now Button -->
              <button
                v-else
                @click="goToEvent(enhancedEvent, 'book')"
                class="action-btn book-btn"
                :disabled="!customer"
              >
                <i class="ri-calendar-check-line"></i>
                {{ customer ? 'Book Now' : 'Login to Book' }}
              </button>
            </div>
            
            <!-- Bookmark Button -->
            <div class="secondary-actions">
              <codex-bookmark
                type="events"
                :item="bookmarkIdentifier"
                :class="{ 'bookmarked': isBookmarked('events', bookmarkIdentifier) }"
              />
            </div>
          </div>
        </div>
      </template>
    </codex-timetable-event-card>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const enhancedEvent = ref({
  id: 131415,
  event_type: { name: 'Vinyasa Flow' },
  start_at: '2024-01-19T19:00:00',
  duration: 60,
  occupancy: 14,
  capacity: 18,
  instructor: { 
    first_name: 'Alex', 
    last_name: 'Thompson'
  },
  studio: { 
    name: 'Flow Studio',
    location: { name: 'Downtown' }
  },
  is_live_stream: true,
  status: 'upcoming'
})

const isCustomerBooked = computed(() => hasBooking.value)
const isOnWaitlist = computed(() => {
  // Would check waitlist status
  return false
})

const formatTime = (dateTime) => {
  return new Date(dateTime).toLocaleTimeString('en', {
    hour: 'numeric',
    minute: '2-digit',
    hour12: true
  })
}

const getCapacityClass = (event) => {
  const available = event.capacity - event.occupancy
  const percentage = available / event.capacity
  
  if (percentage > 0.5) return 'high-availability'
  if (percentage > 0.2) return 'medium-availability'
  return 'low-availability'
}

const handleWaitlistJoined = (event) => {
  console.log('Joined waitlist for:', event.id)
}

const handleWaitlistLeft = (event) => {
  console.log('Left waitlist for:', event.id)
}
</script>

<style scoped>
.custom-event-content {
  padding: 1rem;
}

.event-header-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.timing-info {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
}

.event-time {
  font-weight: bold;
  color: #007bff;
}

.event-duration {
  color: #6c757d;
}

.capacity-display {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
}

.capacity-circle {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: 0.875rem;
  color: white;
}

.capacity-circle.high-availability {
  background: #28a745;
}

.capacity-circle.medium-availability {
  background: #ffc107;
  color: #212529;
}

.capacity-circle.low-availability {
  background: #dc3545;
}

.spaces-label {
  font-size: 0.75rem;
  color: #6c757d;
}

.booking-status {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  padding: 0.25rem 0.5rem;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: bold;
}

.booking-status.booked {
  background: #d4edda;
  color: #155724;
}

.booking-status.waitlist {
  background: #fff3cd;
  color: #856404;
}

.event-details-section {
  margin-bottom: 1rem;
}

.event-title {
  margin: 0 0 0.75rem 0;
  font-size: 1.125rem;
  font-weight: 600;
}

.event-meta {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  font-size: 0.875rem;
}

.instructor-info,
.location-info,
.studio-info,
.livestream-info {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: #6c757d;
}

.substitute-indicator {
  margin-left: 0.25rem;
  color: #ffc107;
}

.event-actions-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.action-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1rem;
  border: none;
  border-radius: 6px;
  font-size: 0.875rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s;
}

.book-btn {
  background: #28a745;
  color: white;
}

.book-btn:hover:not(:disabled) {
  background: #218838;
}

.book-btn:disabled {
  background: #6c757d;
  cursor: not-allowed;
}

.edit-btn {
  background: #17a2b8;
  color: white;
}

.edit-btn:hover {
  background: #138496;
}

.unavailable-btn {
  background: #6c757d;
  color: white;
  cursor: not-allowed;
}

.bookmarked {
  color: #ffc107;
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-card` | Main container styling |
| `_c-timetable-event-card` | Component-specific styling |
| `_c-border` | Border styling when enabled |
| `_c-header` | Header section styling |
| `_c-content` | Content area styling |
| `_c-footer` | Footer section styling |
| `_c-event-timing` | Event timing display |
| `_c-pie-container` | Capacity pie chart container |
| `_c-pie` | Pie chart styling |
| `_c-pie-sm` | Small pie chart variant |
| `_c-pie-info` | Pie chart text information |
| `_c-event-type` | Event type display |
| `_c-event-details` | Event details section |
| `_c-instructor` | Instructor information |
| `_c-instructor-name` | Instructor name styling |
| `_c-location` | Location information |
| `_c-studio` | Studio information |
| `_c-substitute` | Substitute instructor indicator |
| `_c-substitute-popup` | Substitute information popup |
| `_c-btn-container` | Button container styling |
| `_c-flag` | Status flag styling |
| `_c-success` | Success state styling |
| `_c-warning` | Warning state styling |

## Internationalization

The component uses the following translation keys:

### Core Event Information
| Key | Usage |
|-----|-------|
| `time.mins` | Minutes unit for event duration |
| `event.full` | Event at capacity indicator |
| `event.spaces` | Spaces available label |
| `event.location` | Location label |
| `event.studio` | Studio label |
| `event.is_live_stream` | Live stream indicator |
| `event.substitute_instructor` | Substitute instructor notice |

### Timetable-Specific Interface
| Key | Usage |
|-----|-------|
| `timetable.instructor` | Instructor label |
| `timetable.booked` | Customer booked status |
| `timetable.on_waitlist` | Customer waitlist status |
| `timetable.event_unavailable` | Event unavailable state |
| `timetable.edit_booking` | Edit booking button text |
| `timetable.view_your_booking` | View booking button aria-label |
| `timetable.book_event_now` | Book event button aria-label |

### Action Buttons
| Key | Usage |
|-----|-------|
| `button.book_now` | Book now button text |
| `button.processing` | Processing state text |
| `button.cancel_booking` | Cancel booking button text |
| `button.cancelling` | Cancelling state text |
| `button.edit_booking` | Edit booking button text |

### Translation Usage Examples
```vue
<!-- Event timing information -->
<div class="event-timing">
  <i class="ri-time-line"></i>
  <template v-if="showEventDate">
    {{ $filters.dateFormat(displayEvent.start_at, 'D/MM/YYYY') }} •
  </template>
  {{ $filters.dateFormat(displayEvent?.start_at, 'h:mm') }} •
  <template v-if="displayEvent?.duration">
    {{ displayEvent.duration }} {{ $t('time.mins') }}
  </template>
</div>

<!-- Capacity display -->
<div class="capacity-info">
  <template v-if="displayEvent.occupancy === displayEvent.capacity">
    {{ $t('event.full') }}
  </template>
  <template v-else>
    {{ displayEvent.occupancy }}/{{ displayEvent.capacity }} {{ $t('event.spaces') }}
  </template>
</div>

<!-- Booking status indicators -->
<div v-if="isCustomerBooked" class="status-booked">
  <i class="ri-checkbox-circle-fill"></i> {{ $t('timetable.booked') }}
</div>

<div v-else-if="isOnWaitlist" class="status-waitlist">
  <i class="ri-time-line"></i> {{ $t('timetable.on_waitlist') }}
</div>

<!-- Event details -->
<div class="event-details">
  <div v-if="displayEvent?.instructor" class="instructor">
    <span>{{ $t('timetable.instructor') }}</span>
    <span class="instructor-name">
      {{ displayEvent.instructor.first_name }} {{ displayEvent.instructor.last_name }}
    </span>
    
    <!-- Substitute instructor notice -->
    <template v-if="displayEvent.metafields?.original_instructor_name">
      <div class="substitute-notice">
        <i class="ri-information-line"></i>
        <div class="substitute-popup">
          <strong>{{ $t('event.substitute_instructor') }}</strong> 
          {{ displayEvent.metafields?.original_instructor_name }}
        </div>
      </div>
    </template>
  </div>

  <div v-if="displayEvent.studio?.location" class="location">
    <span>{{ $t('event.location') }}</span>
    <span>{{ displayEvent.studio.location.name }}</span>
  </div>
  
  <div v-if="displayEvent?.studio" class="studio">
    <span>{{ $t('event.studio') }}</span>
    <span>{{ displayEvent.studio.name }}</span>
  </div>

  <div v-if="displayEvent.is_live_stream" class="live-stream">
    <span>{{ $t('event.is_live_stream') }}</span>
    <span>{{ displayEvent.is_live_stream }}</span>
  </div>
</div>

<!-- Action buttons -->
<div class="event-actions">
  <!-- Waitlist button (handled by codex-waitlist-button component) -->
  <codex-waitlist-button 
    v-if="customer && displayEvent?.is_waitlistable && displayEvent?.is_fully_booked"
    :event="displayEvent"
  />
  
  <!-- Edit booking button -->
  <codex-anchor
    v-else-if="customer && displayEvent?.is_fully_booked"
    :href="window.codex.urls.account_url"
    :default-text="$t('timetable.edit_booking')"
    :aria-label="$t('timetable.edit_booking')"
  />
  
  <!-- Event unavailable button -->
  <codex-button 
    v-else-if="(customer && displayEvent?.status != 'upcoming') || disabled"
    :default-text="$t('timetable.event_unavailable')"
    :disabled-text="$t('timetable.event_unavailable')"
    disabled="true"
    :aria-label="$t('timetable.event_unavailable')"
  />

  <!-- Book now button -->
  <codex-button 
    v-else 
    :default-text="$t('button.book_now')"
    :processing-text="$t('button.processing')"
    :disabled-text="$t('button.book_now')"
    @click.stop="goToEvent(displayEvent, 'book')"
    :aria-label="$t('timetable.book_event_now')"
  />

  <!-- Bookmark button -->
  <codex-bookmark
    type="events"
    :item="bookmarkIdentifier"
  />
</div>
```

### Date and Time Formatting
The component uses global date formatting functions that respect locale settings:
- `$filters.dateFormat()` for date and time display
- Format strings can be configured globally: `window.codex.formats?.shortDate` and `window.codex.formats?.shortTime`
- Default formats: 'D/MM/YYYY' for dates, 'h:mm' for times

### Booking Status Integration
- The component integrates with `codex-waitlist-button` which has its own translation requirements
- Status indicators use consistent translation keys across the timetable system
- Booking actions maintain consistency with other booking-related components

## Best Practices

### Event Display
- Show essential event information clearly and concisely
- Use consistent visual hierarchy across all cards
- Provide clear indicators for different event states
- Handle instructor substitutions with appropriate notifications
- Display capacity and availability information prominently

### User Interaction
- Provide clear and accessible action buttons
- Use appropriate button states (enabled, disabled, loading)
- Handle different user contexts (logged in, logged out)
- Support touch interactions for mobile devices
- Implement proper keyboard navigation

### Booking Status
- Clearly indicate user's booking status
- Provide appropriate actions based on event state
- Handle waitlist functionality seamlessly
- Show relevant booking restrictions or requirements
- Display timing information prominently

### Performance
- Use skeleton loading during data fetching
- Optimize for rapid scrolling in lists/grids
- Cache frequently accessed data
- Handle real-time updates efficiently
- Minimize layout shifts during loading

### Accessibility
- Provide descriptive labels for all interactive elements
- Use appropriate ARIA attributes for dynamic content
- Support keyboard navigation for all actions
- Ensure adequate color contrast for text and indicators
- Test with screen readers for proper information flow

### Mobile Optimization
- Ensure touch targets are adequately sized
- Optimize information density for smaller screens
- Handle orientation changes gracefully
- Consider thumb-friendly button placement
- Test on various device sizes and resolutions

## Component Registration
```javascript
// Global registration
app.component('CodexTimetableEventCard', TimetableEventCard)

// Local registration  
import TimetableEventCard from '@/components/bookings/TimetableEventCard.vue'

export default {
  components: {
    CodexTimetableEventCard: TimetableEventCard
  }
}
``` 