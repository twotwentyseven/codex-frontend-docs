# PolicyField Component

## Overview
The PolicyField component provides a specialized interface for policy acceptance and digital signature collection. It displays a policy overview with click-to-view functionality, opens the full policy in a modal, and allows users to digitally sign and accept the policy. The component integrates with the usePolicies composable to fetch policy content dynamically and provides a seamless policy acceptance flow with validation.

## Basic Usage
```vue
<template>
  <div class="policy-form">
    <codex-policy-field
      v-model="acceptedPolicy"
      :name="'privacy_policy'"
      :label="'I accept the Privacy Policy'"
      :other="{ handle: 'privacy-policy' }"
      :required="true"
    />
  </div>
</template>

<script setup>
const acceptedPolicy = ref(false)
</script>
```

## Key Features
- Policy overview display with title and excerpt
- Click-to-view policy modal with full content
- Digital signature collection and acceptance
- Dynamic policy loading via usePolicies composable
- Internationalization support for policy text
- Flexible layout configurations
- Label and hint system with tooltip support
- Helper text and error handling
- Form validation integration
- Testing support with Dusk attributes

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `name` | `String` | `required` | Field name for form submission |
| `other` | `Object` | `required` | Configuration object with policy handle |

### Other Configuration
| Property | Type | Description |
|----------|------|-------------|
| `other.handle` | `String` | Policy handle/identifier for loading |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the field |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |
| `placeholder` | `String` | `''` | Placeholder text |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'1'` | Layout size (auto, 4, 3, 2, 1) |

### Value Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `trueValue` | `String\|Boolean\|Number` | `true` | Value when policy is accepted |
| `falseValue` | `String\|Boolean\|Number` | `false` | Value when policy is not accepted |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `required` | `Boolean` | `true` | Whether policy acceptance is required |
| `hasError` | `Boolean` | `false` | Whether field has error state |
| `type` | `String` | `'policy'` | Input type identifier |

### Error Handling Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `errors` | `Array` | `[]` | Array of error messages |
| `error` | `Boolean\|String` | `false` | Single error state or message |
| `options` | `Array\|Object` | `[]` | Additional options configuration |

### Testing Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dusk` | `String` | `''` | Browser testing identifier |
| `id` | `String` | `''` | HTML element ID |

### Tooltip Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `tooltipText` | `String` | `''` | Tooltip text content |
| `tooltipIcon` | `String` | `''` | Tooltip icon class |
| `link` | `String` | `undefined` | Link URL for hint |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `close` | `none` | Emitted when hint is closed |
| `customEvent` | `event` | Custom event from hint component |

## Model Value

The component uses `defineModel({ type: [String, Boolean, Number], default: false })`:

```vue
<template>
  <codex-policy-field v-model="policyAccepted" />
</template>

<script setup>
const policyAccepted = ref(false)
// policyAccepted automatically updates when user accepts/rejects policy
// Value can be customized using trueValue/falseValue props
</script>
```

## Internationalization

The component uses the following translation keys:

| Key | Default | Usage |
|-----|---------|-------|
| `register.click_to_view_policy` | `"Click to view policy"` | Badge text on policy overview |
| `register.sign_policy` | `"Sign Policy"` | Modal accept button text |

## Examples

### Terms of Service and Privacy Policy
```vue
<template>
  <div class="legal-agreements">
    <h3>Legal Agreements</h3>
    
    <div class="agreements-form">
      <codex-policy-field
        v-model="agreements.termsOfService"
        :name="'terms_of_service'"
        :label="'I accept the Terms of Service'"
        :other="{ handle: 'terms-of-service' }"
        :layout="'1'"
        :required="true"
        :errors="agreementErrors.termsOfService"
        :helper-text="'Required to use our service'"
      />
      
      <codex-policy-field
        v-model="agreements.privacyPolicy"
        :name="'privacy_policy'"
        :label="'I accept the Privacy Policy'"
        :other="{ handle: 'privacy-policy' }"
        :layout="'1'"
        :required="true"
        :errors="agreementErrors.privacyPolicy"
        :helper-text="'Required for data processing'"
      />
      
      <codex-policy-field
        v-model="agreements.cookiePolicy"
        :name="'cookie_policy'"
        :label="'I accept the Cookie Policy'"
        :other="{ handle: 'cookie-policy' }"
        :layout="'1'"
        :required="false"
        :helper-text="'Optional - improves your experience'"
      />
      
      <codex-policy-field
        v-model="agreements.marketingConsent"
        :name="'marketing_consent'"
        :label="'I consent to marketing communications'"
        :other="{ handle: 'marketing-consent' }"
        :layout="'1'"
        :required="false"
        :helper-text="'You can unsubscribe at any time'"
      />
    </div>
    
    <div class="agreements-summary" v-if="hasAnyAgreements">
      <h4>Agreement Status</h4>
      
      <div class="agreement-status">
        <div class="status-item" :class="{ accepted: agreements.termsOfService }">
          <i :class="agreements.termsOfService ? 'ri-check-line' : 'ri-close-line'"></i>
          <span>Terms of Service</span>
          <span class="status-badge required">Required</span>
        </div>
        
        <div class="status-item" :class="{ accepted: agreements.privacyPolicy }">
          <i :class="agreements.privacyPolicy ? 'ri-check-line' : 'ri-close-line'"></i>
          <span>Privacy Policy</span>
          <span class="status-badge required">Required</span>
        </div>
        
        <div class="status-item" :class="{ accepted: agreements.cookiePolicy }">
          <i :class="agreements.cookiePolicy ? 'ri-check-line' : 'ri-close-line'"></i>
          <span>Cookie Policy</span>
          <span class="status-badge optional">Optional</span>
        </div>
        
        <div class="status-item" :class="{ accepted: agreements.marketingConsent }">
          <i :class="agreements.marketingConsent ? 'ri-check-line' : 'ri-close-line'"></i>
          <span>Marketing Communications</span>
          <span class="status-badge optional">Optional</span>
        </div>
      </div>
      
      <div class="completion-status">
        <div class="progress-indicator">
          <span class="progress-text">
            Required agreements: {{ requiredAgreementsCount }}/{{ totalRequiredAgreements }} completed
          </span>
          <div class="progress-bar">
            <div class="progress-fill" :style="{ width: agreementProgress + '%' }"></div>
          </div>
        </div>
      </div>
    </div>
    
    <div class="agreement-actions">
      <button @click="reviewAllPolicies" class="review-btn">
        Review All Policies
      </button>
      <button @click="proceedWithAgreements" :disabled="!canProceed" class="proceed-btn">
        Continue Registration
      </button>
    </div>
  </div>
</template>

<script setup>
const agreements = reactive({
  termsOfService: false,
  privacyPolicy: false,
  cookiePolicy: false,
  marketingConsent: false
})

const agreementErrors = ref({
  termsOfService: [],
  privacyPolicy: []
})

const hasAnyAgreements = computed(() => {
  return Object.values(agreements).some(agreement => agreement)
})

const requiredAgreements = ['termsOfService', 'privacyPolicy']
const totalRequiredAgreements = requiredAgreements.length

const requiredAgreementsCount = computed(() => {
  return requiredAgreements.filter(key => agreements[key]).length
})

const agreementProgress = computed(() => {
  return Math.round((requiredAgreementsCount.value / totalRequiredAgreements) * 100)
})

const canProceed = computed(() => {
  return requiredAgreements.every(key => agreements[key])
})

const reviewAllPolicies = () => {
  // Open all policies in separate tabs or a comprehensive review modal
  const policies = ['terms-of-service', 'privacy-policy', 'cookie-policy', 'marketing-consent']
  policies.forEach(policyHandle => {
    window.open(`/policies/${policyHandle}`, '_blank')
  })
}

const proceedWithAgreements = async () => {
  try {
    await submitUserAgreements(agreements)
    toast.success('Agreements recorded successfully')
    router.push('/registration/complete')
  } catch (error) {
    if (error.response?.data?.errors) {
      agreementErrors.value = error.response.data.errors
    }
    toast.error('Failed to record agreements')
  }
}
</script>
```

### Business Contract Acceptance
```vue
<template>
  <div class="business-contracts">
    <h3>Business Partnership Agreement</h3>
    
    <div class="contract-workflow">
      <div class="workflow-step" :class="{ active: currentStep === 1, completed: currentStep > 1 }">
        <h4>Step 1: Service Agreement</h4>
        <p>Review and accept the main service agreement</p>
        
        <codex-policy-field
          v-model="contracts.serviceAgreement"
          :name="'service_agreement'"
          :label="'I accept the Service Agreement'"
          :other="{ handle: 'business-service-agreement' }"
          :layout="'1'"
          :required="true"
          :errors="contractErrors.serviceAgreement"
          :helper-text="'Defines our service terms and responsibilities'"
        />
        
        <button v-if="contracts.serviceAgreement && currentStep === 1" @click="currentStep = 2" class="next-step-btn">
          Continue to SLA
        </button>
      </div>
      
      <div class="workflow-step" :class="{ active: currentStep === 2, completed: currentStep > 2 }">
        <h4>Step 2: Service Level Agreement</h4>
        <p>Accept the SLA defining performance standards</p>
        
        <codex-policy-field
          v-model="contracts.slaAgreement"
          :name="'sla_agreement'"
          :label="'I accept the Service Level Agreement'"
          :other="{ handle: 'business-sla' }"
          :layout="'1'"
          :required="true"
          :errors="contractErrors.slaAgreement"
          :helper-text="'Guarantees uptime and performance standards'"
        />
        
        <button v-if="contracts.slaAgreement && currentStep === 2" @click="currentStep = 3" class="next-step-btn">
          Continue to Data Processing
        </button>
      </div>
      
      <div class="workflow-step" :class="{ active: currentStep === 3, completed: currentStep > 3 }">
        <h4>Step 3: Data Processing Agreement</h4>
        <p>Required for GDPR compliance and data handling</p>
        
        <codex-policy-field
          v-model="contracts.dataProcessing"
          :name="'data_processing_agreement'"
          :label="'I accept the Data Processing Agreement'"
          :other="{ handle: 'business-dpa' }"
          :layout="'1'"
          :required="true"
          :errors="contractErrors.dataProcessing"
          :helper-text="'GDPR-compliant data processing terms'"
        />
        
        <button v-if="contracts.dataProcessing && currentStep === 3" @click="currentStep = 4" class="next-step-btn">
          Review Optional Agreements
        </button>
      </div>
      
      <div class="workflow-step" :class="{ active: currentStep === 4 }">
        <h4>Step 4: Optional Agreements</h4>
        <p>Additional services and partnerships</p>
        
        <div class="optional-agreements">
          <codex-policy-field
            v-model="contracts.supportAgreement"
            :name="'support_agreement'"
            :label="'I want Premium Support Services'"
            :other="{ handle: 'business-premium-support' }"
            :layout="'2'"
            :required="false"
            :helper-text="'24/7 priority support and dedicated account management'"
          />
          
          <codex-policy-field
            v-model="contracts.analyticsAgreement"
            :name="'analytics_agreement'"
            :label="'I consent to Enhanced Analytics'"
            :other="{ handle: 'business-analytics' }"
            :layout="'2'"
            :required="false"
            :helper-text="'Detailed usage analytics and reporting'"
          />
          
          <codex-policy-field
            v-model="contracts.marketingPartnership"
            :name="'marketing_partnership'"
            :label="'I agree to Marketing Partnership'"
            :other="{ handle: 'business-marketing-partnership' }"
            :layout="'1'"
            :required="false"
            :helper-text="'Co-marketing opportunities and case studies'"
          />
        </div>
      </div>
    </div>
    
    <div class="contract-summary" v-if="hasSignedContracts">
      <h4>Contract Summary</h4>
      
      <div class="signed-contracts">
        <h5>Required Agreements</h5>
        <div class="contract-list">
          <div v-for="contract in requiredContracts" :key="contract.key" class="contract-item" :class="{ signed: contracts[contract.key] }">
            <i :class="contracts[contract.key] ? 'ri-check-line' : 'ri-time-line'"></i>
            <span>{{ contract.name }}</span>
            <span v-if="contracts[contract.key]" class="signature-date">
              Signed: {{ formatSignatureDate(contract.key) }}
            </span>
          </div>
        </div>
        
        <h5>Optional Agreements</h5>
        <div class="contract-list">
          <div v-for="contract in optionalContracts" :key="contract.key" class="contract-item" :class="{ signed: contracts[contract.key] }">
            <i :class="contracts[contract.key] ? 'ri-check-line' : 'ri-close-line'"></i>
            <span>{{ contract.name }}</span>
            <span v-if="contracts[contract.key]" class="signature-date">
              Signed: {{ formatSignatureDate(contract.key) }}
            </span>
          </div>
        </div>
      </div>
      
      <div class="contract-value-summary">
        <div class="value-item">
          <h6>Base Service Package</h6>
          <span class="value">Included</span>
        </div>
        <div v-if="contracts.supportAgreement" class="value-item">
          <h6>Premium Support</h6>
          <span class="value">+$299/month</span>
        </div>
        <div v-if="contracts.analyticsAgreement" class="value-item">
          <h6>Enhanced Analytics</h6>
          <span class="value">+$99/month</span>
        </div>
        <div class="value-item total">
          <h6>Total Monthly Value</h6>
          <span class="value">${{ calculateTotalValue() }}/month</span>
        </div>
      </div>
    </div>
    
    <div class="contract-actions">
      <button @click="downloadContracts" :disabled="!allRequiredSigned" class="download-btn">
        Download Signed Contracts
      </button>
      <button @click="finalizeContracts" :disabled="!allRequiredSigned" class="finalize-btn">
        Finalize Partnership
      </button>
    </div>
  </div>
</template>

<script setup>
const currentStep = ref(1)

const contracts = reactive({
  serviceAgreement: false,
  slaAgreement: false,
  dataProcessing: false,
  supportAgreement: false,
  analyticsAgreement: false,
  marketingPartnership: false
})

const contractErrors = ref({
  serviceAgreement: [],
  slaAgreement: [],
  dataProcessing: []
})

const signatureDates = reactive({})

const requiredContracts = [
  { key: 'serviceAgreement', name: 'Service Agreement' },
  { key: 'slaAgreement', name: 'Service Level Agreement' },
  { key: 'dataProcessing', name: 'Data Processing Agreement' }
]

const optionalContracts = [
  { key: 'supportAgreement', name: 'Premium Support Services' },
  { key: 'analyticsAgreement', name: 'Enhanced Analytics' },
  { key: 'marketingPartnership', name: 'Marketing Partnership' }
]

const hasSignedContracts = computed(() => {
  return Object.values(contracts).some(signed => signed)
})

const allRequiredSigned = computed(() => {
  return requiredContracts.every(contract => contracts[contract.key])
})

const formatSignatureDate = (contractKey) => {
  return signatureDates[contractKey] || new Date().toLocaleDateString()
}

const calculateTotalValue = () => {
  let total = 0 // Base package
  if (contracts.supportAgreement) total += 299
  if (contracts.analyticsAgreement) total += 99
  return total
}

// Watch for contract signatures to record dates
Object.keys(contracts).forEach(contractKey => {
  watch(() => contracts[contractKey], (newValue) => {
    if (newValue && !signatureDates[contractKey]) {
      signatureDates[contractKey] = new Date().toISOString()
    }
  })
})

const downloadContracts = () => {
  // Generate and download signed contract bundle
  const signedContracts = Object.keys(contracts)
    .filter(key => contracts[key])
    .map(key => ({ 
      contract: key, 
      signedAt: signatureDates[key] 
    }))
  
  generateContractBundle(signedContracts)
}

const finalizeContracts = async () => {
  try {
    await finalizeBusinessPartnership({
      contracts,
      signatureDates,
      totalValue: calculateTotalValue()
    })
    
    toast.success('Partnership finalized successfully!')
    router.push('/business/onboarding/complete')
  } catch (error) {
    if (error.response?.data?.errors) {
      contractErrors.value = error.response.data.errors
    }
    toast.error('Failed to finalize partnership')
  }
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input-container` | Main container for the field |
| `_c-policy-overview` | Container for policy preview |
| `_c-signature-container` | Container for signature checkbox |
| `_c-form-field--auto` | Auto-sized layout |
| `_c-form-field--quarter` | Quarter width layout (25%) |
| `_c-form-field--third` | Third width layout (33%) |
| `_c-form-field--half` | Half width layout (50%) |
| `_c-form-field--full` | Full width layout (100%) |
| `_c-label-container` | Container for label and hint |
| `_c-subtitle` | Styling for policy title |
| `_c-badge` | Styling for click-to-view badge |
| `_c-card` | Modal card styling |
| `_c-header` | Modal header styling |
| `_c-content` | Modal content styling |
| `_c-footer` | Modal footer styling |
| `_c-policy-content` | Policy content styling |

## Best Practices

### Policy Management
- Use clear, descriptive policy handles for easy identification
- Ensure policies are loaded before component render
- Provide fallback content for policy loading states
- Version control policies and handle updates gracefully
- Cache policy content for better performance

### User Experience
- Provide clear policy summaries in the overview
- Make full policy content easily accessible
- Use progressive disclosure for complex policy sets
- Show clear indication of acceptance status
- Provide easy way to review accepted policies

### Legal Compliance
- Ensure proper consent collection and recording
- Store acceptance timestamps and IP addresses
- Provide clear opt-out mechanisms where required
- Handle policy updates and re-consent flows
- Maintain audit trails for compliance

### Accessibility
- Ensure policy content is screen reader accessible
- Provide clear instructions for policy acceptance
- Use appropriate ARIA labels for policy modals
- Test with keyboard navigation
- Ensure sufficient color contrast for acceptance indicators

### Data Protection
- Encrypt stored consent and signature data
- Implement proper data retention policies
- Provide mechanisms for consent withdrawal
- Handle GDPR right-to-be-forgotten requests
- Secure policy content against unauthorized access

### Performance
- Lazy load policy content to improve initial page load
- Cache policy data appropriately
- Optimize modal rendering for better UX
- Minimize re-renders during policy interaction
- Use efficient change detection for acceptance state

### Validation
- Validate required policy acceptance before form submission
- Provide clear error messages for missing acceptances
- Handle network errors during policy loading gracefully
- Validate policy signatures on both client and server
- Ensure proper form integration for acceptance validation

## Component Registration
```javascript
// Global registration
app.component('CodexPolicyField', PolicyField)

// Local registration  
import PolicyField from '@/components/fields/PolicyField.vue'

export default {
  components: {
    CodexPolicyField: PolicyField
  }
}
``` 