# Policy Component

## Overview
The Policy component provides a specialized checkbox input designed for policy acceptance and signature functionality. It features Vue dependency injection for configuration, v-model binding for state management, custom styling for signature visual representation, error state handling, and true/false value configuration for flexible policy agreement workflows.

## Basic Usage
```vue
<!-- Must be used within a provider that injects required values -->
<template>
  <div>
    <!-- Provider component that injects policy configuration -->
    <policy-provider
      :computed-name="'I agree to the Terms of Service'"
      :true-value="true"
      :false-value="false"
    >
      <codex-policy
        :id="'policy-agreement'"
        :name="'terms-agreement'"
      />
    </policy-provider>
  </div>
</template>
```

## Key Features
- Vue dependency injection for configuration management
- v-model binding with defineModel for reactive state
- Specialized signature-style checkbox rendering
- True/false value configuration for flexible agreements
- Error state visual indication
- Required field validation support
- Accessibility attributes for screen readers
- Custom checkbox styling for policy contexts

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| id | String | No | '' | Unique identifier for the checkbox input |
| name | String | No | '' | Name attribute for form submission |
| dusk | String | No | '' | Testing attribute for browser automation |

## Injected Dependencies

The component relies on Vue's dependency injection system for configuration:

### Required Injections
| Injection Key | Type | Description |
|---------------|------|-------------|
| computedName | String | Display text for the policy agreement |
| trueValue | Any | Value when policy is accepted (default: true) |
| falseValue | Any | Value when policy is not accepted (default: false) |

### Optional Injections
| Injection Key | Type | Default | Description |
|---------------|------|---------|-------------|
| disabled | Boolean | false | Whether the checkbox is disabled |
| placeholder | String | '' | Placeholder text (rarely used for checkboxes) |
| ariaLabel | String | '' | ARIA label for accessibility |
| required | Boolean | false | Whether the field is required |
| hasError | Boolean | false | Whether the field has validation errors |

## Model Value
The component uses `defineModel` for two-way data binding:
- **Type**: Boolean, String, or Number
- **Default**: false
- **Binding**: Uses v-model with true-value and false-value attributes

## Visual States
The component provides different visual states:
- **Unchecked State**: Empty signature box
- **Checked State**: Displays the computed name text
- **Error State**: Applies `_c-error` class for error styling
- **Disabled State**: Non-interactive state when disabled

## Dependency Injection Pattern
The component uses Vue's provide/inject pattern:
- **Provider**: Parent component provides configuration values
- **Consumer**: Policy component injects and uses provided values
- **Reactivity**: Injected values remain reactive across the component tree
- **Defaults**: Fallback values provided for optional injections

## Checkbox Behavior
Standard HTML checkbox behavior with custom styling:
- **Input Type**: checkbox with custom visual representation
- **Value Binding**: Uses true-value and false-value for flexible value types
- **Form Integration**: Works with standard form submission
- **Validation**: Supports HTML5 validation attributes

## Accessibility Features
The component provides comprehensive accessibility:
- **Checkbox Semantics**: Proper input type="checkbox" for screen readers
- **ARIA Label**: Configurable aria-label through injection
- **Label Association**: Uses for/id relationship with label element
- **Required Indication**: Supports required attribute for validation
- **Error States**: Visual and semantic error indication

## Error Handling
Error state management through injection:
- **Visual Indication**: `_c-error` class applied when hasError is true
- **External Control**: Error state managed by parent provider
- **Form Validation**: Integrates with form validation systems
- **User Feedback**: Clear visual indication of validation issues

## Use Cases
Suitable for:
- **Terms of Service**: User agreement to terms and conditions
- **Privacy Policy**: Consent for privacy policy acceptance
- **Legal Agreements**: Contract and agreement signatures
- **Consent Forms**: Data processing consent checkboxes
- **Signature Fields**: Electronic signature acknowledgment

## Internationalization
The component receives internationalized text through the `computedName` injection, allowing for localized policy text.

## Examples

### Terms of Service Agreement
```vue
<template>
  <form @submit="handleSubmit">
    <policy-provider
      :computed-name="$t('legal.terms.agreement')"
      :true-value="'accepted'"
      :false-value="'declined'"
      :required="true"
      :has-error="validationErrors.terms"
    >
      <codex-policy
        :id="'terms-agreement'"
        :name="'terms'"
        v-model="formData.termsAccepted"
      />
    </policy-provider>
    
    <button type="submit" :disabled="!formData.termsAccepted">
      Continue Registration
    </button>
  </form>
</template>

<script setup>
const formData = ref({
  termsAccepted: false
})

const validationErrors = ref({
  terms: false
})

const handleSubmit = () => {
  validationErrors.value.terms = !formData.value.termsAccepted
  
  if (formData.value.termsAccepted) {
    // Proceed with form submission
    console.log('Terms accepted, proceeding...')
  }
}
</script>
```

### Privacy Policy Consent
```vue
<template>
  <div class="privacy-consent">
    <h3>Privacy Settings</h3>
    
    <div class="consent-group">
      <policy-provider
        :computed-name="'I consent to the processing of my personal data'"
        :true-value="true"
        :false-value="false"
        :required="true"
      >
        <codex-policy
          :id="'privacy-consent'"
          :name="'privacy_consent'"
          v-model="privacySettings.dataProcessing"
        />
      </policy-provider>
    </div>
    
    <div class="consent-group">
      <policy-provider
        :computed-name="'I agree to receive marketing communications'"
        :true-value="true"
        :false-value="false"
      >
        <codex-policy
          :id="'marketing-consent'"
          :name="'marketing_consent'"
          v-model="privacySettings.marketing"
        />
      </policy-provider>
    </div>
  </div>
</template>

<script setup>
const privacySettings = ref({
  dataProcessing: false,
  marketing: false
})

// Watch for changes to save preferences
watch(privacySettings, (newSettings) => {
  console.log('Privacy settings updated:', newSettings)
}, { deep: true })
</script>
```

### Legal Document Signature
```vue
<template>
  <div class="legal-signature">
    <div class="document-content">
      <h2>Service Agreement</h2>
      <div class="document-text">
        <!-- Document content -->
        <p>By signing below, you acknowledge...</p>
      </div>
    </div>
    
    <div class="signature-section">
      <policy-provider
        :computed-name="signatureName"
        :true-value="getCurrentTimestamp()"
        :false-value="null"
        :required="true"
        :has-error="!isValidSignature"
      >
        <codex-policy
          :id="'document-signature'"
          :name="'legal_signature'"
          v-model="documentSignature"
        />
      </policy-provider>
      
      <p class="signature-date" v-if="documentSignature">
        Signed on: {{ formatDate(documentSignature) }}
      </p>
    </div>
  </div>
</template>

<script setup>
const documentSignature = ref(null)
const signerName = ref('John Doe')

const signatureName = computed(() => 
  `${signerName.value} - Electronic Signature`
)

const isValidSignature = computed(() => 
  documentSignature.value !== null
)

const getCurrentTimestamp = () => new Date().toISOString()

const formatDate = (timestamp) => {
  return new Date(timestamp).toLocaleString()
}
</script>
```

### Multi-step Registration Flow
```vue
<template>
  <div class="registration-flow">
    <div class="step" v-if="currentStep === 1">
      <h3>Step 1: Account Information</h3>
      <!-- Account form fields -->
      
      <button @click="nextStep" :disabled="!canProceedStep1">
        Next: Review Terms
      </button>
    </div>
    
    <div class="step" v-if="currentStep === 2">
      <h3>Step 2: Terms and Agreements</h3>
      
      <div class="terms-list">
        <div class="term-item">
          <policy-provider
            :computed-name="'Terms of Service Agreement'"
            :true-value="'accepted'"
            :false-value="'declined'"
            :required="true"
          >
            <codex-policy
              :id="'tos-agreement'"
              :name="'terms_of_service'"
              v-model="agreements.termsOfService"
            />
          </policy-provider>
        </div>
        
        <div class="term-item">
          <policy-provider
            :computed-name="'Privacy Policy Acknowledgment'"
            :true-value="'acknowledged'"
            :false-value="'not_acknowledged'"
            :required="true"
          >
            <codex-policy
              :id="'privacy-acknowledgment'"
              :name="'privacy_policy'"
              v-model="agreements.privacyPolicy"
            />
          </policy-provider>
        </div>
      </div>
      
      <div class="step-actions">
        <button @click="previousStep">Back</button>
        <button @click="completeRegistration" :disabled="!allAgreementsAccepted">
          Complete Registration
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
const currentStep = ref(1)
const agreements = ref({
  termsOfService: 'declined',
  privacyPolicy: 'not_acknowledged'
})

const canProceedStep1 = ref(false) // Based on form validation

const allAgreementsAccepted = computed(() => 
  agreements.value.termsOfService === 'accepted' && 
  agreements.value.privacyPolicy === 'acknowledged'
)

const nextStep = () => {
  if (canProceedStep1.value) {
    currentStep.value = 2
  }
}

const previousStep = () => {
  currentStep.value = 1
}

const completeRegistration = () => {
  if (allAgreementsAccepted.value) {
    console.log('Registration completed with agreements:', agreements.value)
  }
}
</script>
```

### Dynamic Policy Loading
```vue
<template>
  <div class="dynamic-policies">
    <h3>Required Agreements</h3>
    
    <div v-if="policiesLoading" class="loading">
      Loading policies...
    </div>
    
    <div v-else class="policy-list">
      <div
        v-for="policy in requiredPolicies"
        :key="policy.id"
        class="policy-item"
      >
        <div class="policy-details">
          <h4>{{ policy.title }}</h4>
          <p>{{ policy.description }}</p>
          <a :href="policy.documentUrl" target="_blank">
            View Full Document
          </a>
        </div>
        
        <policy-provider
          :computed-name="policy.agreementText"
          :true-value="policy.id"
          :false-value="null"
          :required="policy.required"
          :has-error="policyErrors[policy.id]"
        >
          <codex-policy
            :id="`policy-${policy.id}`"
            :name="`policy_${policy.id}`"
            v-model="policyAgreements[policy.id]"
          />
        </policy-provider>
      </div>
    </div>
  </div>
</template>

<script setup>
const policiesLoading = ref(true)
const requiredPolicies = ref([])
const policyAgreements = ref({})
const policyErrors = ref({})

onMounted(async () => {
  try {
    // Load policies from API
    const response = await fetch('/api/policies/required')
    requiredPolicies.value = await response.json()
    
    // Initialize agreement state
    requiredPolicies.value.forEach(policy => {
      policyAgreements.value[policy.id] = null
      policyErrors.value[policy.id] = false
    })
    
    policiesLoading.value = false
  } catch (error) {
    console.error('Failed to load policies:', error)
    policiesLoading.value = false
  }
})

const validatePolicies = () => {
  let hasErrors = false
  
  requiredPolicies.value.forEach(policy => {
    if (policy.required && !policyAgreements.value[policy.id]) {
      policyErrors.value[policy.id] = true
      hasErrors = true
    } else {
      policyErrors.value[policy.id] = false
    }
  })
  
  return !hasErrors
}
</script>
```

### Conditional Policy Rendering
```vue
<template>
  <div class="conditional-policies">
    <div class="user-type-selection">
      <label>
        <input type="radio" v-model="userType" value="individual" />
        Individual User
      </label>
      <label>
        <input type="radio" v-model="userType" value="business" />
        Business User
      </label>
    </div>
    
    <!-- Common policies for all users -->
    <div class="common-policies">
      <policy-provider
        :computed-name="'General Terms of Service'"
        :true-value="true"
        :false-value="false"
        :required="true"
      >
        <codex-policy
          :id="'general-terms'"
          :name="'general_terms'"
          v-model="commonPolicies.generalTerms"
        />
      </policy-provider>
    </div>
    
    <!-- Individual user specific policies -->
    <div v-if="userType === 'individual'" class="individual-policies">
      <policy-provider
        :computed-name="'Personal Data Processing Agreement'"
        :true-value="true"
        :false-value="false"
        :required="true"
      >
        <codex-policy
          :id="'personal-data'"
          :name="'personal_data_processing'"
          v-model="individualPolicies.personalData"
        />
      </policy-provider>
    </div>
    
    <!-- Business user specific policies -->
    <div v-if="userType === 'business'" class="business-policies">
      <policy-provider
        :computed-name="'Business Service Agreement'"
        :true-value="true"
        :false-value="false"
        :required="true"
      >
        <codex-policy
          :id="'business-service'"
          :name="'business_service_agreement'"
          v-model="businessPolicies.serviceAgreement"
        />
      </policy-provider>
      
      <policy-provider
        :computed-name="'Data Processing Addendum'"
        :true-value="true"
        :false-value="false"
        :required="true"
      >
        <codex-policy
          :id="'data-processing'"
          :name="'data_processing_addendum'"
          v-model="businessPolicies.dataProcessing"
        />
      </policy-provider>
    </div>
  </div>
</template>

<script setup>
const userType = ref('individual')
const commonPolicies = ref({
  generalTerms: false
})
const individualPolicies = ref({
  personalData: false
})
const businessPolicies = ref({
  serviceAgreement: false,
  dataProcessing: false
})

// Reset specific policies when user type changes
watch(userType, (newType, oldType) => {
  if (oldType === 'individual') {
    individualPolicies.value = { personalData: false }
  } else if (oldType === 'business') {
    businessPolicies.value = {
      serviceAgreement: false,
      dataProcessing: false
    }
  }
})
</script>
```

### Policy with Custom Validation
```vue
<template>
  <div class="policy-validation">
    <form @submit.prevent="handleSubmit">
      <policy-provider
        :computed-name="agreementText"
        :true-value="'digitally-signed'"
        :false-value="'unsigned'"
        :required="true"
        :has-error="validationState.hasError"
        :aria-label="validationState.ariaLabel"
      >
        <codex-policy
          :id="'validated-policy'"
          :name="'policy_signature'"
          v-model="policySignature"
          :dusk="'policy-signature-checkbox'"
        />
      </policy-provider>
      
      <div v-if="validationState.hasError" class="error-message">
        {{ validationState.errorMessage }}
      </div>
      
      <div v-if="signatureMetadata.timestamp" class="signature-info">
        <p>Signed by: {{ signatureMetadata.signer }}</p>
        <p>Timestamp: {{ formatTimestamp(signatureMetadata.timestamp) }}</p>
        <p>IP Address: {{ signatureMetadata.ipAddress }}</p>
      </div>
      
      <button type="submit">Submit Agreement</button>
    </form>
  </div>
</template>

<script setup>
const policySignature = ref('unsigned')
const signatureMetadata = ref({
  signer: '',
  timestamp: null,
  ipAddress: ''
})

const validationState = ref({
  hasError: false,
  errorMessage: '',
  ariaLabel: ''
})

const agreementText = computed(() => {
  if (policySignature.value === 'digitally-signed') {
    return `Digitally signed by ${signatureMetadata.value.signer}`
  }
  return 'I acknowledge and agree to the terms stated above'
})

// Watch for signature changes
watch(policySignature, (newValue) => {
  if (newValue === 'digitally-signed') {
    // Capture signature metadata
    signatureMetadata.value = {
      signer: 'Current User', // Get from auth context
      timestamp: new Date().toISOString(),
      ipAddress: 'XXX.XXX.XXX.XXX' // Get from request context
    }
    validationState.value.hasError = false
  } else {
    // Clear metadata when unsigned
    signatureMetadata.value = {
      signer: '',
      timestamp: null,
      ipAddress: ''
    }
  }
})

const handleSubmit = () => {
  // Validate signature
  if (policySignature.value !== 'digitally-signed') {
    validationState.value = {
      hasError: true,
      errorMessage: 'You must agree to the terms to continue',
      ariaLabel: 'Agreement required - please check the agreement checkbox'
    }
    return
  }
  
  // Submit with signature metadata
  console.log('Submitting agreement with signature:', {
    signature: policySignature.value,
    metadata: signatureMetadata.value
  })
}

const formatTimestamp = (timestamp) => {
  return new Date(timestamp).toLocaleString()
}
</script>
```

## CSS Classes
- `_c-signature-input`: Applied to the checkbox input element
- `_c-signature-box`: Applied to the label element that acts as the visual signature box
- `_c-error`: Applied to the label when hasError injection is true

## Best Practices

### Recommended Usage Patterns
- Always wrap in a provider component that injects required configuration
- Use descriptive policy text through computedName injection
- Implement proper validation for required policies
- Provide clear visual indication of policy acceptance state
- Use appropriate true/false values that match your data model
- Test accessibility with screen readers
- Handle error states appropriately with clear user feedback

### Common Pitfalls to Avoid
- Using the component without proper provider context
- Not providing required injections (computedName, trueValue, falseValue)
- Missing validation for required policy acceptance
- Not handling error states visually or programmatically
- Using inappropriate values for true/false that don't match backend expectations
- Not providing proper accessibility attributes
- Not testing the component with keyboard navigation

### Accessibility Considerations
- Ensure proper label association through id and for attributes
- Provide meaningful aria-label text through injection
- Support keyboard navigation and activation
- Use semantic checkbox input for screen reader compatibility
- Provide clear error messages when validation fails
- Test with screen readers for proper state announcements
- Ensure sufficient color contrast for all visual states

### Validation Considerations
- Implement server-side validation for policy acceptance
- Provide immediate feedback for validation errors
- Handle required field validation appropriately
- Store acceptance metadata (timestamp, IP, user info) for legal compliance
- Validate true/false values match expected format
- Handle form submission only when all required policies are accepted

### Legal Compliance
- Store audit trails for policy acceptance including timestamps and user identification
- Provide mechanisms for users to review accepted policies
- Handle policy version changes and re-acceptance requirements
- Ensure digital signature legal validity where required
- Implement proper data retention policies for agreement records
- Provide users with copies of accepted agreements

### State Management
- Use reactive state for policy acceptance tracking
- Handle state persistence across sessions when appropriate
- Coordinate policy state with form validation systems
- Manage multiple policy agreements efficiently
- Handle policy dependencies and conditional requirements
- Provide undo functionality where legally appropriate

### Provider Pattern Implementation
- Design reusable provider components for different policy types
- Implement proper dependency injection hierarchies
- Handle provider context validation and error cases
- Design flexible provider APIs for different use cases
- Test provider/consumer relationships thoroughly
- Document provider requirements clearly

### Form Integration
- Integrate with broader form validation systems
- Handle form submission states properly
- Coordinate with other form field validation
- Provide proper form reset functionality
- Handle form field grouping and organization
- Support progressive form enhancement

### Error Handling
- Provide clear, actionable error messages
- Handle network errors during policy loading gracefully
- Implement retry mechanisms for failed policy operations
- Validate provider context and handle missing injections
- Handle edge cases in policy state management
- Provide fallback behavior for critical errors

### Data Security
- Ensure secure transmission of policy acceptance data
- Implement proper encryption for stored agreement records
- Handle sensitive policy data appropriately
- Validate policy data integrity
- Implement secure policy document delivery
- Handle policy data cleanup and retention properly

## Component Registration
The component is registered as `codex-policy` in the application. 