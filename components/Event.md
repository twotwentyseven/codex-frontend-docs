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