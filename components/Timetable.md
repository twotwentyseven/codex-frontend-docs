# Timetable Component

## Overview
The Timetable component displays a comprehensive weekly class schedule with filtering, navigation, and booking capabilities. It provides a grid-based layout showing events by day with calendar navigation, real-time capacity tracking, and integrated booking functionality. The component supports filtering by event types, instructors, and locations, and includes bookmark functionality for logged-in users.

## Basic Usage
```vue
<template>
  <div class="class-timetable">
    <codex-timetable 
      :group="'weekly-schedule'"
      :days-to-render="7"
      :show-filters="true"
      :show-calendar-nav="true"
    />
  </div>
</template>
```

## Key Features
- Weekly timetable grid layout with configurable days
- Swiper-based calendar navigation
- Comprehensive filtering system integration
- Event booking and cancellation
- Bookmark functionality for favorites
- Real-time capacity and availability tracking
- Modal event details view
- Loading states with skeleton cards
- Mobile-responsive design
- Customizable slot content

## Configuration Props

### Display Control Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showHeader` | `Boolean` | `true` | Show timetable header |
| `showFilters` | `Boolean` | `true` | Show filter interface |
| `showCalendarNav` | `Boolean` | `true` | Show calendar navigation |
| `showDateHeader` | `Boolean` | `true` | Show date headers on columns |
| `centerDateHeader` | `Boolean` | `false` | Center align date headers |
| `showDateOnEventCard` | `Boolean` | `false` | Show date on individual event cards |

### Navigation Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `daysToRender` | `Number` | `7` | Number of days to display |
| `navDaysToRender` | `Number` | `7` | Number of days in navigation |
| `updateNavPositionOnClick` | `Boolean` | `true` | Update nav position when date clicked |

### Booking Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `enableBookmarkToggling` | `Boolean` | `true` | Enable bookmark functionality |
| `disableUnavailableEvents` | `Boolean` | `true` | Disable styling for unavailable events |
| `showEventAsModal` | `Boolean` | `true` | Open events in modal vs page |

### Date/Time Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `startDate` | `String` | `''` | Starting date for timetable |
| `showFromStartOfDay` | `Boolean` | `true` | Start from beginning of day |

### Filter Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `group` | `String` | `'default'` | Filter group identifier |
| `preFilter` | `Object` | `{}` | Pre-applied filters |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |
| `...filters.props` | `Various` | Inherits filter-related props |

## Common Props Usage
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Enables border on event cards |
| `showPoweredBy` | Controls "Powered By" footer display |

## Common Functions Usage
| Function | Usage |
|----------|-------|
| `formatDate` | Formats dates in headers and navigation |
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
| `header` | `{ loading, error, genericErrors, events, showBookmarked }` | Custom header content |

### Filter Slots
| Slot | Props | Description |
|------|-------|-------------|
| `filters` | N/A | Complete filter interface |
| `primary-filters` | `{ group }` | Primary filter content |
| `secondary-filters` | `{ group }` | Secondary filter content |

### Navigation Slots
| Slot | Props | Description |
|------|-------|-------------|
| `calendar-nav` | `{ showCalendarNav }` | Calendar navigation interface |
| `prev-arrow` | N/A | Previous navigation arrow |
| `next-arrow` | N/A | Next navigation arrow |

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `content` | `{ loading, error, genericErrors, events, showBookmarked }` | Main timetable content |
| `event-card` | `{ event, customer, showEventDate, isBookmarked, toggleBookmark, ... }` | Individual event cards |
| `event-modal` | `{ id, eventObject, isModal }` | Event modal content |

### Footer Slot
| Slot | Props | Description |
|------|-------|-------------|
| `footer` | `{ loading, error, genericErrors, events, showBookmarked }` | Footer content |

### Error Messages Slot
| Slot | Props | Description |
|------|-------|-------------|
| `error-messages` | `{ genericErrors, error }` | Error message display |

## Loading States

The component provides comprehensive loading states:
- Skeleton cards during data loading
- Loading indicators for navigation
- Event card loading states
- Filter loading states

## Filter Integration

The component integrates with the filter system through:
- Contextual filters for event types, locations, instructors
- Primary and secondary filter slots
- Bookmark toggle for showing favorites only
- Filter result counts

## Calendar Navigation

The component includes Swiper-based calendar navigation:
- Smooth sliding between date ranges
- Custom arrow controls
- Active date highlighting
- Responsive navigation sizing

## Examples

### Basic Weekly Timetable
```vue
<template>
  <div class="weekly-schedule">
    <codex-timetable 
      :group="'fitness-classes'"
      :days-to-render="7"
      :start-date="currentWeekStart"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'

const currentWeekStart = ref(new Date().toISOString().split('T')[0])
</script>
```

### Custom Filtered Timetable
```vue
<template>
  <div class="filtered-timetable">
    <codex-filter-context :group="'studio-classes'" :default-values="defaultFilters">
      <template #default="{ filters }">
        <codex-timetable 
          :group="'studio-classes'"
          :pre-filter="preAppliedFilters"
          :show-calendar-nav="true"
          :show-date-header="true"
          :center-date-header="true"
        >
          <template #header="{ events, showBookmarked }">
            <div class="timetable-header">
              <h2>Studio Class Schedule</h2>
              <div class="schedule-stats">
                <div class="stat">
                  <span class="stat-value">{{ events?.length || 0 }}</span>
                  <span class="stat-label">Classes This Week</span>
                </div>
                <div class="stat">
                  <span class="stat-value">{{ getAvailableSpaces(events) }}</span>
                  <span class="stat-label">Available Spaces</span>
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
              :label="'Class Types'"
              :primary-filter="true"
              :enable-border="false"
            />
            
            <codex-contextual-filter
              :group="group"
              definition="timeSlots"
              filter-key="time_slot"
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
              filter-type="checkbox-group"
              :label="'Instructors'"
            />
            
            <codex-contextual-filter
              :group="group"
              definition="locations"
              filter-key="location.handle"
              filter-type="checkbox-group"
              :label="'Studios'"
            />
            
            <codex-contextual-filter
              :group="group"
              definition="difficultyLevels"
              filter-key="difficulty_level"
              filter-type="multi-select"
              :label="'Difficulty'"
            />
          </template>
          
          <template #event-card="slotProps">
            <custom-event-card
              v-bind="slotProps"
              :show-instructor-photo="true"
              :show-difficulty-badge="true"
              @book-event="handleBookEvent"
              @cancel-booking="handleCancelBooking"
            />
          </template>
          
          <template #footer="{ events }">
            <div class="timetable-footer">
              <div class="booking-info">
                <h4>Booking Information</h4>
                <ul>
                  <li>Classes can be booked up to 7 days in advance</li>
                  <li>Cancellations must be made 2 hours before class start</li>
                  <li>Late cancellations may incur charges</li>
                </ul>
              </div>
              
              <div class="contact-info">
                <h4>Need Help?</h4>
                <button @click="openSupport" class="support-btn">
                  Contact Studio
                </button>
              </div>
            </div>
          </template>
        </codex-timetable>
      </template>
    </codex-filter-context>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const defaultFilters = {
  'event_type.handle': [],
  'time_slot': '',
  'instructor.handle': [],
  'location.handle': [],
  'difficulty_level': []
}

const preAppliedFilters = {
  'status': 'active',
  'is_bookable': true
}

const getAvailableSpaces = (events) => {
  return events?.reduce((total, event) => {
    return total + (event.capacity - event.occupancy)
  }, 0) || 0
}

const handleBookEvent = (event) => {
  console.log('Booking event:', event.id)
}

const handleCancelBooking = (event) => {
  console.log('Cancelling booking for event:', event.id)
}

const openSupport = () => {
  console.log('Opening support')
}
</script>
```

### Compact Mobile Timetable
```vue
<template>
  <div class="mobile-timetable">
    <codex-timetable 
      :group="'mobile-classes'"
      :days-to-render="3"
      :nav-days-to-render="5"
      :show-date-on-event-card="true"
      :show-calendar-nav="true"
      :center-date-header="true"
    >
      <template #calendar-nav="{ showCalendarNav }">
        <div v-if="showCalendarNav" class="mobile-nav">
          <div class="nav-header">
            <h3>Select Date</h3>
            <div class="current-week">
              Week of {{ formatCurrentWeek() }}
            </div>
          </div>
          
          <!-- Custom mobile-optimized navigation -->
          <div class="mobile-date-picker">
            <button 
              v-for="date in mobileNavDates" 
              :key="date.value"
              :class="['date-btn', { active: date.active }]"
              @click="selectDate(date.value)"
            >
              <div class="day-name">{{ date.dayName }}</div>
              <div class="day-number">{{ date.dayNumber }}</div>
              <div class="event-count">{{ date.eventCount }} classes</div>
            </button>
          </div>
        </div>
      </template>
      
      <template #event-card="{ event, customer, hasBooking, goToEvent }">
        <div class="mobile-event-card">
          <div class="event-time">
            {{ formatTime(event.start_at) }}
          </div>
          
          <div class="event-details">
            <h4>{{ event.event_type?.name }}</h4>
            <div class="event-meta">
              <span class="instructor">{{ event.instructor?.first_name }}</span>
              <span class="capacity">
                {{ event.capacity - event.occupancy }} spaces left
              </span>
            </div>
          </div>
          
          <div class="event-actions">
            <button 
              v-if="!hasBooking"
              @click="goToEvent(event, 'book')"
              class="book-btn"
              :disabled="event.is_fully_booked"
            >
              {{ event.is_fully_booked ? 'Full' : 'Book' }}
            </button>
            
            <button 
              v-else
              @click="goToEvent(event, 'view')"
              class="booked-btn"
            >
              Booked
            </button>
          </div>
        </div>
      </template>
    </codex-timetable>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const mobileNavDates = computed(() => {
  // Generate mobile navigation dates
  const dates = []
  const today = new Date()
  
  for (let i = 0; i < 7; i++) {
    const date = new Date(today)
    date.setDate(today.getDate() + i)
    
    dates.push({
      value: date.toISOString().split('T')[0],
      dayName: date.toLocaleDateString('en', { weekday: 'short' }),
      dayNumber: date.getDate(),
      eventCount: Math.floor(Math.random() * 8) + 2, // Mock data
      active: i === 0
    })
  }
  
  return dates
})

const formatCurrentWeek = () => {
  const today = new Date()
  return today.toLocaleDateString('en', { 
    month: 'short', 
    day: 'numeric' 
  })
}

const formatTime = (dateTime) => {
  return new Date(dateTime).toLocaleTimeString('en', {
    hour: 'numeric',
    minute: '2-digit',
    hour12: true
  })
}

const selectDate = (dateValue) => {
  console.log('Selected date:', dateValue)
}
</script>

<style scoped>
.mobile-timetable {
  max-width: 100%;
  overflow-x: hidden;
}

.mobile-nav {
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 8px;
  margin-bottom: 1rem;
}

.nav-header {
  text-align: center;
  margin-bottom: 1rem;
}

.current-week {
  font-size: 0.875rem;
  color: #6c757d;
}

.mobile-date-picker {
  display: flex;
  gap: 0.5rem;
  overflow-x: auto;
  padding: 0.5rem 0;
}

.date-btn {
  flex: 0 0 auto;
  padding: 0.75rem;
  border: 1px solid #dee2e6;
  border-radius: 6px;
  background: white;
  cursor: pointer;
  text-align: center;
  min-width: 80px;
  transition: all 0.2s;
}

.date-btn.active {
  background: #007bff;
  color: white;
  border-color: #007bff;
}

.day-name {
  font-size: 0.75rem;
  text-transform: uppercase;
  font-weight: bold;
}

.day-number {
  font-size: 1.25rem;
  font-weight: bold;
  margin: 0.25rem 0;
}

.event-count {
  font-size: 0.625rem;
  opacity: 0.8;
}

.mobile-event-card {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  background: white;
  margin-bottom: 0.5rem;
}

.event-time {
  flex: 0 0 auto;
  font-weight: bold;
  color: #007bff;
  font-size: 0.875rem;
}

.event-details {
  flex: 1;
}

.event-details h4 {
  margin: 0 0 0.25rem 0;
  font-size: 1rem;
}

.event-meta {
  font-size: 0.875rem;
  color: #6c757d;
}

.event-actions {
  flex: 0 0 auto;
}

.book-btn,
.booked-btn {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  font-size: 0.875rem;
  cursor: pointer;
}

.book-btn {
  background: #28a745;
  color: white;
}

.book-btn:disabled {
  background: #6c757d;
  cursor: not-allowed;
}

.booked-btn {
  background: #17a2b8;
  color: white;
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-timetable` | Main container class |
| `_c-card` | Card styling |
| `_c-header` | Header section styling |
| `_c-content` | Content area styling |
| `_c-footer` | Footer section styling |
| `_c-calendar-nav` | Calendar navigation styling |
| `_c-swiper` | Swiper container styling |
| `_c-calendar-nav-card` | Navigation date cards |
| `_c-in-view` | Active navigation slide |
| `_c-loading` | Loading state styling |
| `_c-timetable-container` | Main timetable grid container |
| `_c-timetable-grid` | Timetable grid layout |
| `_c-grid` | Grid system class |
| `_c-column` | Grid column styling |
| `_c-day-header` | Date header styling |
| `_c-no-events` | Empty day styling |
| `_c-empty-message` | No events message |
| `_c-bookmarks-only` | Bookmarks filter styling |

## Internationalization

The component uses the following translation keys:

### Core Timetable Interface
| Key | Usage |
|-----|-------|
| `timetable.title` | Default page title when title prop is not provided |
| `timetable.introduction` | Header description text |
| `timetable.timetable` | Default timetable heading |
| `timetable.loading_timetable` | Loading state message |

### Date and Time Display
| Key | Usage |
|-----|-------|
| `timetable.today` | Today's date label |
| `timetable.tomorrow` | Tomorrow's date label |
| `timetable.yesterday` | Yesterday's date label |
| `timetable.week_of` | Week of date prefix |

### Event Status and Messages
| Key | Usage |
|-----|-------|
| `timetable.no_events_today` | Empty day message |
| `timetable.no_events_on` | Empty specific date message (with date parameter) |
| `timetable.no_events_found` | No events found after filtering |
| `timetable.no_events_match_your_filters` | No matching events message |

### Event Information Display
| Key | Usage |
|-----|-------|
| `timetable.full` | Full event indicator |
| `timetable.spaces` | Available spaces label |
| `time.mins` | Minutes label for duration |
| `timetable.instructor` | Instructor label |
| `event.location` | Location label |
| `event.studio` | Studio label |

### Filtering and Navigation
| Key | Usage |
|-----|-------|
| `filter.bookmarks_only` | Bookmarks filter toggle |
| `filter.show_favorites` | Show favorites toggle |
| `filter.event_type` | Event type filter label |
| `filter.location` | Location filter label |
| `filter.instructor` | Instructor filter label |
| `filter.hide_full_events` | Hide full events filter |
| `filter.show_available_only` | Show available only filter |

### Mobile Navigation
| Key | Usage |
|-----|-------|
| `timetable.select_date` | Mobile date picker instruction |
| `timetable.events_count` | Event count display (with count parameter) |
| `button.previous_week` | Previous week navigation |
| `button.next_week` | Next week navigation |
| `button.today` | Today navigation button |

### Translation Usage Examples
```vue
<!-- Timetable header -->
<codex-title :content="title || $t('timetable.title')" />
<codex-paragraph :content="introduction || $t('timetable.introduction')" />

<!-- Loading state -->
<div v-if="loading" class="loading-state">
  {{ $t('timetable.loading_timetable') }}
</div>

<!-- Empty state -->
<div v-if="!events.length" class="empty-state">
  <template v-if="isToday">
    {{ $t('timetable.no_events_today') }}
  </template>
  <template v-else>
    {{ $t('timetable.no_events_on', { date: formattedDate }) }}
  </template>
</div>

<!-- Event capacity display -->
<div class="event-capacity">
  <template v-if="event.occupancy === event.capacity">
    {{ $t('timetable.full') }}
  </template>
  <template v-else>
    {{ event.available_spaces }} {{ $t('timetable.spaces') }}
  </template>
</div>

<!-- Filter controls -->
<label>
  <input type="checkbox" v-model="showBookmarksOnly">
  {{ $t('filter.bookmarks_only') }}
</label>
```

## Best Practices

### Timetable Layout
- Use appropriate column counts for screen size
- Implement responsive design for mobile devices
- Show clear date headers and navigation
- Handle empty days gracefully
- Provide loading states for data fetching

### Event Display
- Show essential event information clearly
- Use consistent styling across event cards
- Implement proper booking status indicators
- Handle capacity and availability clearly
- Provide clear call-to-action buttons

### Navigation
- Implement smooth calendar navigation
- Show current position clearly
- Handle navigation boundaries properly
- Support both touch and click interactions
- Provide keyboard navigation support

### Filtering
- Provide relevant filter options
- Show filter result counts
- Allow easy filter clearing
- Persist important filter states
- Handle filter loading states

### Performance
- Implement virtual scrolling for large datasets
- Cache event data appropriately
- Optimize image loading for instructors
- Use skeleton loading for better UX
- Handle rapid navigation efficiently

### Accessibility
- Ensure proper heading hierarchy
- Provide descriptive labels for navigation
- Support keyboard navigation
- Use appropriate ARIA attributes
- Test with screen readers

### Mobile Optimization
- Use touch-friendly navigation controls
- Implement swipe gestures where appropriate
- Optimize for smaller screens
- Consider reduced information density
- Test on various device sizes

## Component Registration
```javascript
// Global registration
app.component('CodexTimetable', Timetable)

// Local registration  
import Timetable from '@/components/bookings/Timetable.vue'

export default {
  components: {
    CodexTimetable: Timetable
  }
}
``` 