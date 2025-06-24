# WaitlistCard Component

## Overview
The WaitlistCard component provides a detailed card interface for displaying individual waitlist entries with comprehensive event information. It features event timing, instructor details, location information, leave waitlist functionality, bookmark integration, and status-based styling for effective waitlist management.

## Basic Usage
```vue
<codex-waitlist-card
  :waitlist="waitlistData"
  :loading="false"
  @leave="handleLeaveWaitlist"
  @view-event="handleEventView"
/>
```

## Key Features
- Comprehensive event information display
- Event timing with duration
- Instructor details with substitution support
- Location and studio information
- Live stream indicators
- Leave waitlist functionality
- Bookmark integration for events
- Status-based visual states (cancelled, expired)
- Loading skeleton support
- Responsive layout with desktop adaptations
- Clickable event viewing

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| waitlist | Object\|Number | Yes | - | Waitlist data object or ID |
| loading | Boolean | No | false | Whether the card is in loading state |
| showInstructorPhoto | Boolean | No | true | Whether to show instructor photo |
| enableBorder | Boolean | No | false | Whether to enable card border styling |
| cancelling | Boolean | No | false | Whether waitlist is being cancelled |

### Common Props
All common props from `@/config/common` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| leave | `waitlist: Object` | Emitted when leave waitlist button is clicked |
| view-event | `waitlist.event: Object` | Emitted when card is clicked to view event |

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
  <!-- Custom content and waitlist display -->
</template>
```

### Footer Slot
```vue
<template #footer>
  <!-- Custom footer content -->
</template>
```

## Waitlist Object Structure
The waitlist prop expects an object with the following structure:
```javascript
{
  id: String|Number,          // Unique waitlist identifier
  status: String,             // Waitlist status ('active', 'cancelled', 'expired')
  event: {                    // Associated event information
    id: String|Number,        // Event ID
    start_at: Date,           // Event start time
    duration: Number,         // Event duration in minutes
    event_type: {             // Event type information
      name: String            // Event type name
    },
    instructor: {             // Instructor information
      first_name: String,     // Instructor first name
      last_name: String       // Instructor last name
    },
    studio: {                 // Studio information
      name: String,           // Studio name
      location: {             // Location information
        name: String          // Location name
      }
    },
    is_live_stream: Boolean,  // Whether event is live streamed
    metafields: {             // Additional event metadata
      original_instructor_name: String  // Original instructor if substituted
    }
  }
}
```

## Visual States
The component supports different visual states:
- **Active**: Normal card styling for active waitlists
- **Cancelled**: Disabled styling with `_c-disabled` class
- **Expired**: Disabled styling with `_c-disabled` class
- **Loading**: Skeleton placeholder during data loading
- **Processing**: Disabled state during leave operations

## Event Information Display
The card displays comprehensive event information:
- **Timing**: Start time and duration with icon
- **Event Type**: Event category/type name
- **Instructor**: Instructor name with substitution indicators
- **Location**: Venue location name
- **Studio**: Studio/room name
- **Live Stream**: Live streaming availability

## Interactive Features
- **Click to View**: Entire card is clickable to view event details
- **Leave Button**: Dedicated button to leave waitlist
- **Bookmark**: Bookmark functionality for the associated event
- **Status Handling**: Disabled interactions for cancelled/expired waitlists

## Loading State
The component supports skeleton loading with:
- **Skeleton Layout**: Structured placeholder layout
- **Timing Skeleton**: Time and occupancy placeholders
- **Event Type Skeleton**: Event type and details placeholders
- **Details Skeleton**: Event details placeholders
- **Border Support**: Optional border styling for skeletons

## Internationalization

The `WaitlistCard` component uses translation keys for waitlist event information and actions:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `time.mins` | Duration unit abbreviation | Event duration display |
| `timetable.instructor` | Instructor label | Event instructor information |
| `event.location` | Location label | Event venue information |
| `event.studio` | Studio label | Event studio information |
| `event.is_live_stream` | Live stream indicator label | Virtual event identification |
| `button.leave` | Leave waitlist button text | Action to remove from waitlist |
| `button.leaving` | Processing state for leave action | Button text while processing |

### Implementation Examples

```vue
<!-- Event timing and duration -->
<div class="_c-event-timing">
    {{ $filters.niceTime(waitlist.event?.start_at) }} •
    {{ waitlist.event?.duration }} {{ $t('time.mins') }}
</div>

<!-- Instructor information -->
<div v-if="waitlist.event?.instructor" class="_c-instructor">
    <span class="_c-text-bold">{{ $t('timetable.instructor') }}</span>
    <span class="_c-instructor-name">
        {{ waitlist.event.instructor.first_name }} {{ waitlist.event.instructor.last_name }}
    </span>
</div>

<!-- Location information -->
<div v-if="waitlist.event?.studio?.location" class="_c-location">
    <span class="_c-text-bold">{{ $t('event.location') }}</span>
    {{ waitlist.event.studio.location.name }}
</div>

<!-- Studio information -->
<div v-if="waitlist.event?.studio" class="_c-studio">
    <span class="_c-text-bold">{{ $t('event.studio') }}</span>
    {{ waitlist.event.studio.name }}
</div>

<!-- Live stream indicator -->
<div v-if="waitlist.event?.is_live_stream" class="_c-live-stream">
    <span class="_c-text-bold">{{ $t('event.is_live_stream') }}</span>
    {{ waitlist.event.is_live_stream }}
</div>

<!-- Leave waitlist button -->
<codex-button 
    v-if="waitlist.status != 'cancelled'"
    :defaultText="$t('button.leave')"
    :processingText="$t('button.leaving')"
    :disabled="cancelling === waitlist.id"
    :processing="cancelling === waitlist.id"
    @click.prevent.stop="leave(waitlist.id)"
/>
```

### Event Status Considerations

The component handles different waitlist states:
- Active waitlists show leave button
- Cancelled/expired waitlists are disabled
- Processing state shows different button text

### Child Component Integration

The component includes a bookmark component:

```vue
<codex-bookmark 
    v-if="customer && waitlist?.event?.id"
    type="events"
    :item="generateBookmarkIdentifier(waitlist.event)"
/>
```

### Notes
- Event type name comes directly from `waitlist.event.event_type.name` (not translated)
- Instructor names are displayed as provided in the data
- Location and studio names are raw venue information
- Time formatting handled through Vue filters
- Integration with bookmark functionality for event saving

## Examples

### Basic Implementation
```vue
<codex-waitlist-card
  :waitlist="waitlist"
  @leave="handleLeave"
  @view-event="viewEventDetails"
/>
```

### With Loading State
```vue
<codex-waitlist-card
  :waitlist="waitlist"
  :loading="isLoading"
  :enable-border="true"
/>
```

### With Cancelling State
```vue
<codex-waitlist-card
  :waitlist="waitlist"
  :cancelling="isCancelling"
  @leave="handleLeaveWaitlist"
/>
```

### Custom Header Content
```vue
<codex-waitlist-card
  :waitlist="waitlist"
  @leave="handleLeave"
>
  <template #header>
    <div class="waitlist-header">
      <div class="priority-badge" v-if="waitlist.priority">
        {{ waitlist.priority }}
      </div>
      <div class="waitlist-joined">
        Joined: {{ formatDate(waitlist.created_at) }}
      </div>
    </div>
  </template>
</codex-waitlist-card>
```

### Custom Footer with Additional Actions
```vue
<codex-waitlist-card
  :waitlist="waitlist"
  @leave="handleLeave"
>
  <template #footer>
    <div class="waitlist-actions">
      <button @click="shareWaitlist(waitlist)">Share</button>
      <button @click="setReminder(waitlist.event)">Set Reminder</button>
      <div class="waitlist-position">
        Position: {{ waitlist.position }}
      </div>
    </div>
  </template>
</codex-waitlist-card>
```

### In Waitlist List
```vue
<div class="waitlists-container">
  <codex-waitlist-card
    v-for="waitlist in waitlists"
    :key="waitlist.id"
    :waitlist="waitlist"
    :cancelling="cancellingIds.includes(waitlist.id)"
    @leave="handleLeaveWaitlist"
    @view-event="openEventModal"
  />
</div>
```

### With Event Tracking
```vue
<codex-waitlist-card
  :waitlist="waitlist"
  @leave="handleLeave"
  @view-event="trackEventView"
/>

<script setup>
const trackEventView = (event) => {
  // Track event viewing analytics
  analytics.track('waitlist_event_viewed', {
    event_id: event.id,
    event_type: event.event_type?.name,
    from_waitlist: true
  })
  
  // Navigate to event details
  router.push(`/events/${event.id}`)
}

const handleLeave = (waitlist) => {
  // Track leave intent
  analytics.track('waitlist_leave_initiated', {
    waitlist_id: waitlist.id,
    event_id: waitlist.event.id
  })
  
  emit('leave', waitlist)
}
</script>
```

### With Instructor Substitution
```vue
<codex-waitlist-card
  :waitlist="waitlistWithSubstitute"
  :show-instructor-photo="true"
/>

<!-- 
Displays instructor with substitution indicator when 
waitlist.event.metafields.original_instructor_name is present
-->
```

## CSS Classes
- `_c-card`: Main container class
- `_c-waitlist-card`: Waitlist card specific styling
- `_c-border`: Border styling (when enabled)
- `_c-disabled`: Disabled state for cancelled/expired waitlists
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
- `_c-event-type`: Event type styling
- `_c-subtitle`: Subtitle text styling
- `_c-event-details`: Event details container
- `_c-gap-0`: No gap utility
- `_c-wrap`: Flex wrap utility
- `_c-gap-lg_lg`: Large gap on large screens
- `_c-instructor`: Instructor container
- `_c-text-bold`: Bold text utility
- `_c-instructor-name`: Instructor name styling
- `_c-substitute`: Substitute instructor indicator
- `_c-location`: Location container
- `_c-studio`: Studio container
- `_c-live-stream`: Live stream indicator
- `_c-btn-container`: Button container styling
- `_c-fill`: Fill container utility
- `_c-w-auto`: Auto width utility

## Best Practices

### Recommended Usage Patterns
- Always provide proper waitlist object structure
- Handle loading states for better UX
- Implement proper event viewing navigation
- Use status-based styling appropriately
- Handle leave waitlist confirmations
- Provide clear event information display
- Implement proper bookmark integration
- Track user interactions for analytics

### Common Pitfalls to Avoid
- Not handling missing event data gracefully
- Missing loading state implementation
- Forgetting to handle status-based styling
- Not providing leave confirmation flows
- Missing event navigation handling
- Insufficient error handling for malformed data
- Not tracking cancellation states properly
- Missing accessibility considerations

### Accessibility Considerations
- Ensure cards are keyboard navigable
- Provide clear labels for interactive elements
- Use appropriate ARIA attributes for states
- Ensure adequate color contrast for status indicators
- Provide screen reader friendly event information
- Include proper focus management
- Use semantic HTML for event data
- Handle disabled states properly for screen readers

### Error Handling
- Handle missing waitlist data gracefully
- Provide fallbacks for malformed event objects
- Handle leave action failures appropriately
- Display meaningful error states
- Clear error states when data updates
- Handle bookmark operation failures
- Provide user feedback for failed operations

### State Management
- Track waitlist status properly
- Handle status changes dynamically
- Manage loading states consistently
- Update UI after leave operations
- Handle concurrent state changes
- Coordinate with parent component state
- Manage bookmark state appropriately

### Performance Considerations
- Optimize event data rendering
- Lazy load instructor photos when enabled
- Minimize re-renders during status changes
- Handle large waitlist lists efficiently
- Implement proper cleanup for event listeners
- Cache formatted event data appropriately
- Optimize bookmark operations

### Event Data Management
- Validate event data structure
- Handle missing event properties gracefully
- Format event timing consistently
- Display instructor information properly
- Handle substitution scenarios appropriately
- Manage location data effectively
- Format duration values consistently

### Interaction Management
- Prevent event propagation appropriately
- Handle click events correctly
- Manage button states during operations
- Provide clear interaction feedback
- Handle disabled state interactions
- Coordinate multiple interactive elements
- Prevent double-click operations

### Status Management
- Display status indicators clearly
- Handle status transitions properly
- Update visual states immediately
- Provide clear status feedback
- Handle edge cases in status logic
- Coordinate status with parent components
- Track status changes for analytics

## Component Registration
The component is registered as `codex-waitlist-card` in the application. 