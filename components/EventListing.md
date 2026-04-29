# EventListing Component

## Overview
The EventListing component displays events in a list format with filtering capabilities, pagination, and booking functionality. It provides a streamlined interface for browsing events across multiple dates without the grid-based layout of the timetable. The component supports comprehensive filtering, bookmark management, and modal event details with integrated booking flows.

## Basic Usage
```vue
<template>
  <div class="events-listing">
    <codex-event-listing 
      :group="'events-list'"
      :show-filters="true"
      :days-to-show="7"
    />
  </div>
</template>
```

## Key Features
- List-based event display across multiple days
- Comprehensive filtering system integration
- Pagination support for large event collections
- Event booking and cancellation functionality
- Bookmark functionality for logged-in users
- Modal event details view
- Loading states with skeleton cards
- Mobile-responsive design
- Customizable event card content

## Configuration Props

### Display Control Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showHeader` | `Boolean` | `true` | Show component header |
| `showFilters` | `Boolean` | `true` | Show filter interface |
| `showDateOnEventCard` | `Boolean` | `false` | Show date on individual event cards |

### Date/Time Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `startDate` | `String` | `''` | Starting date for event listing |
| `daysToShow` | `Number` | `7` | Number of days to include in listing |
| `showElapsedEvents` | `Boolean` | `false` | Include past events in listing |

### User Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showEventsWhenLoggedOut` | `Boolean` | `false` | Show events for anonymous users |
| `onlyShowBookmarked` | `Boolean` | `false` | Only display bookmarked events |

### Modal Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showEventAsModal` | `Boolean` | `true` | Open events in modal vs page navigation |
| `portalSelector` | `String` | `'body'` | Portal selector for modal rendering |

### Interaction Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `disableUnavailableEvents` | `Boolean` | `true` | Disable styling for unavailable events |

### Filter Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `group` | `String` | `'default'` | Filter group identifier |
| `preFilter` | `Object` | `{}` | Pre-applied filters |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...filters.props` | `Various` | Inherits filter-related props |
| `...commonProps` | `Various` | Inherits common props from config |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Enables border on event cards |
| `showPoweredBy` | Controls "Powered By" footer display |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatDate` | Formats event dates and times |
| `isMobile` | Handles responsive behavior |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `booking-created` | `{ event, booking }` | Emitted when booking is created |
| `booking-cancelled` | `{ event, booking }` | Emitted when booking is cancelled |
| `event-updated` | `{ event }` | Emitted when event data is updated |

## Slots

### Header Slot
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ loading, error, genericErrors, events, hydratedFilters, isFiltered, activeOptions, removeSelection, removeAllFilters, bookmarksOnly, showBookmarkToggle }` | Custom header content |

### Filter Slots
| Slot | Props | Description |
|------|-------|-------------|
| `filters` | N/A | Complete filter interface |
| `primary-filters` | `{ group }` | Primary filter content |
| `secondary-filters` | `{ group }` | Secondary filter content |

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `content` | `{ loading, error, genericErrors, events, hydratedFilters, isFiltered, activeOptions, removeSelection, removeAllFilters, bookmarksOnly, showBookmarkToggle }` | Main content area |
| `event-card` | `{ event, customer, showEventDate, isBookmarked, toggleBookmark, bookmarkIdentifier, goToEvent, cancelBooking, hasBooking, loading }` | Individual event cards |
| `event-modal` | `{ id, eventObject, isModal }` | Event modal content |

### Footer Slot
| Slot | Props | Description |
|------|-------|-------------|
| `footer` | `{ loading, error, genericErrors, events, hydratedFilters, isFiltered, activeOptions, removeSelection, removeAllFilters, bookmarksOnly, showBookmarkToggle }` | Footer with pagination |

### Error Messages Slot
| Slot | Props | Description |
|------|-------|-------------|
| `error-messages` | `{ genericErrors, error }` | Error message display |

## Loading States

The component provides comprehensive loading states:
- Skeleton cards during data loading
- Loading indicators for filters
- Event card loading states
- Pagination loading states

## Filter Integration

The component integrates with the filter system through:
- Filter context detection and registration
- Primary and secondary filter slots
- Filter result tracking and display
- Bookmark filtering integration

## Pagination Integration

The component includes pagination functionality:
- Automatic pagination based on result count
- Loading states during page transitions
- Integration with filter context
- URL state management

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-event-listing` | Main container class |
| `_c-card` | Card styling |
| `_c-header` | Header section styling |
| `_c-content` | Content area styling |
| `_c-footer` | Footer section styling |
| `_c-event-listing-container` | Events container styling |
| `_c-event-listing-grid` | Events grid layout |
| `_c-grid` | Grid system class |

## Internationalization

The component uses the following translation keys:

### Core Interface Messages
| Key | Usage |
|-----|-------|
| `event.no_events` | No events available message |

### Translation Usage Examples
```vue
<!-- No events message -->
<div v-else class="_c-no-results">
  <slot name="no-results">{{ $t('event.no_events') }}</slot>
</div>
```

### Translation Notes

#### Minimal Direct Translation Requirements
The EventListing component has minimal direct translation needs as it primarily orchestrates other components. Most translation is handled by:

#### Child Component Translations
The component integrates with several translated child components:
- `codex-timetable-event-card` components handle event-specific translations (times, booking status, instructor names, etc.)
- `codex-filter-wrapper` and contextual filters handle filter-related translations
- `codex-pagination` handles pagination navigation translations
- `codex-event` modal component handles detailed event translations

#### Event Card Integration
Event cards within the listing use extensive translations for:
- Event timing and duration information
- Booking status and availability
- Instructor and location details
- Action buttons (book, cancel, view details)
- Status indicators (full, available, waitlist)

#### Modal Integration
When `showEventAsModal` is enabled, the event modal uses translations for:
- Event details and descriptions
- Booking forms and validation
- Payment and checkout processes
- Success and error messages

#### Filter Integration
The filter system uses translations for:
- Filter labels and options
- Search placeholders
- Filter result counts
- Clear filter actions

## Best Practices

### Event Display
- Show events in chronological order by default
- Provide clear visual hierarchy for event information
- Use consistent styling across all event cards
- Handle various event types and states appropriately
- Display relevant booking status and availability

### Filtering
- Provide relevant filter options for event discovery
- Show filter result counts and active filters
- Allow easy filter clearing and modification
- Handle filter loading states appropriately
- Persist filter states in URL when beneficial

### Performance
- Implement pagination for large event sets
- Use skeleton loading for better perceived performance
- Cache frequently accessed event data
- Optimize image loading for event photos
- Handle rapid filter changes efficiently

### User Experience
- Provide clear feedback for all user actions
- Show loading states during data operations
- Handle empty states with helpful messaging
- Support both bookmark and regular views
- Implement responsive design for mobile devices

### Accessibility
- Ensure proper heading hierarchy throughout
- Provide descriptive labels for all interactive elements
- Support keyboard navigation
- Use appropriate ARIA attributes
- Test with screen readers for event information

### Mobile Optimization
- Use touch-friendly interface elements
- Optimize for smaller screen sizes
- Consider reduced information density
- Implement swipe gestures where appropriate
- Test on various mobile devices

## Component Registration
```javascript
// Global registration
app.component('CodexEventListing', EventListing)

// Local registration  
import EventListing from '@/components/bookings/EventListing.vue'

export default {
  components: {
    CodexEventListing: EventListing
  }
}
``` 