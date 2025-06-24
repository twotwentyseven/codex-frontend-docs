# Bookings Component

## Overview
The Bookings component provides a comprehensive interface for managing customer bookings, including display, cancellation, and booking actions. It features grouped booking displays by date, cancellation modals with refund information, event viewing capabilities, and integration with video and Zoom functionality.

## Basic Usage
```vue
<codex-bookings
  :per-page="5"
  :type="'upcoming'"
  :hide-if-no-results="false"
  :event-metrics="false"
  :group="'customer-bookings'"
/>
```

## Key Features
- Date-grouped booking display
- Booking cancellation with confirmation modal
- Refund status indication
- Event viewing and navigation
- Video watching functionality
- Zoom meeting integration
- Loading states with skeleton placeholders
- No results handling with booking CTA
- Error message display
- Pagination support with filter context
- Customer authentication integration
- Event metrics integration

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| perPage | Number | No | 2 | Number of bookings per page |
| type | String | No | 'upcoming' | Type of bookings to display (upcoming, past, etc.) |
| hideIfNoResults | Boolean | No | false | Hide component when no bookings are available |
| eventMetrics | Object\|Boolean | No | false | Event metrics data for booking cards |
| group | String | No | undefined | Filter context group for pagination and filtering |

### Common Props
All common props from `@/config/common` are supported, including:
| Prop Name | Usage |
|-----------|-------|
| enableBorder | Adds border styling to booking cards |
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
| video-watched | `video: Object` | Emitted when a video is accessed |
| zoom-joined | `booking: Object` | Emitted when a Zoom meeting is joined |

## Slots

### Header Slot
```vue
<template #header="{ error, genericErrors }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ error, genericErrors }">
  <!-- Custom content and booking display -->
</template>
```

### Footer Slot
```vue
<template #footer="{ error, genericErrors }">
  <!-- Custom footer content (pagination and CTA by default) -->
</template>
```

### Error Messages Slot
```vue
<template #error-messages="{ genericErrors }">
  <!-- Custom error display -->
</template>
```

### Slot Props
| Prop Name | Type | Description |
|-----------|------|-------------|
| error | Object | Current error state |
| genericErrors | Array | Generic error messages |

## States
The component has multiple states in order of priority:
1. **Unauthenticated State** - Component hidden when no customer
2. **Loading State** - Shows skeleton booking cards during data fetch
3. **Error State** - Displays error messages
4. **No Results State** - Shows no bookings message with booking CTA
5. **Bookings Display State** - Normal grouped booking display
6. **Cancellation Modal State** - Booking cancellation confirmation and processing
7. **Success State** - Cancellation confirmation with auto-close

## Booking Actions
The component supports various booking actions:
- **View Event**: Navigate to event details page
- **Cancel Booking**: Cancel with confirmation modal and refund information
- **Watch Video**: Access associated video content
- **Join Zoom**: Open Zoom meeting in new tab

## Cancellation Flow
- **Selection**: User initiates cancellation from booking card
- **Confirmation**: Modal displays refund information and confirmation options
- **Processing**: Cancellation request processing with loading states
- **Success**: Success confirmation with auto-close and state reset

## Refund Information
The component displays different refund messages based on booking status:
- **Refundable**: "A refund will be given"
- **Maybe Refundable**: "A refund may be given"
- **Non-refundable**: "No refund will be given"

## Internationalization

The component uses the following translation keys:

### Core Interface Messages
| Key | Usage |
|-----|-------|
| `booking.you_have_no_type_bookings` | No bookings message with type parameter |
| `booking.book_now` | Book now CTA button in footer |

### Booking Cancellation Modal
| Key | Usage |
|-----|-------|
| `booking.cancel_title` | Cancellation modal title |
| `booking.cancel_success_title` | Cancellation success title |
| `booking.back_to_account` | Return to account button |
| `booking.go_back` | Cancel action button |
| `booking.confirm_cancel` | Confirm cancellation button |

### Refund Information Messages
| Key | Usage |
|-----|-------|
| `booking.refund_may_be_given` | Maybe refundable booking message |
| `booking.refund_will_be_given` | Refundable booking message |
| `booking.no_refund_will_be_given` | Non-refundable booking message |

### Translation Usage Examples
```vue
<!-- No bookings message with type parameter -->
<div v-if="!loading && !bookings.length" class="_c-noresults">
  {{ $t('booking.you_have_no_type_bookings', { value: type }) }}
</div>

<!-- Book now CTA button -->
<codex-anchor 
  href="window.codex.urls.booking_url" 
  :defaultText="$t('booking.book_now')" 
  className="_c-btn _c-primary-btn"
/>

<!-- Cancellation modal title -->
<div class="_c-cancellation-confirmation-title _c-title">
  {{ $t('booking.cancel_title') }}
</div>

<!-- Success title -->
<codex-title 
  :tag="titleTag" 
  :content="$t('booking.cancel_success_title')" 
  className="_c-success-title"
/>

<!-- Refund information messages -->
<div class="_c-desc _c-cancellation-confirmation-body">
  <template v-if="selectedBooking?.is_refundable === 'maybe'">
    {{ $t('booking.refund_may_be_given') }}
  </template>
  <template v-else-if="selectedBooking?.is_refundable === true">
    {{ $t('booking.refund_will_be_given') }}
  </template>
  <template v-else>
    {{ $t('booking.no_refund_will_be_given') }}
  </template>
</div>

<!-- Action buttons -->
<codex-button 
  variant="secondary"
  :defaultText="$t('booking.go_back')"
  @click="close"  
/>
<codex-button 
  variant="primary"
  :defaultText="$t('booking.confirm_cancel')"
  @click="handleCancel"
/>
<codex-button 
  variant="primary"
  :defaultText="$t('booking.back_to_account')"
  @click="close"
/>
```

### Translation Notes

#### Booking Management Flow
The component provides comprehensive translation support for:
- Type-specific empty state messaging (upcoming, past, etc.)
- Booking cancellation confirmation flow
- Refund policy communication
- Navigation and action button labels

#### Dynamic Type Translation
The no bookings message uses a parameter for booking type:
```javascript
// Supports different booking types
$t('booking.you_have_no_type_bookings', { value: 'upcoming' })
$t('booking.you_have_no_type_bookings', { value: 'past' })
$t('booking.you_have_no_type_bookings', { value: 'cancelled' })
```

#### Refund Policy Communication
The component provides clear refund information through different message types:
- **Maybe Refundable**: Indicates refund depends on circumstances
- **Refundable**: Confirms refund will be processed
- **Non-refundable**: Clearly states no refund available

#### Modal Interface Translation
The cancellation modal uses translations for:
- Modal titles and headers
- Confirmation and success messages
- Action button labels for different flow states
- Navigation options

#### Integration with Child Components
The component integrates with `codex-booking-card` components that handle their own translations for:
- Event names and descriptions
- Date and time formatting
- Booking status indicators
- Action buttons (cancel, watch, join Zoom, etc.)
- Event metrics and attendance information

#### Call-to-Action Integration
When no bookings exist, the component provides:
- Translated empty state message
- Book now button with translation
- Integration with booking URLs and navigation

## Examples

### Basic Implementation
```vue
<codex-bookings />
```

### Upcoming Bookings with Custom Page Size
```vue
<codex-bookings
  :type="'upcoming'"
  :per-page="10"
  :event-metrics="eventMetricsData"
/>
```

### Past Bookings with Custom Header
```vue
<codex-bookings
  :type="'past'"
  :hide-if-no-results="true"
>
  <template #header>
    <div class="bookings-header">
      <h2>Your Past Bookings</h2>
      <p>Review your completed bookings and access recordings</p>
    </div>
  </template>
</codex-bookings>
```

### Custom No Results State
```vue
<codex-bookings>
  <template #content="{ error, genericErrors }">
    <div v-if="!bookings.length && !loading" class="custom-no-results">
      <h3>No Upcoming Bookings</h3>
      <p>You don't have any upcoming bookings. Browse our events to make a booking!</p>
      <codex-button 
        @click="navigateToEvents"
        :default-text="'Browse Events'"
        variant="primary"
      />
    </div>
    <template v-else>
      <!-- Default grouped bookings display -->
    </template>
  </template>
</codex-bookings>
```

### With Event Metrics and Custom Actions
```vue
<codex-bookings
  :event-metrics="{
    attendees: true,
    rating: true,
    duration: true
  }"
  @booking-cancelled="handleBookingCancelled"
  @event-viewed="trackEventView"
  @video-watched="trackVideoWatch"
  @zoom-joined="trackZoomJoin"
/>
```

### Custom Footer with Statistics
```vue
<codex-bookings>
  <template #footer="{ error, genericErrors }">
    <div class="bookings-footer">
      <div class="booking-stats">
        <span>Total Bookings: {{ bookings.length }}</span>
        <span>This Month: {{ thisMonthCount }}</span>
      </div>
      <codex-pagination :group="group" />
    </div>
  </template>
</codex-bookings>
```

### Custom Cancellation Handling
```vue
<codex-bookings
  @booking-cancelled="handleCustomCancellation"
>
  <template #content="{ error, genericErrors }">
    <!-- Custom booking display with additional cancellation options -->
    <div v-for="(bookingsGroup, date) in groupedBookings" :key="date">
      <h3>{{ formatDate(date) }}</h3>
      <codex-booking-card 
        v-for="booking in bookingsGroup"
        :key="booking.id"
        :booking="booking"
        @cancel="showCustomCancelModal"
      />
    </div>
  </template>
</codex-bookings>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-bookings-card`: Bookings-specific card styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-booking-group`: Date group container
- `_c-date-title`: Date group title styling
- `_c-column`: Booking cards column layout
- `_c-btn-container`: Button container
- `_c-w-fill`: Full width utility
- `_c-noresults`: No results state styling
- `_c-cancellation-confirmation`: Cancellation modal styling
- `_c-cancellation-confirmation-title`: Modal title
- `_c-cancellation-confirmation-body`: Modal body text
- `_c-success-container`: Success state container
- `_c-success-icon`: Success icon container
- `_c-success-title`: Success message title

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Implement proper loading states during booking fetch
- Provide clear cancellation confirmation with refund information
- Use appropriate booking types for different contexts
- Handle booking actions with proper navigation
- Implement error retry mechanisms
- Group bookings by date for better organization

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing loading states during data fetch
- Forgetting to handle cancellation confirmation
- Not providing clear refund information
- Missing error handling for failed cancellations
- Not updating booking list after successful cancellations
- Insufficient handling of Zoom integration errors

### Accessibility Considerations
- Ensure booking cards are keyboard navigable
- Provide clear labels for booking actions
- Use appropriate ARIA attributes for grouped content
- Ensure modal dialogs are accessible
- Provide screen reader friendly date grouping
- Include proper focus management for modals
- Use semantic HTML for booking information

### Error Handling
- Display clear error messages for failed loads
- Provide retry mechanisms for network failures
- Handle cancellation errors gracefully
- Show validation errors for invalid cancellations
- Clear error states when operations succeed
- Handle authentication errors appropriately
- Provide fallback states for partial failures

### State Management
- Track booking state properly
- Handle customer authentication changes
- Manage loading states consistently
- Update booking list after cancellations
- Handle modal state transitions
- Manage cancellation success states
- Coordinate with pagination and filter state

### Performance Considerations
- Implement virtual scrolling for large booking lists
- Lazy load booking card components
- Optimize grouped rendering performance
- Handle large cancellation operations efficiently
- Consider pagination for better performance
- Implement proper cleanup for event listeners
- Debounce action handlers where appropriate

### Cancellation Management
- Provide clear cancellation confirmation
- Display accurate refund information
- Handle partial cancellation failures
- Update UI state after successful cancellations
- Provide cancellation progress feedback
- Handle cancellation policy validation
- Clear cancellation state appropriately

### Event Integration
- Handle event navigation properly
- Integrate with event metrics system
- Provide fallback for missing event data
- Handle video access appropriately
- Manage Zoom integration securely
- Track user interactions properly
- Handle external navigation gracefully

### Date Grouping
- Format dates consistently
- Handle timezone considerations
- Group bookings logically by date
- Provide clear date headers
- Handle empty date groups
- Sort bookings within groups appropriately
- Consider locale-specific date formatting

## Component Registration
The component is registered as `codex-bookings` in the application. 