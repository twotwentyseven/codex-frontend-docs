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

## Examples

### Basic Event Listing
```vue
<template>
  <div class="simple-events">
    <codex-event-listing 
      :group="'all-events'"
      :days-to-show="14"
      :show-date-on-event-card="true"
    />
  </div>
</template>
```

### Filtered Event Listing with Custom Header
```vue
<template>
  <div class="filtered-events">
    <codex-filter-context :group="'fitness-events'" :default-values="defaultFilters">
      <template #default="{ filters }">
        <codex-event-listing 
          :group="'fitness-events'"
          :show-elapsed-events="false"
          :disable-unavailable-events="true"
        >
          <template #header="{ events, isFiltered, activeOptions, removeAllFilters }">
            <div class="events-header">
              <div class="header-content">
                <h2>Upcoming Events</h2>
                <p>Discover and book your next fitness experience</p>
              </div>
              
              <div class="events-stats">
                <div class="stat-card">
                  <span class="stat-number">{{ events?.length || 0 }}</span>
                  <span class="stat-label">Events Available</span>
                </div>
                <div class="stat-card">
                  <span class="stat-number">{{ getUpcomingCount(events) }}</span>
                  <span class="stat-label">This Week</span>
                </div>
                <div class="stat-card">
                  <span class="stat-number">{{ getAvailableSpacesCount(events) }}</span>
                  <span class="stat-label">Open Spaces</span>
                </div>
              </div>
              
              <div v-if="isFiltered" class="active-filters">
                <h4>Active Filters:</h4>
                <div class="filter-tags">
                  <span 
                    v-for="option in activeOptions" 
                    :key="option.key"
                    class="filter-tag"
                  >
                    {{ option.label }}: {{ option.value }}
                    <button @click="removeSelection(option.key, option.value)" class="remove-filter">
                      <i class="ri-close-line"></i>
                    </button>
                  </span>
                  <button @click="removeAllFilters" class="clear-all-filters">
                    Clear All
                  </button>
                </div>
              </div>
            </div>
          </template>
          
          <template #primary-filters="{ group }">
            <codex-contextual-filter
              :group="group"
              definition="eventTypes"
              filter-key="event_type.handle"
              filter-type="checkbox-group"
              :label="'Event Types'"
              :primary-filter="true"
            />
            
            <codex-contextual-filter
              :group="group"
              definition="timeRanges"
              filter-key="time_range"
              filter-type="radio"
              :label="'Time of Day'"
              :primary-filter="true"
            />
          </template>
          
          <template #secondary-filters="{ group }">
            <codex-contextual-filter
              :group="group"
              definition="instructors"
              filter-key="instructor.handle"
              filter-type="multi-select"
              :label="'Instructors'"
            />
            
            <codex-contextual-filter
              :group="group"
              definition="locations"
              filter-key="location.handle"
              filter-type="checkbox-group"
              :label="'Locations'"
            />
            
            <codex-contextual-filter
              :group="group"
              definition="availability"
              filter-key="availability"
              filter-type="radio"
              :label="'Availability'"
            />
          </template>
          
          <template #event-card="slotProps">
            <enhanced-event-card
              v-bind="slotProps"
              :show-instructor-details="true"
              :show-booking-status="true"
              @quick-book="handleQuickBook"
            />
          </template>
          
          <template #footer="{ events }">
            <div class="events-footer">
              <div class="footer-info">
                <h4>Can't Find What You're Looking For?</h4>
                <p>We're always adding new events. Check back soon or set up alerts for your favorite instructors.</p>
                
                <div class="footer-actions">
                  <button @click="setupAlerts" class="alert-btn">
                    <i class="ri-notification-line"></i>
                    Set Up Alerts
                  </button>
                  <button @click="requestEvent" class="request-btn">
                    <i class="ri-add-line"></i>
                    Request Event
                  </button>
                </div>
              </div>
              
              <!-- Pagination handled by default footer slot -->
              <codex-pagination :group="group" :loading="loading" />
            </div>
          </template>
        </codex-event-listing>
      </template>
    </codex-filter-context>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const defaultFilters = {
  'event_type.handle': [],
  'time_range': '',
  'instructor.handle': [],
  'location.handle': [],
  'availability': ''
}

const getUpcomingCount = (events) => {
  const weekFromNow = new Date()
  weekFromNow.setDate(weekFromNow.getDate() + 7)
  
  return events?.filter(event => {
    const eventDate = new Date(event.start_at)
    return eventDate <= weekFromNow
  }).length || 0
}

const getAvailableSpacesCount = (events) => {
  return events?.reduce((total, event) => {
    return total + Math.max(0, event.capacity - event.occupancy)
  }, 0) || 0
}

const handleQuickBook = (event) => {
  console.log('Quick booking for event:', event.id)
}

const setupAlerts = () => {
  console.log('Setting up event alerts')
}

const requestEvent = () => {
  console.log('Opening event request form')
}
</script>

<style scoped>
.events-header {
  padding: 2rem 0;
  text-align: center;
}

.header-content {
  margin-bottom: 2rem;
}

.events-stats {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin: 2rem 0;
  flex-wrap: wrap;
}

.stat-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 8px;
  min-width: 120px;
}

.stat-number {
  font-size: 2rem;
  font-weight: bold;
  color: #007bff;
}

.stat-label {
  font-size: 0.875rem;
  color: #6c757d;
  margin-top: 0.25rem;
}

.active-filters {
  margin-top: 1.5rem;
  text-align: left;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.filter-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 0.5rem;
}

.filter-tag {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  padding: 0.25rem 0.5rem;
  background: #e9ecef;
  border-radius: 12px;
  font-size: 0.875rem;
}

.remove-filter {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  margin-left: 0.25rem;
}

.clear-all-filters {
  padding: 0.25rem 0.75rem;
  background: #dc3545;
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 0.875rem;
  cursor: pointer;
}

.events-footer {
  padding: 2rem 0;
  text-align: center;
  border-top: 1px solid #dee2e6;
}

.footer-actions {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-top: 1rem;
}

.alert-btn,
.request-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  border: 1px solid #007bff;
  background: white;
  color: #007bff;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s;
}

.alert-btn:hover,
.request-btn:hover {
  background: #007bff;
  color: white;
}
</style>
```

### Bookmark-Only Event Listing
```vue
<template>
  <div class="bookmarked-events">
    <codex-event-listing 
      :group="'bookmarked-events'"
      :only-show-bookmarked="true"
      :show-date-on-event-card="true"
      :days-to-show="30"
    >
      <template #header="{ events }">
        <div class="bookmarks-header">
          <h2>
            <i class="ri-bookmark-fill"></i>
            Your Favorite Events
          </h2>
          <p>{{ events?.length || 0 }} bookmarked events</p>
        </div>
      </template>
      
      <template #content="{ events, loading }">
        <div v-if="!loading && (!events || events.length === 0)" class="no-bookmarks">
          <div class="no-bookmarks-icon">
            <i class="ri-bookmark-line"></i>
          </div>
          <h3>No Bookmarked Events</h3>
          <p>Start bookmarking events you're interested in to see them here.</p>
          <button @click="browseEvents" class="browse-btn">
            Browse All Events
          </button>
        </div>
        
        <div v-else class="bookmarked-events-grid">
          <codex-timetable-event-card
            v-for="event in events"
            :key="event.id"
            :event="event"
            :customer="customer"
            :show-event-date="true"
            :is-bookmarked="() => true"
            :toggle-bookmark="toggleBookmark"
            :bookmark-identifier="generateBookmarkIdentifier(event)"
            :has-booking="hasBookingForEvent(event)"
            :go-to-event="goToEvent"
            :cancel-booking="cancelBooking"
            :enable-border="true"
            @booking-cancelled="handleBookingCancelled"
            @event-updated="handleEventUpdated"
          />
        </div>
      </template>
    </codex-event-listing>
  </div>
</template>

<script setup>
const browseEvents = () => {
  // Navigate to main events page
  window.location.href = '/events'
}
</script>

<style scoped>
.bookmarks-header {
  text-align: center;
  padding: 2rem 0;
}

.bookmarks-header h2 {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  color: #007bff;
}

.no-bookmarks {
  text-align: center;
  padding: 4rem 2rem;
}

.no-bookmarks-icon {
  font-size: 4rem;
  color: #dee2e6;
  margin-bottom: 1rem;
}

.browse-btn {
  padding: 0.75rem 2rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  margin-top: 1rem;
}

.bookmarked-events-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1rem;
  padding: 1rem;
}
</style>
```

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