# CreditCard Component

## Overview
The CreditCard component provides a compact card interface for displaying individual credit entries with selection functionality, expiration tracking, and transfer capabilities. It features credit type information, purchase and expiry dates, usage status indicators, selection checkboxes for transferable credits, and expiration warnings for effective credit management.

## Basic Usage
```vue
<codex-credit-card
  :credit="creditData"
  :loading="false"
  v-model="isSelected"
  @transfer-credit="handleCreditTransfer"
/>
```

## Key Features
- Credit type information display
- Purchase and expiry date tracking
- Usage status indicators
- Selection checkbox for transferable credits
- Expiration warning system
- Transfer functionality integration
- Loading skeleton support
- Different visual states (used vs unused)
- Automatic expiration detection

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| credit | Object\|Number | Yes | - | Credit data object or ID |
| loading | Boolean | No | false | Whether the card is in loading state |
| enableBorder | Boolean | No | false | Whether to enable card border styling |

### Common Props
All common props from `@/config/common` are supported.

### Model Value
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| modelValue | Boolean | No | false | Selection state for transferable credits |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| transfer-credit | `credit: Object` | Emitted when credit transfer is initiated |
| update:modelValue | `selected: Boolean` | Emitted when selection state changes |

## Slots

### Header Slot
```vue
<template #header="{ credit }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ credit }">
  <!-- Custom content and credit display -->
</template>
```

### Footer Slot
```vue
<template #footer="{ transferCredit, credit }">
  <!-- Custom footer content -->
</template>
```

## Credit Object Structure
The credit prop expects an object with the following structure:
```javascript
{
  id: String|Number,          // Unique credit identifier
  created_at: Date,           // Credit purchase date
  expires_at: Date,           // Credit expiration date
  used_for_booking_id: Number|null,  // Booking ID if credit is used (null if unused)
  is_transferrable: Boolean,  // Whether credit can be transferred
  credit_type: {              // Credit type information
    id: String|Number,        // Credit type ID
    name: String              // Credit type name
  }
}
```

## Credit States
The component displays different states based on credit usage:

### Unused Credits (`used_for_booking_id === null`)
- Shows selection checkbox (if transferable)
- Displays transfer functionality in footer
- Uses `_c-credit-unused` styling
- Shows expiration warnings when applicable

### Used Credits (`used_for_booking_id !== null`)
- Hides selection checkbox
- Shows booking usage information
- Uses `_c-credit-used` styling
- Displays which booking the credit was used for

## Selection System
For transferable, unused credits:
- **Checkbox Display**: Shows selection checkbox in header
- **Model Binding**: Two-way binding with v-model
- **Transfer Integration**: Selected credits can be transferred
- **Visual Feedback**: Selection state affects styling

## Expiration Management
The component includes automatic expiration detection:
- **Expiring Soon**: Credits expiring within 7 days
- **Visual Warning**: Special styling for soon-to-expire credits
- **Status Text**: Clear expiration warning message
- **Current Date Logic**: Uses moment.js for accurate date calculations

## Loading State
The component supports skeleton loading with:
- **Structured Layout**: Organized placeholder elements
- **Header Section**: Credit type name placeholder
- **Content Section**: Purchase and expiry date placeholders
- **Footer Section**: Transfer button placeholder (for transferable credits)

## Internationalization

The `CreditCard` component uses translation keys for credit information and status messages:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `credit.purchased_prefix` | Credit purchase date label | When credit was acquired |
| `credit.expires_prefix` | Credit expiry date label | When credit expires |
| `credit.expiring_soon` | Warning message for soon-to-expire credits | Credits expiring within 7 days |
| `credit.used_for_prefix` | Used credit reference label | Which booking used the credit |

### Implementation Examples

```vue
<!-- Credit purchase information -->
<div class="_c-credit-purchased">
    {{ $t('credit.purchased_prefix') }} {{ $filters.shortDate(credit.created_at) }}
</div>

<!-- Credit expiry information -->
<div class="_c-credit-expiry">
    {{ $t('credit.expires_prefix') }} {{ $filters.shortDate(credit.expires_at) }}
</div>

<!-- Expiry warning -->
<div v-if="expiringSoon" class="_c-credit-expiring-soon">
    {{ $t('credit.expiring_soon') }}
</div>

<!-- Used credit reference -->
<div v-if="credit.used_for_booking_id" class="_c-credit-used-for">
    {{ $t('credit.used_for_prefix') }} {{ credit.used_for_booking_id }}
</div>
```

### Computed Properties for Translation Context

The component includes logic for determining when to show expiry warnings:

```javascript
const expiringSoon = computed(() => {
    return moment(props.credit.expires_at).isBefore(moment().add(7, 'days')) && 
           !moment(props.credit.expires_at).isBefore(moment());
});
```

### Notes
- Credit name comes from `credit.credit_type.name` (not translated)
- Date formatting handled through Vue filters
- Conditional translation rendering based on credit status
- Transfer functionality handled through parent component slots
- Credit type and booking ID are displayed as raw values

## Examples

### Basic Implementation
```vue
<codex-credit-card
  :credit="credit"
  v-model="selectedCredits[credit.id]"
/>
```

### With Loading State
```vue
<codex-credit-card
  :credit="credit"
  :loading="isLoading"
  :enable-border="true"
/>
```

### Used Credit Display
```vue
<codex-credit-card
  :credit="usedCredit"
/>

<!-- 
Automatically displays usage information when 
credit.used_for_booking_id is not null
-->
```

### Custom Header Content
```vue
<codex-credit-card
  :credit="credit"
  v-model="isSelected"
>
  <template #header="{ credit }">
    <div class="credit-header">
      <div class="credit-type-info">
        <h4 class="credit-type-name">{{ credit.credit_type?.name || 'Standard Credit' }}</h4>
        <span class="credit-id">ID: {{ credit.id }}</span>
      </div>
      
      <div class="credit-status">
        <span v-if="credit.used_for_booking_id" class="used-badge">Used</span>
        <span v-else-if="isExpiringSoon(credit)" class="expiring-badge">Expiring Soon</span>
        <span v-else class="available-badge">Available</span>
      </div>
      
      <codex-checkbox-field
        v-if="credit.used_for_booking_id == null && credit.is_transferrable"
        v-model="model"
        :id="'credit_id_'+credit.id"
        :required="false"
        label=""
      />
    </div>
  </template>
</codex-credit-card>
```

### Custom Content with Additional Information
```vue
<codex-credit-card
  :credit="credit"
  v-model="isSelected"
>
  <template #content="{ credit }">
    <div class="credit-details">
      <div class="credit-dates">
        <div class="purchase-info">
          <span class="label">{{ $t('credit.purchased_prefix') }}</span>
          <span class="date">{{ $filters.shortDate(credit.created_at) }}</span>
        </div>
        
        <div class="expiry-info">
          <span class="label">{{ $t('credit.expires_prefix') }}</span>
          <span class="date" :class="{ 'expiring': isExpiringSoon(credit) }">
            {{ $filters.shortDate(credit.expires_at) }}
          </span>
        </div>
      </div>
      
      <div class="credit-value" v-if="credit.value">
        <span class="value">{{ formatCurrency(credit.value) }}</span>
      </div>
      
      <div class="credit-restrictions" v-if="credit.restrictions">
        <ul class="restrictions-list">
          <li v-for="restriction in credit.restrictions" :key="restriction">
            {{ restriction }}
          </li>
        </ul>
      </div>
      
      <div v-if="isExpiringSoon(credit)" class="expiration-warning">
        <i class="warning-icon"></i>
        {{ $t('credit.expiring_soon') }}
      </div>
      
      <div v-if="credit.used_for_booking_id" class="usage-info">
        {{ $t('credit.used_for_prefix') }} Booking #{{ credit.used_for_booking_id }}
      </div>
    </div>
  </template>
</codex-credit-card>
```

### Custom Footer with Transfer Actions
```vue
<codex-credit-card
  :credit="credit"
  v-model="isSelected"
>
  <template #footer="{ transferCredit, credit }">
    <div class="credit-actions" v-if="credit.used_for_booking_id == null && credit.is_transferrable">
      <button 
        @click="transferCredit(credit)" 
        class="transfer-btn"
        :disabled="!isSelected"
      >
        Transfer Credit
      </button>
      
      <button @click="viewCreditDetails(credit)" class="details-btn">
        View Details
      </button>
      
      <button @click="extendExpiry(credit)" class="extend-btn" v-if="isExpiringSoon(credit)">
        Extend Expiry
      </button>
    </div>
  </template>
</codex-credit-card>
```

### In Credits List with Selection
```vue
<div class="credits-container">
  <div class="credits-header">
    <h3>Available Credits</h3>
    <button 
      @click="selectAllCredits" 
      :disabled="!hasTransferableCredits"
    >
      Select All
    </button>
  </div>
  
  <codex-credit-card
    v-for="credit in credits"
    :key="credit.id"
    :credit="credit"
    v-model="selectedCredits[credit.id]"
    @transfer-credit="initiateCreditTransfer"
  />
  
  <div class="bulk-actions" v-if="hasSelectedCredits">
    <button @click="transferSelectedCredits">
      Transfer {{ selectedCount }} Credits
    </button>
  </div>
</div>
```

### With Event Tracking
```vue
<codex-credit-card
  :credit="credit"
  v-model="isSelected"
  @transfer-credit="trackCreditTransfer"
/>

<script setup>
const trackCreditTransfer = (credit) => {
  analytics.track('credit_transfer_initiated', {
    credit_id: credit.id,
    credit_type_id: credit.credit_type?.id,
    expires_at: credit.expires_at,
    is_expiring_soon: isExpiringSoon(credit)
  })
  
  emit('transfer-credit', credit)
}

const isExpiringSoon = (credit) => {
  return moment(credit.expires_at).isBefore(moment().add(7, 'days')) && 
         !moment(credit.expires_at).isBefore(moment())
}
</script>
```

### Grouped Credits Display
```vue
<div class="credits-by-type">
  <div v-for="(typeCredits, typeName) in creditsByType" :key="typeName" class="credit-type-group">
    <h4 class="type-header">{{ typeName }}</h4>
    <div class="type-credits">
      <codex-credit-card
        v-for="credit in typeCredits"
        :key="credit.id"
        :credit="credit"
        v-model="selectedCredits[credit.id]"
        :enable-border="true"
      />
    </div>
  </div>
</div>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-credit-card`: Credit card specific styling
- `_c-credit-unused`: Unused credit styling
- `_c-credit-used`: Used credit styling
- `_c-border`: Border styling (when enabled)
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-flex`: Flexbox utility
- `_c-justify-between`: Space between flex items
- `_c-items-start`: Align flex items to start
- `_c-title`: Title styling
- `_c-credit-purchased`: Purchase date styling
- `_c-credit-expiry`: Expiry date styling
- `_c-credit-expiring-soon`: Expiring soon warning styling
- `_c-credit-used-for`: Used for booking styling

## Best Practices

### Recommended Usage Patterns
- Always provide proper credit object structure
- Handle loading states for better UX
- Implement proper selection state management
- Display expiration warnings clearly
- Handle transfer functionality appropriately
- Use appropriate visual states for used/unused credits
- Track credit management actions
- Handle timezone considerations for dates

### Common Pitfalls to Avoid
- Not handling missing credit data gracefully
- Missing loading state implementation
- Forgetting to handle selection state properly
- Not providing clear expiration warnings
- Missing transfer confirmation flows
- Insufficient handling of different credit states
- Not tracking selection states properly
- Missing accessibility considerations

### Accessibility Considerations
- Ensure cards are keyboard navigable
- Provide clear labels for selection checkboxes
- Use appropriate ARIA attributes for status indicators
- Ensure adequate color contrast for expiration warnings
- Provide screen reader friendly credit information
- Include proper focus management
- Use semantic HTML for credit data
- Handle disabled states properly for screen readers

### Error Handling
- Handle missing credit data gracefully
- Provide fallbacks for malformed credit objects
- Handle transfer action failures appropriately
- Display meaningful error states
- Clear error states when data updates
- Handle date parsing errors
- Provide user feedback for failed operations

### State Management
- Track credit selection state properly
- Handle selection changes dynamically
- Manage loading states consistently
- Update UI after transfer actions
- Handle concurrent state changes
- Coordinate with parent component state
- Manage transfer processing states appropriately

### Performance Considerations
- Optimize credit data rendering
- Minimize re-renders during selection changes
- Handle large credit lists efficiently
- Implement proper cleanup for event listeners
- Cache formatted credit data appropriately
- Optimize date calculations
- Handle real-time credit updates efficiently

### Credit Management
- Validate transfer actions based on credit status
- Handle transfer eligibility properly
- Provide clear action feedback
- Handle partial transfer failures
- Update credit state after successful transfers
- Provide transfer progress feedback
- Handle transfer cancellation properly

### Date Management
- Handle timezone considerations properly
- Calculate expiration warnings accurately
- Format dates consistently
- Handle edge cases in date calculations
- Display dates in user's preferred format
- Handle daylight saving time transitions
- Validate date data integrity

### Selection Management
- Track selection state across component updates
- Handle bulk selection operations
- Provide clear selection feedback
- Coordinate selection with transfer actions
- Handle selection persistence appropriately
- Validate selection constraints
- Clear selection state when appropriate

### Transfer Integration
- Coordinate with transfer systems properly
- Handle transfer eligibility validation
- Provide clear transfer feedback
- Handle transfer error scenarios
- Track transfer analytics
- Manage transfer state appropriately
- Handle concurrent transfer operations

## Component Registration
The component is registered as `codex-credit-card` in the application. 