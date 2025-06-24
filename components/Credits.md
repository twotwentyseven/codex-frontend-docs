# Credits Component

## Overview
The Credits component provides a comprehensive interface for managing customer credits, including display, selection, and transfer functionality. It features credit card layouts, selection management, transfer modals, pagination support, and error handling with flexible slot-based customization.

## Basic Usage
```vue
<codex-credits
  :hide-if-no-results="false"
  :group="'customer-credits'"
  @credits-loaded="handleCreditsLoaded"
/>
```

## Key Features
- Credit display with card layout
- Multi-credit selection functionality
- Credit transfer modal integration
- Pagination support with filter context
- Loading states with skeleton placeholders
- No results handling
- Error message display
- Floating transfer button
- Transfer confirmation modal
- Dynamic credit loading
- Customer authentication integration
- Responsive grid layout

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| hideIfNoResults | Boolean | No | false | Hide component when no credits are available |
| group | String | No | undefined | Filter context group for pagination and filtering |

### Common Props
All common props from `@/config/common` are supported, including:
| Prop Name | Usage |
|-----------|-------|
| enableBorder | Adds border styling to credit cards |
| loadingItems | Number of skeleton items to show during loading |

### Common Functions
The component uses utilities from `useCommon`:
| Function | Usage |
|----------|-------|
| loadingItems | Provides skeleton loading item configuration |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| credits-loaded | `credits: Array` | Emitted when credits are successfully loaded |
| transfer-complete | `transferData: Object` | Emitted when credit transfer is completed |

## Slots

### Header Slot
```vue
<template #header="{ credits, error, genericErrors }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ credits, error, genericErrors }">
  <!-- Custom content and credit display -->
</template>
```

### Footer Slot
```vue
<template #footer>
  <!-- Custom footer content (pagination by default) -->
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
| credits | Array | Array of customer credits |
| error | Object | Current error state |
| genericErrors | Array | Generic error messages |

## States
The component has multiple states in order of priority:
1. **Unauthenticated State** - Component hidden when no customer
2. **Loading State** - Shows skeleton credit cards during data fetch
3. **Error State** - Displays error messages
4. **No Results State** - Shows no credits message
5. **Credits Display State** - Normal credit grid display
6. **Selection State** - Credits selected with floating transfer button
7. **Transfer Modal State** - Transfer confirmation and processing

## Credit Selection
The component supports multi-credit selection:
- Individual credit card selection via checkboxes
- Selected credits tracking in reactive state
- Floating transfer button when credits are selected
- Transfer confirmation modal
- Clear selection after successful transfer

## Transfer Functionality
- **Selection Phase**: Users select multiple credits
- **Confirmation Phase**: Transfer modal with recipient selection
- **Processing Phase**: Transfer execution with loading states
- **Completion Phase**: Success feedback and state reset

## Internationalization

The component uses the following translation keys:

### Credits Interface Messages
| Key | Usage |
|-----|-------|
| `credit.no_results` | No credits available message |
| `credit.credits_selected_for_transfer` | Selected credits count with parameter |
| `credit.transfer_modal_btn` | Transfer credits modal button text |

### Translation Usage Examples
```vue
<!-- No results state -->
<div v-else-if="credits.length === 0 && !loading" class="_c-noresults">
  {{ $t('credit.no_results') }}
</div>

<!-- Transfer button modal -->
<div class="_c-flex _c-justify-between _c-items-center">
  <div class="_c-desc">
    {{ $t('credit.credits_selected_for_transfer', { value: selectedCredits.length }) }}
  </div>
  
  <codex-button 
    variant="primary"
    :default-text="$t('credit.transfer_modal_btn')"
    :disabled="!creditsAreSelected"
    @click="showConfirmationModal"
    dusk="codex-transfer-credits"
  />
</div>
```

### Translation Notes

#### Credit Management Interface
The Credits component has focused translation requirements:
- Primarily handles no results messaging and transfer actions
- Uses parameterized translations for dynamic credit counts
- Integrates with modal systems for transfer functionality

#### Child Component Integration
The component delegates most translation work to child components:
- `codex-credit-card` components handle individual credit display translations
- `codex-credit-transfer` component handles transfer form translations
- `codex-pagination` component handles pagination-related translations

#### Transfer Workflow Translations
The component supports credit transfer workflows:
- Selection count with dynamic parameter support
- Transfer button integration with confirmation modal
- Success and error state handling through child components

#### Filter Integration
The component integrates with filter systems:
- Uses filter context for credit loading and pagination
- Supports customer authentication state changes
- Handles loading states during credit data fetching

## Usage Examples

### Basic Credits Display
```vue
<codex-credits>
  <template #header>
    <h2>{{ $t('credits.title') }}</h2>
    <p>{{ $t('credits.description') }}</p>
  </template>
</codex-credits>
```

### With Transfer Functionality
```vue
<codex-credits 
  :enable-transfer="true"
  @transfer-complete="handleTransferComplete"
>
  <template #transfer-actions>
    <div class="custom-transfer">
      <codex-button 
        :default-text="$t('credits.transfer.transfer_button')"
        @click="openTransferModal"
      />
    </div>
  </template>
</codex-credits>
```

### With Custom Empty State
```vue
<codex-credits>
  <template #empty-state>
    <div class="custom-empty-state">
      <h3>{{ $t('credits.empty.title') }}</h3>
      <p>{{ $t('credits.empty.description') }}</p>
      <codex-button 
        :default-text="$t('credits.empty.earn_credits')"
        @click="navigateToEarnCredits"
      />
    </div>
  </template>
</codex-credits>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-credits-card`: Credits-specific card styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-grid`: Credit cards grid layout
- `_c-w-full`: Full width utility
- `_c-noresults`: No results state styling
- `_c-credits-transfer-button`: Floating transfer button container
- `_c-flex`: Flexbox utility
- `_c-justify-between`: Space between flex items
- `_c-items-center`: Center align flex items
- `_c-desc`: Description text styling

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Implement proper loading states during credit fetch
- Provide clear feedback for selection states
- Use pagination for large credit datasets
- Handle transfer completion with proper state updates
- Implement error retry mechanisms
- Provide clear visual feedback for selected credits

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing loading states during data fetch
- Forgetting to clear selections after transfer
- Not providing feedback during transfer process
- Missing error handling for failed transfers
- Not updating credit list after successful transfers
- Insufficient visual feedback for selection states

### Accessibility Considerations
- Ensure credit cards are keyboard navigable
- Provide clear labels for selection checkboxes
- Use appropriate ARIA attributes for dynamic content
- Ensure transfer buttons are accessible
- Provide screen reader friendly error messages
- Include proper focus management for modals
- Use semantic HTML for credit information

### Error Handling
- Display clear error messages for failed loads
- Provide retry mechanisms for network failures
- Handle transfer errors gracefully
- Show validation errors for invalid transfers
- Clear error states when operations succeed
- Handle authentication errors appropriately
- Provide fallback states for partial failures

### State Management
- Track credit selection state properly
- Handle customer authentication changes
- Manage loading states consistently
- Update credit list after transfers
- Clear selection after successful operations
- Handle pagination state with filters
- Coordinate with transfer modal state

### Performance Considerations
- Implement virtual scrolling for large credit lists
- Lazy load credit card components
- Debounce selection state updates
- Optimize grid rendering performance
- Handle large transfer operations efficiently
- Consider pagination for better performance
- Implement proper cleanup for event listeners

### Transfer Management
- Validate credit eligibility before transfer
- Provide clear transfer confirmation
- Handle partial transfer failures
- Update UI state after successful transfers
- Clear selection state appropriately
- Provide transfer progress feedback
- Handle transfer cancellation properly

### Filter Context Integration
- Use appropriate group names for filtering
- Handle filter state changes
- Coordinate pagination with filters
- Update credits when filters change
- Handle filter reset scenarios
- Provide clear filter feedback
- Maintain filter state across navigation

## Component Registration
The component is registered as `codex-credits` in the application. 