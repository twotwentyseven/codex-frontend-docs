# Waitlists Component

## Overview
The Waitlists component provides a comprehensive interface for displaying and managing customer waitlists with date grouping, cancellation functionality, and pagination support. It features grouped waitlist displays by date, leave waitlist modals, loading states, error handling, and flexible slot-based customization for waitlist management.

## Basic Usage
```vue
<codex-waitlists
  :per-page="10"
  :group="'customer-waitlists'"
/>
```

## Key Features
- Date-grouped waitlist display
- Waitlist card integration with event details
- Leave waitlist functionality with confirmation modal
- Loading states with skeleton placeholders
- No results handling with optional hiding
- Error message display
- Pagination support with filter context
- Customer authentication integration
- Success feedback for waitlist actions
- Responsive layout with mobile adaptations

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| perPage | Number | No | 10 | Number of waitlists per page |
| hideIfNoResults | Boolean | No | false | Hide component completely when no waitlists exist |
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
| waitlist-left | `waitlist: Object` | Emitted when a waitlist is successfully left |
| waitlists-loaded | `waitlists: Array` | Emitted when waitlists are loaded |
| waitlist-cancelled | `waitlist: Object` | Emitted when waitlist cancellation is confirmed |

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
<template #footer="{ pagination }">
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
<template #no-results>
  <!-- Custom no results message -->
</template>
```

### Slot Props
| Prop Name | Type | Description |
|-----------|------|-------------|
| pagination | Object | Pagination state information |
| genericErrors | Array | Generic error messages |

## States
The component has multiple states in order of priority:
1. **Unauthenticated State** - Component hidden when no customer
2. **Loading State** - Shows skeleton waitlist cards during data fetch
3. **Error State** - Displays error messages
4. **No Results State** - Shows no waitlists message (can be hidden)
5. **Waitlists Display State** - Normal grouped waitlist display
6. **Leave Modal State** - Waitlist cancellation confirmation modal
7. **Success State** - Success feedback after waitlist actions

## Waitlist Management
The component provides comprehensive waitlist management:

### Leave Waitlist Flow
- **Initiate**: User clicks leave on waitlist card
- **Modal**: Confirmation modal opens with cancellation details
- **Confirm**: User confirms waitlist cancellation
- **Processing**: Shows processing state during API call
- **Success**: Displays success message and updates list
- **Cleanup**: Removes waitlist from display

### Date Grouping
- **Automatic Grouping**: Waitlists are grouped by event date
- **Date Formatting**: Uses formatted date headers for groups
- **Chronological Order**: Groups are displayed in chronological order
- **Group Headers**: Clear date headers for each group

## Modal Management
The cancellation modal provides:
- **Confirmation Interface**: Clear cancellation confirmation
- **Success Feedback**: Success state with confirmation message
- **Processing States**: Loading indicators during cancellation
- **Error Handling**: Error display and retry options
- **Auto-close**: Automatic modal closure after success

## Internationalization

The component uses the following translation keys:

### Core Interface Messages
| Key | Usage |
|-----|-------|
| `account.you_have_no_waitlists` | No waitlists available message |

### Waitlist Cancellation Modal
| Key | Usage |
|-----|-------|
| `waitlist.cancel_title` | Cancellation modal title |
| `waitlist.cancel_description` | Cancellation confirmation text |
| `waitlist.cancel_success_title` | Success message title |
| `waitlist.leave_waitlist` | Leave waitlist button text |
| `waitlist.go_back` | Go back button text |
| `waitlist.back_to_account` | Back to account button text |

### Translation Usage Examples
```vue
<!-- No waitlists message -->
<div v-if="!loading && !waitlists.length" class="_c-noresults">
  <slot name="no-results">{{ $t('account.you_have_no_waitlists') }}</slot>
</div>

<!-- Cancellation modal title -->
<div class="_c-cancellation-confirmation-title _c-title">
  {{ $t('waitlist.cancel_title') }}
</div>

<!-- Cancellation description -->
<codex-paragraph 
  :tag="'div'" 
  :content="$t('waitlist.cancel_description')" 
/>

<!-- Success title -->
<codex-title 
  :tag="titleTag" 
  :content="$t('waitlist.cancel_success_title')" 
  className="_c-success-title"
/>

<!-- Action buttons -->
<codex-button 
  variant="secondary"
  :defaultText="$t('waitlist.go_back')"
  @click="close"  
/>
<codex-button 
  variant="primary"
  :defaultText="$t('waitlist.leave_waitlist')"
  @click="handleLeave"
/>
<codex-button 
  variant="primary"
  :defaultText="$t('waitlist.back_to_account')"
  @click="close"
/>
```

### Translation Notes

#### Waitlist Management Flow
The component provides comprehensive translation support for the complete waitlist management flow:
- Empty state messaging when no waitlists exist
- Confirmation dialog for leaving waitlists
- Success feedback after waitlist actions
- Navigation and action button labels

#### Modal Interface
The cancellation modal uses translations for:
- Modal titles and headers
- Confirmation messages and descriptions
- Action button labels for different states
- Success confirmation messages

#### Integration with Child Components
The component integrates with `codex-waitlist-card` components that handle their own translations for:
- Event names and descriptions
- Date and time formatting
- Waitlist position information
- Event status indicators

## Examples

### Basic Implementation
```vue
<codex-waitlists />
```

### With Custom Page Size and Group
```vue
<codex-waitlists
  :per-page="15"
  :group="'active-waitlists'"
/>
```

### Hide When No Results
```vue
<codex-waitlists
  :hide-if-no-results="true"
/>
```

### With Custom Header
```vue
<codex-waitlists>
  <template #header>
    <div class="waitlists-header">
      <h2>Your Waitlists</h2>
      <p>Manage your event waitlists and view upcoming opportunities</p>
    </div>
  </template>
</codex-waitlists>
```

### Custom No Results State
```vue
<codex-waitlists>
  <template #no-results>
    <div class="custom-no-waitlists">
      <h3>No Active Waitlists</h3>
      <p>You're not currently on any waitlists. Browse our events to join waitlists for full classes!</p>
      <codex-button 
        @click="navigateToEvents"
        :default-text="'Browse Events'"
        variant="primary"
      />
    </div>
  </template>
</codex-waitlists>
```

### Custom Error Handling
```vue
<codex-waitlists>
  <template #error-messages="{ genericErrors }">
    <div class="custom-error-display">
      <i class="error-icon" />
      <div>
        <h4>Unable to Load Waitlists</h4>
        <ul>
          <li v-for="error in genericErrors" :key="error">
            {{ error }}
          </li>
        </ul>
        <button @click="retryLoadWaitlists">Try Again</button>
      </div>
    </div>
  </template>
</codex-waitlists>
```

### With Event Handlers
```vue
<codex-waitlists
  @waitlist-left="handleWaitlistLeft"
  @waitlists-loaded="trackWaitlistsLoaded"
  @waitlist-cancelled="updateWaitlistStats"
/>
```

### Custom Footer with Statistics
```vue
<codex-waitlists>
  <template #footer="{ pagination }">
    <div class="waitlists-footer">
      <div class="waitlist-stats">
        <span>Total Waitlists: {{ waitlistCount }}</span>
        <span>This Week: {{ thisWeekCount }}</span>
        <span>Next Available: {{ nextAvailableDate }}</span>
      </div>
      <codex-pagination :group="group" />
    </div>
  </template>
</codex-waitlists>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-waitlists`: Waitlists-specific card styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-waitlist-group`: Waitlist group container
- `_c-mb-sm`: Small margin bottom utility
- `_c-date-title`: Date group header styling
- `_c-text-bold`: Bold text utility
- `_c-desc`: Description text styling
- `_c-column`: Column layout utility
- `_c-noresults`: No results state styling
- `_c-cancellation-confirmation`: Modal confirmation styling
- `_c-success-container`: Success state container
- `_c-success-icon`: Success icon styling
- `_c-success-title`: Success title styling
- `_c-btn-container`: Button container styling
- `_c-fill`: Fill container utility

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Implement proper loading states during waitlist fetch
- Provide clear waitlist management options
- Use appropriate page sizes for performance
- Handle waitlist state changes with proper feedback
- Implement error retry mechanisms
- Use pagination for large waitlist datasets
- Provide clear date grouping for better organization

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing loading states during data fetch
- Forgetting to handle waitlist state changes
- Not providing clear cancellation confirmation
- Missing error handling for failed operations
- Not updating waitlist list after management actions
- Insufficient handling of date formatting
- Not tracking waitlist processing states

### Accessibility Considerations
- Ensure waitlist cards are keyboard navigable
- Provide clear labels for management actions
- Use appropriate ARIA attributes for dynamic content
- Ensure modal dialogs are accessible
- Provide screen reader friendly waitlist information
- Include proper focus management for modals
- Use semantic HTML for waitlist data
- Ensure adequate color contrast for date headers

### Error Handling
- Display clear error messages for failed loads
- Provide retry mechanisms for network failures
- Handle waitlist management errors gracefully
- Show validation errors for management actions
- Clear error states when operations succeed
- Handle authentication errors appropriately
- Provide fallback states for partial failures

### State Management
- Track waitlist state properly
- Handle customer authentication changes
- Manage loading states consistently
- Update waitlist list after management actions
- Handle modal state transitions
- Manage processing states for individual waitlists
- Coordinate with pagination and filter state

### Performance Considerations
- Implement virtual scrolling for large waitlist lists
- Lazy load waitlist card components
- Optimize date grouping performance
- Handle large management operations efficiently
- Consider pagination for better performance
- Implement proper cleanup for event listeners
- Cache waitlist data appropriately

### Waitlist Management
- Validate waitlist eligibility before actions
- Provide clear management confirmation
- Handle partial management failures
- Update UI state after successful actions
- Provide management progress feedback
- Handle management cancellation properly
- Clear management state appropriately

### Date Grouping Management
- Format dates consistently across groups
- Handle timezone considerations properly
- Group waitlists logically by event dates
- Provide clear visual separation between groups
- Handle edge cases with date boundaries
- Sort groups chronologically
- Format group headers appropriately

### Modal Integration
- Handle modal state properly
- Provide clear modal navigation
- Implement proper modal cleanup
- Handle modal accessibility requirements
- Manage modal focus appropriately
- Provide clear modal close mechanisms
- Coordinate with other modal systems

### Filter Context Integration
- Use appropriate group names for filtering
- Handle filter state changes
- Coordinate pagination with filters
- Update waitlists when filters change
- Handle filter reset scenarios
- Provide clear filter feedback
- Maintain filter state across navigation

## Component Registration
The component is registered as `codex-waitlists` in the application. 