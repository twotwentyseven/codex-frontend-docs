# CreditTransfer Component

## Overview
The CreditTransfer component provides a comprehensive interface for transferring credits between customers with validation, confirmation, and success feedback. It features email validation, transfer confirmation, processing states, success notifications, and error handling for secure credit transfer operations.

## Basic Usage
```vue
<codex-credit-transfer
  :credits="selectedCredits"
  @transfer-complete="handleTransferComplete"
  @close="closeTransferModal"
/>
```

## Key Features
- Credit transfer interface with email validation
- Transfer confirmation with acknowledgment checkbox
- Processing states during transfer operations
- Success feedback with completion messages
- Comprehensive error handling and display
- Email address validation and formatting
- Transfer confirmation requirements
- Loading states and user feedback
- Modal-based interface design
- Transfer operation tracking

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| credits | Object | Yes | - | Object containing selected credits for transfer |
| titleTag | String | No | 'h1' | HTML tag for the title element |

### Common Props
All common props from `@/config/common` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| transferComplete | - | Emitted when credit transfer is successfully completed |
| close | - | Emitted when transfer modal should be closed |

## Slots

### Header Slot
```vue
<template #header="{ error, fieldErrors, genericErrors }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ error, fieldErrors, genericErrors }">
  <!-- Custom content and transfer interface -->
</template>
```

### Footer Slot
```vue
<template #footer="{ error, fieldErrors, genericErrors }">
  <!-- Custom footer content -->
</template>
```

### Error Messages Slot
```vue
<template #error-messages="{ genericErrors }">
  <!-- Custom error display -->
</template>
```

## Transfer Process
The component manages a multi-step transfer process:

### Step 1: Transfer Form
- **Email Input**: Recipient email address validation
- **Confirmation Checkbox**: User acknowledgment of transfer finality
- **Transfer Button**: Initiates transfer when form is valid
- **Validation**: Real-time form validation

### Step 2: Processing State
- **Loading Display**: Shows transfer in progress
- **User Feedback**: Clear processing indication
- **State Management**: Disables interactions during processing

### Step 3: Success State
- **Success Message**: Confirmation of successful transfer
- **Action Buttons**: Options to close or continue
- **State Reset**: Prepares for potential additional transfers

## Form Validation
The component includes comprehensive validation:

### Email Validation
- **Input Type**: Email input with browser validation
- **Field Errors**: Server-side validation error display
- **Required Field**: Email address is mandatory

### Confirmation Validation
- **Checkbox Required**: Must acknowledge transfer terms
- **Form State**: Transfer button disabled until confirmed
- **User Agreement**: Clear terms of transfer finality

### Transfer Eligibility
- **Selected Credits**: Must have credits selected for transfer
- **Credit Validation**: Validates transferable credit selection
- **Button State**: Transfer button state based on validation

## Credit Selection Management
The component processes selected credits:

### Selection Processing
- **Object Keys**: Processes credits object for selected items
- **Array Conversion**: Converts selection to transferable array
- **Validation**: Ensures valid credit selection exists

### Transfer Data
- **Recipient Email**: Validated email address
- **Credit IDs**: Array of selected credit identifiers
- **Transfer Request**: Formatted for API submission

## Error Handling
The component provides comprehensive error management:

### Field Errors
- **Email Validation**: Specific email field error display
- **Server Validation**: Backend validation error handling
- **Real-time Feedback**: Immediate error indication

### Generic Errors
- **Transfer Errors**: General transfer operation errors
- **Network Errors**: Connection and timeout handling
- **User Feedback**: Clear error messaging and recovery options

## Loading States
The component manages multiple loading states:

### Transfer Processing
- **Processing Indicator**: Visual feedback during transfer
- **Disabled State**: Prevents multiple transfer attempts
- **User Communication**: Clear processing messaging

### Success State
- **Completion Indication**: Visual success confirmation
- **Action Options**: Clear next steps for user
- **State Reset**: Proper cleanup for additional operations

## Internationalization

The component uses the following translation keys:

### Transfer Form Interface
| Key | Usage |
|-----|-------|
| `credit.transfer_credits_title` | Transfer modal title |
| `credit.credits_transfer_intro` | Transfer introduction/description |
| `credit.email_address` | Email field label and placeholder |
| `credit.understand_credit_transfers_are_final` | Confirmation checkbox label |
| `credit.transfer_btn` | Transfer button text |

### Transfer Success Interface
| Key | Usage |
|-----|-------|
| `credit.success_title` | Success state title |
| `credit.success_msg` | Success state message |
| `credit.close_transfer_modal` | Close modal button text |

### Translation Usage Examples
```vue
<!-- Transfer form header -->
<template v-if="selectedCredits.length > 0">
  <codex-title :tag="titleTag" :content="$t('credit.transfer_credits_title')" />
  <codex-paragraph :content="$t('credit.credits_transfer_intro')" />
</template>

<!-- Email input field -->
<codex-input-field
  type="email"
  name="creditTransferEmail"
  id="creditTransferEmail"
  :label="$t('credit.email_address')"
  :placeholder="$t('credit.email_address')"
  :has-error="fieldErrors['email']"
  :errors="fieldErrors['email']"
  v-model="recipient"
/>

<!-- Confirmation checkbox -->
<codex-checkbox-field
  v-model="confirm"
  :label="$t('credit.understand_credit_transfers_are_final')"
  name="confirm_transfer"
  id="confirm_transfer"
  :required="true"
/>

<!-- Transfer button -->
<codex-button 
  :error="error"
  class-name="_c-btn"
  :default-text="$t('credit.transfer_btn')"
  :disabled="!canTransfer"
  @click="transfer"
/>

<!-- Success state -->
<div class="_c-success-container">
  <div class="_c-success-icon">
    <i class="ri-checkbox-circle-fill"></i>
  </div>
  
  <codex-title :tag="titleTag" :content="$t('credits.success_title')" class-name="_c-success-title" />
  <codex-paragraph tag="p" :content="$t('credits.success_msg')" class-name="_c-success-desc" />
  
  <div class="_c-btn-container _c-fill">
    <codex-button 
      :default-text="$t('credits.close_transfer_modal')"
      @click="$emit('close')"
    />
  </div>
</div>
```

### Translation Notes

#### Two-State Translation Structure
The CreditTransfer component has two distinct translation states:
- **Transfer Form State**: Input form with validation and confirmation
- **Success State**: Completion confirmation with action options

#### Form Validation Integration
The component integrates with form validation systems:
- Email field validation with error display
- Required confirmation checkbox with clear terms
- Button state management based on form validity

#### Security and Confirmation Messaging
The component emphasizes transfer finality:
- Clear confirmation checkbox with terms understanding
- Success messaging with completion confirmation
- User education about transfer permanence

#### Modal Integration
The component operates within modal contexts:
- Modal title and description translations
- Close modal action translations
- Success state navigation options

## Usage Examples

### Basic Credit Transfer
```vue
<codex-credit-transfer 
  :credits="selectedCredits"
  @transfer-complete="handleComplete"
>
  <template #header>
    <h3>{{ $t('credit_transfer.title') }}</h3>
  </template>
</codex-credit-transfer>
```

### With Custom Validation
```vue
<codex-credit-transfer 
  :credits="selectedCredits"
  @transfer-complete="handleComplete"
  @validation-error="handleValidationError"
>
  <template #error="{ error }">
    <div class="custom-error">
      <p>{{ $t(`credit_transfer.errors.${error.type}`) }}</p>
    </div>
  </template>
</codex-credit-transfer>
```

## Examples

### Basic Implementation
```vue
<codex-credit-transfer
  :credits="selectedCreditsObject"
  @transfer-complete="handleComplete"
  @close="closeModal"
/>
```

### Custom Header with Transfer Details
```vue
<codex-credit-transfer
  :credits="selectedCredits"
  @transfer-complete="handleTransferComplete"
>
  <template #header="{ error, fieldErrors, genericErrors }">
    <div class="transfer-header">
      <h2>Transfer Credits</h2>
      <div class="transfer-summary">
        <p>You are transferring {{ selectedCreditsCount }} credits</p>
        <div class="credit-details">
          <div v-for="credit in selectedCreditsList" :key="credit.id" class="credit-item">
            <span>{{ credit.credit_type?.name }}</span>
            <span>Expires: {{ formatDate(credit.expires_at) }}</span>
          </div>
        </div>
      </div>
      
      <div class="transfer-warning">
        <i class="warning-icon"></i>
        <p>Credit transfers cannot be reversed once completed.</p>
      </div>
    </div>
  </template>
</codex-credit-transfer>
```

### Custom Content with Enhanced Validation
```vue
<codex-credit-transfer
  :credits="selectedCredits"
  @transfer-complete="handleTransferComplete"
>
  <template #content="{ error, fieldErrors, genericErrors }">
    <div class="enhanced-transfer-form" v-if="selectedCredits.length > 0 && !updating">
      <div class="recipient-section">
        <h4>Recipient Information</h4>
        
        <codex-input-field
          type="email"
          name="creditTransferEmail"
          id="creditTransferEmail"
          :label="$t('credit.email_address')"
          :placeholder="'Enter recipient email address'"
          :has-error="fieldErrors['email']"
          :errors="fieldErrors['email']"
          v-model="recipient"
          @blur="validateEmail"
        />
        
        <div class="email-suggestions" v-if="emailSuggestions.length">
          <p>Recent recipients:</p>
          <button 
            v-for="email in emailSuggestions" 
            :key="email" 
            @click="selectSuggestedEmail(email)"
            class="suggestion-btn"
          >
            {{ email }}
          </button>
        </div>
      </div>
      
      <div class="confirmation-section">
        <h4>Transfer Confirmation</h4>
        
        <div class="transfer-terms">
          <ul>
            <li>Credits will be immediately transferred to the recipient</li>
            <li>This action cannot be undone</li>
            <li>Recipient will receive email notification</li>
            <li>Transfer history will be recorded</li>
          </ul>
        </div>
        
        <codex-checkbox-field
          v-model="confirm"
          :label="$t('credit.understand_credit_transfers_are_final')"
          name="confirm_transfer"
          id="confirm_transfer"
          :required="true"
        />
        
        <codex-checkbox-field
          v-model="notifyRecipient"
          :label="'Send email notification to recipient'"
          name="notify_recipient"
          id="notify_recipient"
        />
      </div>
      
      <div class="transfer-actions">
        <codex-button 
          :error="error"
          class-name="_c-btn"
          variant="secondary"
          :default-text="'Cancel'"
          @click="$emit('close')"
        />
        
        <codex-button 
          :error="error"
          class-name="_c-btn"
          variant="primary"
          :default-text="$t('credit.transfer_btn')"
          :disabled="!canTransfer"
          @click="transfer"
        />
      </div>
    </div>
    
    <div v-else-if="updating" class="transfer-processing">
      <div class="processing-animation">
        <i class="spinner-icon"></i>
      </div>
      <h3>Transferring Credits...</h3>
      <p>Please wait while we process your transfer.</p>
    </div>
    
    <div v-else class="transfer-success">
      <div class="success-container">
        <div class="success-icon">
          <i class="ri-checkbox-circle-fill"></i>
        </div>
        
        <h3 class="success-title">{{ $t('credits.success_title') }}</h3>
        <p class="success-description">{{ $t('credits.success_msg') }}</p>
        
        <div class="transfer-summary">
          <p>{{ transferredCount }} credits transferred to {{ transferRecipient }}</p>
        </div>
        
        <div class="success-actions">
          <codex-button 
            :default-text="'Transfer More Credits'"
            @click="resetTransfer"
            variant="secondary"
          />
          
          <codex-button 
            :default-text="$t('credits.close_transfer_modal')"
            @click="$emit('close')"
            variant="primary"
          />
        </div>
      </div>
    </div>
  </template>
</codex-credit-transfer>
```

### With Event Tracking
```vue
<codex-credit-transfer
  :credits="selectedCredits"
  @transfer-complete="trackTransferComplete"
  @close="trackTransferCancelled"
/>

<script setup>
const trackTransferComplete = () => {
  analytics.track('credit_transfer_completed', {
    credits_count: selectedCreditsCount.value,
    recipient_email: recipient.value,
    transfer_value: calculateTransferValue(),
    customer_id: customer.value?.id
  })
  
  emit('transfer-complete')
}

const trackTransferCancelled = () => {
  analytics.track('credit_transfer_cancelled', {
    credits_selected: selectedCreditsCount.value,
    stage: updating.value ? 'processing' : 'form',
    customer_id: customer.value?.id
  })
  
  emit('close')
}
</script>
```

### In Credits Management Interface
```vue
<div class="credits-management">
  <div class="credits-list">
    <codex-credit-card
      v-for="credit in credits"
      :key="credit.id"
      :credit="credit"
      v-model="selectedCredits[credit.id]"
    />
  </div>
  
  <div class="bulk-actions" v-if="hasSelectedCredits">
    <button @click="showTransferModal = true">
      Transfer {{ selectedCount }} Credits
    </button>
  </div>
  
  <codex-modal v-if="showTransferModal" @close="showTransferModal = false">
    <codex-credit-transfer
      :credits="selectedCredits"
      @transfer-complete="handleTransferSuccess"
      @close="showTransferModal = false"
    />
  </codex-modal>
</div>
```

### Multi-Step Transfer Process
```vue
<codex-credit-transfer
  :credits="selectedCredits"
  @transfer-complete="handleComplete"
>
  <template #content="{ error, fieldErrors, genericErrors }">
    <div class="multi-step-transfer">
      <div class="step-indicator">
        <div class="step" :class="{ active: currentStep === 1 }">1. Select Recipient</div>
        <div class="step" :class="{ active: currentStep === 2 }">2. Review Transfer</div>
        <div class="step" :class="{ active: currentStep === 3 }">3. Confirm</div>
      </div>
      
      <div v-if="currentStep === 1" class="step-content">
        <!-- Recipient selection -->
      </div>
      
      <div v-if="currentStep === 2" class="step-content">
        <!-- Transfer review -->
      </div>
      
      <div v-if="currentStep === 3" class="step-content">
        <!-- Final confirmation -->
      </div>
    </div>
  </template>
</codex-credit-transfer>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-credit-transfer`: Credit transfer specific styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-success-container`: Success state container
- `_c-success-icon`: Success icon styling
- `_c-success-title`: Success title styling
- `_c-success-desc`: Success description styling
- `_c-btn-container`: Button container styling
- `_c-fill`: Fill container utility
- `_c-btn`: Button styling
- `_c-loading`: Loading state styling

## Best Practices

### Recommended Usage Patterns
- Always validate recipient email addresses
- Require explicit confirmation for transfers
- Provide clear transfer terms and conditions
- Handle processing states with user feedback
- Implement proper error handling and recovery
- Track transfer operations for analytics
- Provide success confirmation and next steps
- Use secure transfer validation

### Common Pitfalls to Avoid
- Not validating email addresses properly
- Missing transfer confirmation requirements
- Insufficient error handling for failed transfers
- Not providing clear processing feedback
- Missing success state management
- Forgetting to handle edge cases (no credits selected)
- Not tracking transfer analytics
- Missing accessibility considerations

### Accessibility Considerations
- Provide clear labels for all form fields
- Use appropriate ARIA attributes for form states
- Ensure keyboard navigation works properly
- Provide screen reader friendly success/error feedback
- Include proper form validation messages
- Handle focus management appropriately
- Use semantic HTML for form structure
- Ensure adequate color contrast for all states

### Security Considerations
- Validate recipient email addresses on server
- Implement transfer rate limiting
- Validate credit ownership before transfer
- Use secure authentication for transfers
- Log transfer operations for audit trail
- Prevent double-spending through proper validation
- Handle sensitive data appropriately

### Error Handling
- Display clear validation errors for form fields
- Handle server-side transfer errors gracefully
- Provide meaningful error messages for users
- Clear errors when input changes
- Handle network connectivity issues
- Implement proper retry mechanisms
- Validate credit eligibility before transfer

### State Management
- Track form validation state properly
- Handle transfer processing states
- Manage success/error state transitions
- Update UI state after transfer operations
- Handle component cleanup appropriately
- Coordinate with parent component state
- Manage modal state properly

### Performance Considerations
- Optimize form validation performance
- Handle large credit selections efficiently
- Implement proper cleanup for event listeners
- Minimize re-renders during state changes
- Cache validation results when appropriate
- Optimize transfer processing feedback
- Handle concurrent transfer attempts

### Form Validation
- Implement real-time email validation
- Validate credit selection before transfer
- Ensure confirmation requirements are met
- Handle edge cases in validation logic
- Provide immediate feedback for validation errors
- Use appropriate validation rules
- Clear validation state appropriately

### Transfer Process Management
- Guide users through transfer steps clearly
- Provide appropriate progress feedback
- Handle transfer interruptions gracefully
- Implement proper transfer confirmation
- Track transfer completion accurately
- Handle partial transfer failures
- Provide clear next steps after completion

### Email Validation
- Use proper email format validation
- Handle international email addresses
- Provide suggestions for common domains
- Validate email deliverability when possible
- Handle email normalization appropriately
- Prevent common email input errors
- Provide clear email format guidance

## Component Registration
The component is registered as `codex-credit-transfer` in the application. 