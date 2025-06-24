# AvailableCredits Component

## Overview
The AvailableCredits component provides a dashboard widget interface for displaying available credit statistics with count visualization and navigation to detailed credit management. It features credit counting, loading states, error handling, and navigation to comprehensive credit views for effective credit balance tracking.

## Basic Usage
```vue
<codex-available-credits
  :type="'unused'"
  :default-target-tab="'credits'"
  :hide-if-no-results="false"
/>
```

## Key Features
- Available credit count display
- Credit type filtering and tracking
- Large number visualization
- Navigation to detailed credit views
- Loading states with skeleton placeholders
- Error handling and display
- No results state management
- Customer authentication integration
- Responsive design with mobile adaptations
- Credit statistics integration

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| type | String | No | 'unused' | Type of credits to track ('unused' or 'used') |
| hideIfNoResults | Boolean | No | false | Hide component when no credits exist |
| defaultTargetTab | String | No | 'credits' | Target tab when navigating to detailed view |

### Type Validation
The type prop includes validation for supported credit types:
- `'unused'`: Shows available/unused credits
- `'used'`: Shows used/consumed credits

### Common Props
All common props from `@/config/common` are supported.

### Common Functions
The component uses utilities from `useCommon`:
| Function | Usage |
|----------|-------|
| handleClick | Handles navigation clicks with proper event handling |
| isMobile | Provides mobile device detection for responsive layout |

## Events
Currently, the AvailableCredits component does not emit custom events. It serves as a dashboard widget with internal navigation.

## Slots

### Header Slot
```vue
<template #header="{ credits, loading, error, genericErrors }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ credits, loading, error, genericErrors, relations }">
  <!-- Custom content and credit display -->
</template>
```

### Footer Slot
```vue
<template #footer="{ credits, loading, error, genericErrors }">
  <!-- Custom footer content -->
</template>
```

### Error Messages Slot
```vue
<template #error-messages="{ genericErrors }">
  <!-- Custom error display -->
</template>
```

## Credit Management Integration
The component integrates with the credit management system:
- **Credit Loading**: Uses `useCredits` composable for data management
- **Real-time Updates**: Automatically updates when credit data changes
- **Authentication**: Requires authenticated customer for display
- **Relations**: Supports credit relationships and metadata

## Credit Counting System
The component includes credit counting functionality:

### Total Credit Calculation
- **Count Logic**: Counts total credits in the credits array
- **Type Filtering**: Filters credits based on specified type
- **Real-time Updates**: Updates count when credits change
- **Performance**: Efficient counting for large credit lists

### Credit Type Counter
- **Type-based Filtering**: `creditTypeCounter(type)` method for type-specific counts
- **Dynamic Counting**: Supports various credit type categorizations
- **Flexible Filtering**: Adapts to different credit type structures

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
- **Conditional Logic**: Complex logic for determining display (needs rework)

### Success State
- **Credit Display**: Shows credit count and information
- **Interactive Elements**: Provides navigation to detailed views

## Responsive Design
The component adapts to different screen sizes:
- **Mobile Layout**: Stacked layout for smaller screens
- **Desktop Layout**: Horizontal layout with spacing
- **Conditional Wrapper**: Uses `codex-conditional-wrapper` for responsive behavior

## Data Integration
The component integrates with multiple data sources:
- **Credits Data**: Primary credit information
- **Relations**: Credit relationships and metadata
- **Pagination**: Credit pagination information
- **Customer Data**: Customer authentication and stats

## Internationalization

The component uses the following translation keys:

### Credit Summary Display
| Key | Usage |
|-----|-------|
| `credit.remaining_credits` | Remaining credits section title |
| `credit.you_have_credits_remaining` | Credits count message with parameter |
| `credit.view_all` | View all credits button text |

### Translation Usage Examples
```vue
<!-- Credit summary display -->
<div class="_c-remaining-credits-container _c-column _c-gap-sm _c-grow">
  <div class="_c-title">{{ $t('credit.remaining_credits') }}</div>
  
  <div class="_c-credit-type _c-total-credits">
    <div class="_c-credit-name">
      {{ $t('credit.you_have_credits_remaining', { value: totalCredits }) }}
    </div>
  </div>
</div>

<!-- View all button -->
<div class="_c-btn-container">
  <codex-button 
    :default-text="$t('credit.view_all')"
    @click="(e) => handleClick(e, props.defaultTargetTab)"
  />
</div>
```

### Translation Notes

#### Dashboard Widget Focus
The AvailableCredits component serves as a dashboard widget with minimal translation needs:
- Displays credit count with parameterized messaging
- Provides navigation to detailed credit management
- Shows large numeric displays with descriptive labels

#### Credit Count Integration
The component calculates and displays credit statistics:
- Uses `totalCredits` computed property for count display
- Supports parameterized translations for dynamic count values
- Integrates with credit filtering and type management

#### Navigation Integration
The component provides navigation to detailed views:
- Uses `handleClick` from common utilities for navigation
- Supports configurable target tabs through `defaultTargetTab` prop
- Integrates with responsive design for mobile vs desktop layout

#### Loading State Management
The component includes comprehensive loading state handling:
- Skeleton loading layouts during data fetch
- Conditional display logic based on credit availability
- Error state management through slot system

## Usage Examples

### Basic Available Credits
```vue
<codex-available-credits :credits="userCredits">
  <template #header>
    <h3>{{ $t('available_credits.title') }}</h3>
  </template>
</codex-available-credits>
```

### With Custom Actions
```vue
<codex-available-credits 
  :credits="userCredits"
  @click="handleCreditsClick"
>
  <template #actions>
    <codex-button 
      :default-text="$t('available_credits.manage_credits')"
      @click="navigateToManagement"
    />
  </template>
</codex-available-credits>
```

## Examples

### Basic Implementation
```vue
<codex-available-credits />
```

### Used Credits Display
```vue
<codex-available-credits
  :type="'used'"
  :default-target-tab="'used-credits'"
/>
```

### Custom Header with Credit Types
```vue
<codex-available-credits>
  <template #header="{ credits, loading, error, genericErrors }">
    <div class="credits-header">
      <h3>Your Credit Balance</h3>
      <div class="credit-types-summary">
        <div class="credit-type" v-for="type in creditTypes" :key="type.id">
          <span class="type-name">{{ type.name }}</span>
          <span class="type-count">{{ creditTypeCounter(type.id) }}</span>
        </div>
      </div>
    </div>
  </template>
</codex-available-credits>
```

### Custom Content with Credit Details
```vue
<codex-available-credits>
  <template #content="{ credits, loading, error, genericErrors, relations }">
    <div class="credit-details" v-if="!error">
      <div class="main-display">
        <div class="total-number">{{ totalCredits }}</div>
        
        <div class="credit-breakdown">
          <h4>{{ $t('credit.remaining_credits') }}</h4>
          
          <div class="credit-summary">
            <div class="summary-item">
              <span class="label">Total Available:</span>
              <span class="value">{{ totalCredits }}</span>
            </div>
            <div class="summary-item">
              <span class="label">Expiring Soon:</span>
              <span class="value">{{ expiringSoonCount }}</span>
            </div>
            <div class="summary-item">
              <span class="label">This Month Added:</span>
              <span class="value">{{ thisMonthAddedCount }}</span>
            </div>
          </div>
          
          <div class="action-button">
            <codex-button 
              :default-text="$t('credit.view_all')"
              @click="handleClick($event, defaultTargetTab)"
            />
          </div>
        </div>
      </div>
    </div>
    
    <slot v-else name="error-messages" :genericErrors="genericErrors">
      <codex-error :error="genericErrors" />
    </slot>
  </template>
</codex-available-credits>
```

### Custom Footer with Actions
```vue
<codex-available-credits>
  <template #footer="{ credits, loading, error, genericErrors }">
    <div class="credits-actions">
      <div class="quick-actions">
        <button @click="purchaseCredits" class="purchase-btn">
          Buy More Credits
        </button>
        <button @click="transferCredits" class="transfer-btn">
          Transfer Credits
        </button>
      </div>
      
      <div class="credit-info">
        <span class="expiry-warning" v-if="hasExpiringSoon">
          {{ expiringSoonCount }} credits expiring soon
        </span>
      </div>
    </div>
  </template>
</codex-available-credits>
```

### With Error Handling
```vue
<codex-available-credits>
  <template #error-messages="{ genericErrors }">
    <div class="custom-error-display">
      <i class="error-icon" />
      <div>
        <h4>Unable to Load Credits</h4>
        <ul>
          <li v-for="error in genericErrors" :key="error">
            {{ error }}
          </li>
        </ul>
        <button @click="retryLoadCredits">Try Again</button>
      </div>
    </div>
  </template>
</codex-available-credits>
```

### Dashboard Integration
```vue
<div class="account-dashboard">
  <div class="balance-widgets">
    <codex-available-credits 
      class="credits-widget"
      :type="'unused'"
    />
    
    <codex-bookings-completed 
      class="bookings-widget"
    />
    
    <div class="subscriptions-widget">
      <!-- Subscription status widget -->
    </div>
  </div>
</div>
```

### Credit Type Breakdown
```vue
<codex-available-credits>
  <template #content="{ credits, loading, error, genericErrors }">
    <div class="credit-type-breakdown" v-if="!loading && !error">
      <div class="total-display">
        <span class="total-number">{{ totalCredits }}</span>
        <span class="total-label">Total Credits</span>
      </div>
      
      <div class="type-breakdown">
        <div v-for="type in uniqueCreditTypes" :key="type.id" class="type-item">
          <span class="type-name">{{ type.name }}</span>
          <span class="type-count">{{ getCreditsForType(type.id) }}</span>
        </div>
      </div>
      
      <div class="management-actions">
        <codex-button 
          :default-text="'Manage Credits'"
          @click="navigateToCredits"
        />
      </div>
    </div>
  </template>
</codex-available-credits>

<script setup>
const uniqueCreditTypes = computed(() => {
  const types = credits.value.map(credit => credit.credit_type).filter(Boolean)
  return [...new Map(types.map(type => [type.id, type])).values()]
})

const getCreditsForType = (typeId) => {
  return credits.value.filter(credit => credit.credit_type?.id === typeId).length
}
</script>
```

### With Expiry Warnings
```vue
<codex-available-credits>
  <template #content="{ credits, loading, error }">
    <div class="credits-with-warnings" v-if="!loading && !error">
      <div class="main-count">
        <span class="count">{{ totalCredits }}</span>
        <span class="label">Available Credits</span>
      </div>
      
      <div class="warnings" v-if="hasWarnings">
        <div class="warning-item" v-if="expiringSoonCount > 0">
          <i class="warning-icon"></i>
          <span>{{ expiringSoonCount }} credits expire within 7 days</span>
        </div>
        
        <div class="warning-item" v-if="expiredCount > 0">
          <i class="error-icon"></i>
          <span>{{ expiredCount }} credits have expired</span>
        </div>
      </div>
      
      <div class="actions">
        <codex-button 
          :default-text="'View Details'"
          @click="handleClick($event, 'credits')"
        />
      </div>
    </div>
  </template>
</codex-available-credits>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-grow`: Flex grow utility
- `_c-available-credits-card`: Available credits specific styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-justify-center`: Center justify utility
- `_c-footer`: Footer section
- `_c-row`: Row flexbox layout
- `_c-items-center`: Center align flex items
- `_c-justify-between`: Space between flex items
- `_c-wrap`: Flex wrap utility
- `_c-nowrap_lg`: No wrap on large screens
- `_c-total-number`: Total number display styling
- `_c-text-display`: Display text styling
- `_c-column`: Column layout utility
- `_c-gap-lg`: Large gap utility
- `_c-gap-sm`: Small gap utility
- `_c-grow`: Flex grow utility
- `_c-remaining-credits-container`: Credits container styling
- `_c-title`: Title styling
- `_c-credit-type`: Credit type styling
- `_c-total-credits`: Total credits styling
- `_c-credit-name`: Credit name styling
- `_c-btn-container`: Button container styling

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Provide clear credit count visualization
- Handle loading states during credit data fetch
- Implement proper navigation to detailed views
- Provide clear credit information
- Use responsive design principles
- Track credit viewing analytics
- Handle credit type variations gracefully

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing loading states during data fetch
- Not providing clear navigation paths
- Missing responsive considerations
- Insufficient handling of edge cases (no credits)
- Not optimizing count calculations
- Missing accessibility considerations
- Complex conditional logic (noted for rework)

### Accessibility Considerations
- Provide meaningful count descriptions
- Use appropriate ARIA attributes for counts
- Ensure keyboard navigation for interactive elements
- Provide screen reader friendly credit information
- Include proper semantic HTML structure
- Ensure adequate color contrast for counts
- Handle focus management appropriately
- Use semantic markup for statistics

### Error Handling
- Handle missing credit data gracefully
- Provide fallbacks for malformed data
- Display meaningful error messages
- Clear error states when data updates
- Handle count calculation errors
- Provide retry mechanisms for failed loads
- Handle edge cases in credit counting

### State Management
- Track credit data changes
- Handle authentication state changes
- Manage loading states consistently
- Update counts when data changes
- Handle credit additions/removals dynamically
- Coordinate with credit management systems
- Manage component lifecycle properly

### Performance Considerations
- Optimize count calculation performance
- Minimize re-calculations of totals
- Handle large credit arrays efficiently
- Implement proper cleanup for watchers
- Cache count calculations appropriately
- Optimize responsive layout transitions
- Handle frequent credit updates efficiently

### Credit Management Integration
- Coordinate with credit loading systems
- Handle credit relationships properly
- Track credit viewing analytics
- Handle credit state changes appropriately
- Provide clear navigation to management
- Coordinate with credit transfer systems
- Handle credit expiry tracking

### Count Calculation Optimization
- Use efficient counting algorithms
- Cache calculation results when appropriate
- Handle large credit datasets
- Optimize type-based filtering
- Minimize recalculations
- Handle edge cases in counting logic
- Validate count accuracy

### Navigation Integration
- Provide clear navigation paths
- Handle navigation state properly
- Coordinate with routing systems
- Track navigation analytics
- Handle navigation errors gracefully
- Provide appropriate navigation feedback
- Ensure navigation accessibility

### Component Architecture Notes
- The current hideIfNoResults logic is complex and may need rework
- Consider simplifying conditional display logic
- Review credit type counter implementation for optimization
- Evaluate responsive wrapper usage patterns

## Component Registration
The component is registered as `codex-available-credits` in the application. 