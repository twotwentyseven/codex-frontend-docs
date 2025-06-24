# PastBookings Component

## Overview
The PastBookings component provides a comprehensive interface for displaying and managing customer past bookings with grid layout, pagination support, and booking actions. It features booking card integration, error handling, loading states, pagination, and event handling for effective historical booking management.

## Basic Usage
```vue
<codex-past-bookings
  :per-page="9"
  :group="'past-bookings'"
  :event-metrics="true"
/>
```

## Key Features
- Past booking display with grid layout
- Booking card integration with event details
- Pagination support with filter context
- Loading states with skeleton placeholders
- No results handling with customizable messaging
- Error message display and handling
- Customer authentication integration
- Event viewing and interaction capabilities
- Zoom integration for virtual events
- Cancellation handling (for eligible bookings)
- Live stream access for recorded content

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| perPage | Number | No | 9 | Number of bookings per page |
| hideIfNoResults | Boolean | No | false | Hide component completely when no bookings exist |
| eventMetrics | Object\|Boolean | No | false | Event metrics configuration for booking cards |
| group | String | No | undefined | Filter context group for pagination and filtering |

### Common Props
All common props from `@/config/common` are supported, including:
| Prop Name | Usage |
|-----------|-------|
| loadingItems | Number of skeleton items to show during loading |

### Common Functions
The component uses utilities from `useCommon`:
| Function | Usage |
|----------|-------|
| loadingItems | Provides skeleton loading item configuration |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| booking-cancelled | `booking: Object` | Emitted when a booking is successfully cancelled |
| event-viewed | `event: Object` | Emitted when an event is viewed |
| live-stream-watched | `booking: Object` | Emitted when live stream is accessed |
| zoom-joined | `booking: Object` | Emitted when Zoom call is joined |

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
<template #footer="{ loading, group }">
  <!-- Custom footer content (pagination by default) -->
</template>
```

### Error Messages Slot
```vue
<template #error-messages="{ genericErrors }">
  <!-- Custom error display -->
</template>
```

### No Results Slot
```vue
<template #no-results="{ type }">
  <!-- Custom no results message -->
</template>
```

### Slot Props
| Prop Name | Type | Description |
|-----------|------|-------------|
| loading | Boolean | Loading state information |
| group | String | Filter context group |
| genericErrors | Array | Generic error messages |
| type | String | Booking type ('past') |

## States
The component has multiple states in order of priority:
1. **Unauthenticated State** - Component hidden when no customer
2. **Loading State** - Shows skeleton booking cards during data fetch
3. **Error State** - Displays error messages
4. **No Results State** - Shows no bookings message (can be hidden)
5. **Bookings Display State** - Normal grid booking display

## Booking Management
The component provides comprehensive booking interaction:

### Event Viewing
- **Navigation**: Direct navigation to event detail pages
- **URL Generation**: Uses configured event URL templates
- **Integration**: Seamless integration with event system

### Live Stream Access
- **Video URLs**: Direct navigation to video streaming pages
- **Content Access**: Integration with video delivery system
- **User Experience**: Smooth transition to video content

### Zoom Integration
- **Meeting Access**: Opens Zoom meetings in new windows
- **Virtual Events**: Support for virtual event participation
- **Security**: Secure handling of Zoom URLs

### Booking Cancellation
- **Eligibility**: Handles cancellation eligibility checks
- **Processing**: Manages cancellation request processing
- **Feedback**: Provides user feedback for cancellation actions

## Filter Context Integration
The component integrates with the filter context system:
- **Pagination**: Coordinated pagination with filtering
- **Data Loading**: Filter-aware data loading
- **State Management**: Synchronized filter and pagination state
- **Real-time Updates**: Dynamic updates based on filter changes

## Grid Layout
The component uses a responsive grid layout:
- **Card Display**: Grid-based booking card arrangement
- **Responsive Design**: Adapts to different screen sizes
- **Spacing**: Consistent spacing and alignment
- **Loading States**: Skeleton cards maintain grid structure

## Internationalization

The `PastBookings` component uses translation keys for booking management interface:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `booking.you_have_no_type_bookings` | No results message with parameterized type | Empty state for past bookings |
| `booking.past_bookings_type` | Type parameter for no results message | Booking category identifier |

### Implementation Examples

```vue
<!-- No results state -->
<div v-if="!loading && !bookings.length" class="_c-noresults">
    <slot name="no-results" :type="type">
        {{ $t('booking.you_have_no_type_bookings', { value: $t('booking.past_bookings_type') }) }}
    </slot>
</div>
```

### Child Component Integration

The component renders `codex-booking-card` components which have their own extensive translation requirements:

```vue
<codex-booking-card 
    v-for="(booking, index) in bookings"
    :key="booking.id"
    :booking="booking"
    :type="'past'"
    @view-event="handleViewEvent"
    @cancel="handleCancel"
    @watch="handleWatch"
    @join-zoom="handleJoinZoom"
/>
```

### Notes
- Minimal translation requirements as main interface is delegated to child components
- Uses parameterized translations with booking type
- Error handling messages come from `useBookings` composable
- Pagination component integration for "Load more" functionality
- Child `BookingCard` components handle detailed booking translations

## Examples

### Basic Implementation
```vue
<codex-past-bookings />
```

### With Custom Page Size and Group
```vue
<codex-past-bookings
  :per-page="12"
  :group="'customer-past-bookings'"
  :event-metrics="true"
/>
```

### Hide When No Results
```vue
<codex-past-bookings
  :hide-if-no-results="true"
/>
```

### With Custom Header
```vue
<codex-past-bookings>
  <template #header>
    <div class="past-bookings-header">
      <h2>Your Class History</h2>
      <p>Review your completed classes and access recorded content</p>
      <div class="stats-summary">
        <span>Total Classes: {{ totalBookings }}</span>
        <span>This Month: {{ thisMonthBookings }}</span>
      </div>
    </div>
  </template>
</codex-past-bookings>
```

### Custom No Results State
```vue
<codex-past-bookings>
  <template #no-results="{ type }">
    <div class="custom-no-bookings">
      <h3>No Past Bookings</h3>
      <p>You haven't attended any classes yet. Start your fitness journey today!</p>
      <codex-button 
        @click="navigateToTimetable"
        :default-text="'Browse Classes'"
        variant="primary"
      />
    </div>
  </template>
</codex-past-bookings>
```

### Custom Error Handling
```vue
<codex-past-bookings>
  <template #error-messages="{ genericErrors }">
    <div class="custom-error-display">
      <i class="error-icon" />
      <div>
        <h4>Unable to Load Past Bookings</h4>
        <ul>
          <li v-for="error in genericErrors" :key="error">
            {{ error }}
          </li>
        </ul>
        <button @click="retryLoadBookings">Try Again</button>
      </div>
    </div>
  </template>
</codex-past-bookings>
```

### With Event Handlers
```vue
<codex-past-bookings
  @booking-cancelled="handleBookingCancelled"
  @event-viewed="trackEventView"
  @live-stream-watched="trackStreamAccess"
  @zoom-joined="trackZoomJoin"
/>
```

### Custom Footer with Statistics
```vue
<codex-past-bookings>
  <template #footer="{ loading, group }">
    <div class="past-bookings-footer">
      <div class="booking-stats">
        <span>Total Past Bookings: {{ pastBookingCount }}</span>
        <span>Favorite Class Type: {{ favoriteClassType }}</span>
        <span>Total Classes This Year: {{ yearlyCount }}</span>
      </div>
      <codex-pagination :loading="loading" :group="group" />
    </div>
  </template>
</codex-past-bookings>
```

### With Metrics Display
```vue
<codex-past-bookings
  :event-metrics="{
    showCadence: true,
    showWatts: true,
    showDistance: true,
    showCalories: true
  }"
/>
```

### In Account Dashboard
```vue
<div class="account-dashboard">
  <div class="booking-sections">
    <div class="upcoming-section">
      <h3>Upcoming Classes</h3>
      <codex-bookings :type="'upcoming'" />
    </div>
    
    <div class="past-section">
      <h3>Class History</h3>
      <codex-past-bookings 
        :per-page="6"
        :group="'dashboard-past'"
      />
    </div>
  </div>
</div>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-bookings-card`: Bookings-specific card styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-grid`: Grid layout container
- `_c-noresults`: No results state styling

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Implement proper loading states during booking fetch
- Provide clear booking interaction options
- Use appropriate page sizes for performance
- Handle booking state changes with proper feedback
- Implement error retry mechanisms
- Use pagination for large booking datasets
- Provide clear historical booking organization

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing loading states during data fetch
- Forgetting to handle booking state changes
- Not providing clear interaction feedback
- Missing error handling for failed operations
- Not updating booking list after actions
- Insufficient handling of empty states
- Not tracking booking interaction states

### Accessibility Considerations
- Ensure booking cards are keyboard navigable
- Provide clear labels for booking actions
- Use appropriate ARIA attributes for dynamic content
- Ensure grid layout is accessible
- Provide screen reader friendly booking information
- Include proper focus management
- Use semantic HTML for booking data
- Ensure adequate color contrast for all elements

### Error Handling
- Display clear error messages for failed loads
- Provide retry mechanisms for network failures
- Handle booking action errors gracefully
- Show validation errors for actions
- Clear error states when operations succeed
- Handle authentication errors appropriately
- Provide fallback states for partial failures

### State Management
- Track booking state properly
- Handle customer authentication changes
- Manage loading states consistently
- Update booking list after actions
- Handle filter state transitions
- Manage pagination and filter coordination
- Coordinate with booking management systems

### Performance Considerations
- Implement virtual scrolling for large booking lists
- Lazy load booking card components
- Optimize grid layout performance
- Handle large action operations efficiently
- Consider pagination for better performance
- Implement proper cleanup for event listeners
- Cache booking data appropriately

### Booking Management
- Validate booking actions before execution
- Provide clear action confirmation
- Handle partial action failures
- Update UI state after successful actions
- Provide action progress feedback
- Handle action cancellation properly
- Clear action state appropriately

### Grid Layout Management
- Ensure responsive grid behavior
- Handle variable card sizes appropriately
- Maintain consistent spacing
- Optimize for different screen sizes
- Handle grid reflow efficiently
- Provide clear visual hierarchy
- Ensure accessibility in grid navigation

### Filter Context Integration
- Use appropriate group names for filtering
- Handle filter state changes properly
- Coordinate pagination with filters
- Update bookings when filters change
- Handle filter reset scenarios
- Provide clear filter feedback
- Maintain filter state across navigation

### Event Integration
- Handle event viewing navigation properly
- Manage live stream access securely
- Coordinate with video delivery systems
- Handle Zoom integration appropriately
- Manage event cancellation scenarios
- Coordinate with event booking systems
- Track event interaction analytics

### Metrics Integration
- Display booking metrics when available
- Handle missing metrics data gracefully
- Provide meaningful metric displays
- Consider metric privacy settings
- Handle metric calculation errors
- Format metrics consistently
- Track metric viewing analytics

## Component Registration
The component is registered as `codex-past-bookings` in the application. 