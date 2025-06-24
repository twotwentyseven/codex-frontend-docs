# BookingsCompleted Component

## Overview
The BookingsCompleted component provides a dashboard widget interface for displaying completed booking statistics with milestone tracking and progress visualization. It features circular progress charts, milestone progression, completion counters, and navigation to detailed booking history for effective booking completion tracking.

## Basic Usage
```vue
<codex-bookings-completed
  :type="'completed'"
  :default-target-tab="'history'"
  :milestones="[5, 10, 20, 50, 100]"
/>
```

## Key Features
- Booking completion statistics display
- Circular progress chart visualization
- Milestone tracking and progression
- Progress to next milestone calculation
- Completion counter with flexible messaging
- Navigation to detailed booking views
- Loading states with skeleton placeholders
- Error handling and display
- No results state management
- Customer authentication integration
- Responsive design with mobile adaptations

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| type | String | No | 'completed' | Type of bookings to track |
| hideIfNoResults | Boolean | No | false | Hide component when no bookings exist |
| defaultTargetTab | String | No | 'history' | Target tab when navigating to detailed view |
| milestones | Array | No | [5, 10, 20, 50, 100] | Milestone progression array |

### Milestone Validation
The milestones prop includes validation:
- All values must be numbers
- All values must be greater than 0
- Array is sorted automatically for progression logic

### Common Props
All common props from `@/config/common` are supported.

### Common Functions
The component uses utilities from `useCommon`:
| Function | Usage |
|----------|-------|
| handleClick | Handles navigation clicks with proper event handling |
| isMobile | Provides mobile device detection for responsive layout |

## Events
Currently, the BookingsCompleted component does not emit custom events. It serves as a dashboard widget with internal navigation.

## Slots

### Header Slot
```vue
<template #header="{ type, error, genericErrors }">
  <!-- Custom header content -->
</template>
```

### Footer Slot
```vue
<template #footer="{ customer, loading, error, genericErrors }">
  <!-- Custom footer content -->
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

## Customer Statistics Integration
The component relies on customer statistics:
- **Total Bookings**: Uses `customer.stats.total_bookings`
- **Real-time Updates**: Automatically updates when customer data changes
- **Authentication**: Requires authenticated customer for display

## Milestone System
The component includes a sophisticated milestone tracking system:

### Milestone Progression
- **Sequential Tracking**: Finds next unachieved milestone
- **Progress Calculation**: Calculates percentage to next milestone
- **Maximum Achievement**: Handles cases where all milestones are reached
- **Visual Feedback**: Circular progress chart shows advancement

### Default Milestones
```javascript
[5, 10, 20, 50, 100]  // Classes completed milestones
```

### Progress Visualization
- **Circular Chart**: SVG-based progress visualization
- **Percentage Display**: Shows completion percentage to next milestone
- **Current Count**: Displays current booking total
- **Target Display**: Shows next milestone target

## Chart Configuration
The circular progress chart features:
- **CSS Variables**: Customizable colors via CSS custom properties
- **Responsive Design**: Adapts to different container sizes
- **Animation**: Smooth progress transitions
- **Accessibility**: Screen reader friendly with proper labels

## States Management
The component handles multiple states:

### Loading State
- **Skeleton Display**: Shows structured placeholder layout
- **Smooth Transitions**: Seamless transition to loaded state

### Error State
- **Error Display**: Shows error messages with retry options
- **Graceful Degradation**: Maintains layout structure during errors

### No Results State
- **Optional Display**: Can be hidden based on `hideIfNoResults`
- **Customizable Messaging**: Slot-based no results content

### Success State
- **Statistics Display**: Shows completion statistics and progress
- **Interactive Elements**: Provides navigation to detailed views

## Responsive Design
The component adapts to different screen sizes:
- **Mobile Layout**: Stacked layout for smaller screens
- **Desktop Layout**: Horizontal layout with spacing
- **Conditional Wrapper**: Uses `codex-conditional-wrapper` for responsive behavior

## Internationalization

The `BookingsCompleted` component uses translation keys for milestone tracking and completed bookings overview:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `booking.classes_completed` | Main title for completed bookings section | Dashboard widget header |
| `booking.you_have_no_type_bookings` | No results message with type parameter | When customer has no completed bookings |
| `booking.next_milestone` | Progress message to next milestone with count parameter | "X more classes to your next milestone" |
| `booking.max_milestone_reached` | Message when highest milestone achieved | Customer has reached maximum milestone |
| `booking.view_all` | Button text to view all booking history | Navigation to detailed history |

### Implementation Examples

```vue
<!-- Main title -->
<div class="_c-title">
    {{ $t('booking.classes_completed') }}
</div>

<!-- No results state -->
<div v-if="!customer.stats.total_bookings" class="_c-noresults">
    {{ $t('booking.you_have_no_type_bookings', { value: type }) }}
</div>

<!-- Milestone progress -->
<div class="_c-milestone-text" v-if="nextMilestone !== 'MAX'">
    {{ $t('booking.next_milestone', { count: nextMilestone - customer.stats.total_bookings }) }}
</div>

<!-- Maximum milestone reached -->
<div class="_c-milestone-text" v-else>
    {{ $t('booking.max_milestone_reached') }}
</div>

<!-- View all button -->
<codex-button 
    :defaultText="$t('booking.view_all')"
    @click="handleClick">
</codex-button>
```

### Milestone Configuration

The component supports configurable milestones:

```vue
milestones: {
    type: Array,
    default: () => [5, 10, 20, 50, 100],
    validator: (value) => value.every(milestone => typeof milestone === 'number' && milestone > 0)
}
```

### Progress Calculations

```javascript
const nextMilestone = computed(() => {
    const currentCount = customer.value.stats.total_bookings;
    return props.milestones.find(milestone => milestone > currentCount) || 'MAX';
});
```

### Notes
- Parameterized translations for milestone progress with dynamic counts
- Milestone values are numeric and don't require translation
- Integration with customer statistics from `useCustomer` composable
- Responsive design with conditional layout for mobile vs desktop
- Progress visualization through CSS custom properties

## Examples

### Basic Implementation
```vue
<codex-bookings-completed />
```

### Custom Milestones
```vue
<codex-bookings-completed
  :milestones="[3, 7, 15, 30, 60, 120]"
  :default-target-tab="'completed'"
/>
```

### Custom Header with Additional Stats
```vue
<codex-bookings-completed>
  <template #header="{ type, error, genericErrors }">
    <div class="completion-header">
      <h3>Your Fitness Journey</h3>
      <div class="stats-grid">
        <div class="stat-item">
          <span class="value">{{ customer.stats.total_bookings }}</span>
          <span class="label">Classes Completed</span>
        </div>
        <div class="stat-item">
          <span class="value">{{ customer.stats.this_month_bookings }}</span>
          <span class="label">This Month</span>
        </div>
        <div class="stat-item">
          <span class="value">{{ streakDays }}</span>
          <span class="label">Day Streak</span>
        </div>
      </div>
    </div>
  </template>
</codex-bookings-completed>
```

### Custom Footer with Actions
```vue
<codex-bookings-completed>
  <template #footer="{ customer, loading, error, genericErrors }">
    <div class="completion-actions">
      <div class="achievement-badges">
        <span v-for="milestone in achievedMilestones" :key="milestone" class="badge">
          {{ milestone }} Classes
        </span>
      </div>
      
      <div class="action-buttons">
        <codex-button 
          :default-text="'View History'"
          @click="navigateToHistory"
        />
        <codex-button 
          :default-text="'Book Next Class'"
          @click="navigateToTimetable"
          variant="primary"
        />
      </div>
    </div>
  </template>
</codex-bookings-completed>
```

### Custom No Results State
```vue
<codex-bookings-completed>
  <template #no-results="{ type }">
    <div class="no-completions">
      <div class="motivational-message">
        <h4>Start Your Fitness Journey!</h4>
        <p>Complete your first class to begin tracking your progress.</p>
        <div class="first-milestone">
          <span>First milestone: {{ milestones[0] }} classes</span>
        </div>
      </div>
      <codex-button 
        :default-text="'Browse Classes'"
        @click="navigateToTimetable"
        variant="primary"
      />
    </div>
  </template>
</codex-bookings-completed>
```

### With Progress Tracking
```vue
<codex-bookings-completed
  :milestones="customMilestones"
>
  <template #header="{ type }">
    <div class="progress-header">
      <h3>Progress Tracker</h3>
      <div class="milestone-preview">
        <span>Next Goal: {{ nextMilestone }} classes</span>
        <span>{{ remainingClasses }} classes to go!</span>
      </div>
    </div>
  </template>
</codex-bookings-completed>

<script setup>
const customMilestones = [5, 10, 20, 50, 100, 200, 500]

const nextMilestone = computed(() => {
  return milestones.find(m => m > customer.value?.stats?.total_bookings) || 'MAX'
})

const remainingClasses = computed(() => {
  if (nextMilestone.value === 'MAX') return 0
  return nextMilestone.value - (customer.value?.stats?.total_bookings || 0)
})
</script>
```

### Dashboard Integration
```vue
<div class="account-dashboard">
  <div class="stats-widgets">
    <codex-bookings-completed 
      class="completion-widget"
      :hide-if-no-results="false"
    />
    
    <codex-available-credits 
      class="credits-widget"
    />
    
    <div class="quick-actions-widget">
      <!-- Other dashboard widgets -->
    </div>
  </div>
</div>
```

### Gamification Integration
```vue
<codex-bookings-completed>
  <template #header="{ type }">
    <div class="gamification-header">
      <h3>Fitness Level</h3>
      <div class="level-info">
        <span class="current-level">Level {{ getCurrentLevel(customer.stats.total_bookings) }}</span>
        <span class="next-level">{{ getNextLevelName(customer.stats.total_bookings) }}</span>
      </div>
    </div>
  </template>
  
  <template #footer="{ customer }">
    <div class="achievement-footer">
      <div class="rewards-earned">
        <span>Rewards Earned: {{ calculateRewards(customer.stats.total_bookings) }}</span>
      </div>
      <div class="share-achievement">
        <button @click="shareProgress">Share Progress</button>
      </div>
    </div>
  </template>
</codex-bookings-completed>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-grow`: Flex grow utility
- `_c-bookings-completed-card`: Bookings completed specific styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-justify-center`: Center justify utility
- `_c-footer`: Footer section
- `_c-row`: Row flexbox layout
- `_c-items-center`: Center align flex items
- `_c-wrap`: Flex wrap utility
- `_c-nowrap_lg`: No wrap on large screens
- `_c-pie`: Circular progress chart styling
- `animate`: Animation class for chart
- `_c-pie-lg-text`: Large text inside pie chart
- `_c-column`: Column layout utility
- `_c-gap-lg`: Large gap utility
- `_c-grow`: Flex grow utility
- `_c-remaining-credits-container`: Credits container styling
- `_c-gap-sm`: Small gap utility
- `_c-title`: Title styling
- `_c-milestone-text`: Milestone text styling
- `_c-mb-sm`: Small margin bottom utility
- `_c-noresults`: No results state styling
- `_c-btn-container`: Button container styling
- `_c-fill`: Fill container utility

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Provide meaningful milestone progressions
- Use appropriate chart sizing for containers
- Handle loading states during customer data fetch
- Implement proper navigation to detailed views
- Provide clear progress feedback
- Use responsive design principles
- Track milestone achievements for engagement

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing loading states during data fetch
- Setting unrealistic milestone progressions
- Not providing clear navigation paths
- Missing responsive considerations
- Insufficient handling of edge cases (no bookings)
- Not optimizing chart performance
- Missing accessibility considerations

### Accessibility Considerations
- Provide meaningful chart descriptions
- Use appropriate ARIA attributes for progress indicators
- Ensure keyboard navigation for interactive elements
- Provide screen reader friendly progress information
- Include proper semantic HTML structure
- Ensure adequate color contrast for chart elements
- Handle focus management appropriately
- Use semantic markup for statistics

### Error Handling
- Handle missing customer statistics gracefully
- Provide fallbacks for malformed data
- Display meaningful error messages
- Clear error states when data updates
- Handle chart rendering errors
- Provide retry mechanisms for failed loads
- Handle edge cases in milestone calculations

### State Management
- Track customer statistics changes
- Handle authentication state changes
- Manage loading states consistently
- Update charts when data changes
- Handle milestone progression dynamically
- Coordinate with customer data updates
- Manage component lifecycle properly

### Performance Considerations
- Optimize chart rendering performance
- Minimize re-calculations of progress
- Handle large milestone arrays efficiently
- Implement proper cleanup for chart animations
- Cache milestone calculations appropriately
- Optimize responsive layout transitions
- Handle frequent statistics updates efficiently

### Chart Optimization
- Use CSS transforms for smooth animations
- Optimize SVG rendering performance
- Handle chart resizing efficiently
- Minimize chart re-renders
- Use efficient progress calculations
- Implement proper chart accessibility
- Handle edge cases in chart display

### Milestone Management
- Design meaningful milestone progressions
- Validate milestone arrays properly
- Handle milestone achievement detection
- Provide appropriate milestone feedback
- Consider different user fitness levels
- Track milestone engagement metrics
- Handle milestone progression edge cases

### Navigation Integration
- Provide clear navigation paths
- Handle navigation state properly
- Coordinate with routing systems
- Track navigation analytics
- Handle navigation errors gracefully
- Provide appropriate navigation feedback
- Ensure navigation accessibility

## Component Registration
The component is registered as `codex-bookings-completed` in the application. 