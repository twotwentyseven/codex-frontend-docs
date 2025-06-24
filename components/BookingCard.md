# BookingCard Component

## Overview
The BookingCard component provides a detailed card interface for displaying individual booking entries with comprehensive event information, occupancy visualization, and action buttons. It features event timing, instructor details, location information, live stream support, Zoom integration, metrics display, and booking management functionality for effective booking display and interaction.

## Basic Usage
```vue
<codex-booking-card
  :booking="bookingData"
  :loading="false"
  :type="'upcoming'"
  @cancel="handleCancelBooking"
  @watch="handleWatchLiveStream"
/>
```

## Key Features
- Comprehensive event information display
- Event timing with duration and occupancy visualization
- Instructor details with substitution support
- Location and studio information
- Live stream indicators and access buttons
- Zoom integration with join functionality
- Event metrics display (cadence, watts, speed, etc.)
- Booking cancellation with confirmation
- Bookmark integration for events
- Loading skeleton support
- Responsive layout with desktop adaptations
- Different display modes (upcoming vs past bookings)

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| booking | Object\|Number | Yes | - | Booking data object or ID |
| loading | Boolean | No | false | Whether the card is in loading state |
| showInstructorPhoto | Boolean | No | true | Whether to show instructor photo |
| eventMetrics | Object\|Boolean | No | false | Event metrics configuration |
| enableBorder | Boolean | No | false | Whether to enable card border styling |
| type | String | No | 'upcoming' | Booking type ('upcoming' or 'past') |
| cancelling | Boolean | No | false | Whether booking is being cancelled |

### Common Props
All common props from `@/config/common` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| cancel | `booking: Object` | Emitted when cancel booking button is clicked |
| watch | `booking: Object` | Emitted when watch live stream button is clicked |
| join-zoom | `booking: Object` | Emitted when join Zoom call button is clicked |
| view-event | `booking.event: Object` | Emitted when event details should be viewed |

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
  <!-- Custom content and booking display -->
</template>
```

### Footer Slot
```vue
<template #footer="{ booking }">
  <!-- Custom footer content -->
</template>
```

## Booking Object Structure
The booking prop expects an object with the following structure:
```javascript
{
  id: String|Number,          // Unique booking identifier
  can_cancel: Boolean,        // Whether booking can be cancelled
  zoom_join_url: String,      // Zoom meeting URL (if applicable)
  credits_used: Array,        // Credits used for booking
  subscription_used: Object,  // Subscription used for booking
  slot: String|Number,        // Studio slot/position
  metafields: {               // Additional booking metadata
    stages_data: {            // Workout metrics
      avgCadence: Number,     // Average cadence
      maxCadence: Number,     // Maximum cadence
      avgWatt: Number,        // Average watts
      maxWatt: Number,        // Maximum watts
      avgSpeed: Number,       // Average speed
      distanceInKm: Number,   // Distance in kilometers
      kiloCalories: Number    // Calories burned
    }
  },
  event: {                    // Associated event information
    id: String|Number,        // Event ID
    start_at: Date,           // Event start time
    duration: Number,         // Event duration in minutes
    occupancy: Number,        // Current occupancy
    capacity: Number,         // Maximum capacity
    is_live_stream: Boolean,  // Whether event is live streamed
    event_type: {             // Event type information
      name: String            // Event type name
    },
    instructor: {             // Instructor information
      first_name: String,     // Instructor first name
      last_name: String       // Instructor last name
    },
    studio: {                 // Studio information
      name: String,           // Studio name
      has_layout: Boolean,    // Whether studio has layout
      location: {             // Location information
        name: String          // Location name
      },
      layout: {               // Studio layout (if applicable)
        slots: Array          // Available slots
      }
    },
    metafields: {             // Additional event metadata
      original_instructor_name: String  // Original instructor if substituted
    }
  }
}
```

## Display Modes
The component supports different display modes based on the `type` prop:

### Upcoming Bookings (`type="upcoming"`)
- Shows occupancy pie chart
- Displays action buttons (cancel, watch, join Zoom)
- Uses row layout on large screens
- Shows full event management options

### Past Bookings (`type="past"`)
- Hides occupancy information
- Limited action buttons
- Uses column layout
- Focuses on historical information

## Occupancy Visualization
For upcoming bookings, the component displays a pie chart showing:
- **Visual Indicator**: Circular progress chart
- **Occupancy Ratio**: Current vs maximum capacity
- **Full Status**: Special indicator when at capacity
- **Responsive Sizing**: Adapts to screen size

## Interactive Features
- **Cancellation**: Cancel booking with confirmation
- **Live Stream**: Watch live stream events
- **Zoom Integration**: Join Zoom calls for virtual events
- **Metrics Toggle**: Show/hide workout metrics
- **Bookmark**: Bookmark events for future reference
- **Event Viewing**: Navigate to event details

## Metrics Display
When available, the component can display workout metrics:
- **Cadence**: Average and maximum cadence
- **Power**: Average and maximum watts
- **Speed**: Average speed
- **Distance**: Total distance in kilometers
- **Calories**: Total calories burned
- **Toggle Control**: Show/hide metrics section

## Loading State
The component supports skeleton loading with:
- **Structured Layout**: Organized placeholder elements
- **Type-Specific**: Different layouts for upcoming vs past
- **Border Support**: Optional border styling for skeletons
- **Responsive**: Adapts to different screen sizes

## Internationalization
The component uses the following translation keys:

### Event Information Keys
- `time.mins`: Minutes label for duration
- `event.full`: Full occupancy indicator  
- `event.spaces`: Spaces label for occupancy
- `event.location`: Location label
- `event.studio`: Studio label
- `event.is_live_stream`: Live stream label
- `event.substitute_for`: Substitute instructor prefix (commented in code)
- `timetable.instructor`: Instructor label

### Action Button Keys
- `booking.watch_live_stream`: Watch live stream button
- `booking.join_zoom_call`: Join Zoom call button
- `booking.cancel`: Cancel booking button
- `booking.cancelling`: Cancelling process text
- `booking.view_metric`: View metrics button
- `booking.hide_metric`: Hide metrics button

### Metrics Display Keys
- `booking.avgCadence`: Average cadence label
- `booking.maxCadence`: Maximum cadence label
- `booking.avgWatt`: Average watts label
- `booking.maxWatt`: Maximum watts label
- `booking.avgSpeed`: Average speed label
- `booking.distanceInKm`: Distance label
- `booking.kiloCalories`: Calories label

### Additional Information Keys
- `credit.credits_used`: Credits used text (with count parameter)
- `booking.subscription_used`: Subscription used prefix

### Translation Usage Examples
```vue
<!-- Event timing display -->
<div class="event-timing">
  {{ booking.event.duration }} {{ $t('time.mins') }}
</div>

<!-- Occupancy display -->
<div class="occupancy">
  <template v-if="booking.event.occupancy === booking.event.capacity">
    {{ $t('event.full') }}
  </template>
  <template v-else>
    {{ booking.event.occupancy }}/{{ booking.event.capacity }} {{ $t('event.spaces') }}
  </template>
</div>

<!-- Action buttons with translations -->
<codex-button 
  :defaultText="$t('booking.cancel')"
  :processingText="$t('booking.cancelling')"
  @click="handleCancel"
/>

<!-- Metrics display -->
<div v-for="metric in metrics" :key="metric.label">
  <div class="metric-label">{{ $t(`booking.${metric.label}`) }}</div>
  <div class="metric-value">{{ metricValue }}</div>
</div>
```

## Examples

### Basic Implementation
```vue
<codex-booking-card
  :booking="booking"
  @cancel="handleCancel"
  @watch="watchLiveStream"
/>
```

### With Loading State
```vue
<codex-booking-card
  :booking="booking"
  :loading="isLoading"
  :enable-border="true"
  :type="'upcoming'"
/>
```

### Past Booking Display
```vue
<codex-booking-card
  :booking="pastBooking"
  :type="'past'"
  @view-event="viewEventDetails"
/>
```

### With Event Metrics
```vue
<codex-booking-card
  :booking="bookingWithMetrics"
  :event-metrics="true"
  @cancel="handleCancel"
/>
```

### Custom Header Content
```vue
<codex-booking-card
  :booking="booking"
  @cancel="handleCancel"
>
  <template #header>
    <div class="booking-header">
      <div class="booking-status" :class="getStatusClass(booking.status)">
        {{ booking.status }}
      </div>
      <div class="booking-date">
        Booked: {{ formatDate(booking.created_at) }}
      </div>
    </div>
  </template>
</codex-booking-card>
```

### Custom Footer with Additional Actions
```vue
<codex-booking-card
  :booking="booking"
  @cancel="handleCancel"
>
  <template #footer="{ booking }">
    <div class="booking-actions">
      <button @click="shareBooking(booking)">Share</button>
      <button @click="addToCalendar(booking.event)">Add to Calendar</button>
      <div class="credits-info" v-if="booking.credits_used.length">
        {{ booking.credits_used.length }} credits used
      </div>
    </div>
  </template>
</codex-booking-card>
```

### In Bookings List
```vue
<div class="bookings-container">
  <codex-booking-card
    v-for="booking in bookings"
    :key="booking.id"
    :booking="booking"
    :cancelling="cancellingIds.includes(booking.id)"
    :type="bookingType"
    @cancel="handleCancelBooking"
    @watch="handleWatchStream"
    @join-zoom="handleJoinZoom"
  />
</div>
```

### With Event Tracking
```vue
<codex-booking-card
  :booking="booking"
  @cancel="trackCancelBooking"
  @watch="trackWatchStream"
  @join-zoom="trackZoomJoin"
/>

<script setup>
const trackCancelBooking = (booking) => {
  analytics.track('booking_cancel_initiated', {
    booking_id: booking.id,
    event_id: booking.event.id,
    event_type: booking.event.event_type?.name
  })
  emit('cancel', booking)
}

const trackWatchStream = (booking) => {
  analytics.track('live_stream_accessed', {
    booking_id: booking.id,
    event_id: booking.event.id,
    from_booking_card: true
  })
  emit('watch', booking)
}
</script>
```

### With Instructor Substitution
```vue
<codex-booking-card
  :booking="bookingWithSubstitute"
  :show-instructor-photo="true"
/>

<!-- 
Displays instructor with substitution indicator when 
booking.event.metafields.original_instructor_name is present
-->
```

## CSS Classes
- `_c-card`: Main container class
- `_c-booking-card`: Booking card specific styling
- `_c-border`: Border styling (when enabled)
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-column`: Column layout utility
- `_c-row_lg`: Row layout on large screens
- `_c-items-center_lg`: Center align on large screens
- `_c-gap-sm`: Small gap utility
- `_c-grow`: Flex grow utility
- `_c-event-timing`: Event timing container
- `_c-text-sm`: Small text utility
- `_c-flex`: Flexbox utility
- `_c-gap-xs`: Extra small gap utility
- `_c-items-center`: Center align flex items
- `_c-text-icon`: Icon styling
- `_c-pie-container`: Pie chart container
- `_c-pie`: Pie chart styling
- `_c-pie-sm`: Small pie chart variant
- `_c-pie-info`: Pie chart information
- `_c-text-uppercase`: Uppercase text utility
- `_c-text-bold`: Bold text utility
- `_c-event-type`: Event type styling
- `_c-subtitle`: Subtitle text styling
- `_c-event-details`: Event details container
- `_c-gap-0`: No gap utility
- `_c-wrap`: Flex wrap utility
- `_c-gap-lg_lg`: Large gap on large screens
- `_c-instructor`: Instructor container
- `_c-instructor-name`: Instructor name styling
- `_c-substitute`: Substitute instructor indicator
- `_c-location`: Location container
- `_c-studio`: Studio container
- `_c-live-stream`: Live stream indicator
- `_c-btn-container`: Button container styling
- `_c-fill`: Fill container utility
- `_c-w-auto`: Auto width utility
- `_c-metrics`: Metrics container
- `_c-metric`: Individual metric styling
- `_c-metric-label`: Metric label styling
- `_c-metric-value`: Metric value styling

## Best Practices

### Recommended Usage Patterns
- Always provide proper booking object structure
- Handle loading states for better UX
- Implement proper cancellation confirmation flows
- Use appropriate display type for context
- Handle live stream and Zoom integrations properly
- Provide clear event information display
- Implement proper bookmark integration
- Track user interactions for analytics

### Common Pitfalls to Avoid
- Not handling missing event data gracefully
- Missing loading state implementation
- Forgetting to handle cancellation confirmations
- Not providing proper error handling for actions
- Missing responsive considerations
- Insufficient handling of different booking types
- Not tracking action states properly
- Missing accessibility considerations

### Accessibility Considerations
- Ensure cards are keyboard navigable
- Provide clear labels for interactive elements
- Use appropriate ARIA attributes for pie charts
- Ensure adequate color contrast for occupancy indicators
- Provide screen reader friendly event information
- Include proper focus management
- Use semantic HTML for booking data
- Handle disabled states properly for screen readers

### Error Handling
- Handle missing booking data gracefully
- Provide fallbacks for malformed event objects
- Handle action failures appropriately (cancel, watch, join)
- Display meaningful error states
- Clear error states when data updates
- Handle network connectivity issues
- Provide user feedback for failed operations

### State Management
- Track booking status properly
- Handle cancellation states dynamically
- Manage loading states consistently
- Update UI after booking actions
- Handle concurrent state changes
- Coordinate with parent component state
- Manage metrics display state appropriately

### Performance Considerations
- Optimize event data rendering
- Lazy load instructor photos when enabled
- Minimize re-renders during state changes
- Handle large booking lists efficiently
- Implement proper cleanup for event listeners
- Cache formatted event data appropriately
- Optimize occupancy calculations

### Booking Management
- Validate booking actions based on status
- Handle cancellation deadlines properly
- Provide clear action feedback
- Handle partial action failures
- Update booking state after successful actions
- Provide action progress feedback
- Handle action cancellation properly

### Event Integration
- Handle live stream availability properly
- Manage Zoom integration securely
- Coordinate with event booking systems
- Handle real-time occupancy updates
- Manage event status changes
- Coordinate bookmark state with events
- Handle event cancellation scenarios

### Metrics Display
- Format metrics consistently
- Handle missing metrics data gracefully
- Provide meaningful metric labels
- Handle metric calculation errors
- Display metrics appropriately for different activities
- Consider metric privacy settings
- Handle metric data loading states

## Component Registration
The component is registered as `codex-booking-card` in the application. 