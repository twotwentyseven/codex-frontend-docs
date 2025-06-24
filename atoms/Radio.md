# Radio Component

## Overview
The Radio component provides an interactive radio button input with visual icon integration, v-model support, and dependency injection capabilities. It features single-selection behavior, visual feedback with Remix icons, accessibility attributes, and form integration for mutually exclusive selection interfaces.

## Basic Usage
```vue
<codex-radio
  v-model="selectedValue"
  :value="'option1'"
  :id="'option1-radio'"
  :dusk="'option1-radio'"
/>
```

## Key Features
- Two-way data binding with v-model support
- Single selection radio button behavior
- Visual icon feedback with Remix icons
- Value-based selection mechanism
- Dependency injection for configuration
- Accessibility attributes and ARIA support
- Custom label association through ID
- Radio group coordination

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| modelValue | Any | No | - | Radio group selected value for two-way binding |
| value | String | Yes | '' | Unique value for this radio option |
| dusk | String | No | '' | Testing identifier attribute |
| id | String | No | '' | Radio button and label ID for association |

### Injected Props
The component receives additional configuration through Vue's dependency injection:

| Injected Prop | Type | Default | Description |
|---------------|------|---------|-------------|
| ariaLabel | String | '' | ARIA label for accessibility |
| name | String | '' | Radio group name attribute |
| required | Boolean | false | Whether radio selection is required |

## Events
The component uses v-model which automatically handles:
- **Input Events**: Updates model value when radio is selected
- **Change Events**: Triggers when radio selection changes

## Visual Icons
The component uses Remix icons for visual feedback:
- **Selected State**: `ri-checkbox-circle-fill` (filled circle icon)
- **Unselected State**: `ri-checkbox-blank-circle-line` (empty circle outline)
- **Dynamic Display**: Icons change based on model value vs radio value

## Radio Group Behavior
The component implements standard radio button behavior:
- **Mutual Exclusion**: Only one radio in a group can be selected
- **Group Coordination**: Uses shared name attribute for grouping
- **Value Selection**: Model value matches the value prop of selected radio
- **Single Selection**: Selecting one radio deselects others in the group

## Value Matching
The component determines selection state through:
- **Strict Equality**: Uses `===` to compare model value with radio value
- **Type Sensitivity**: Values must match exactly in type and content
- **Dynamic Updates**: Visual state updates automatically when model changes

## Label Integration
The component includes an integrated label element:
- **Label Association**: Uses `for` attribute linking to radio ID
- **Click Handling**: Label clicks select the radio option
- **CSS Classes**: `_c-checkbox` for styling (shared with checkbox)
- **Icon Container**: Label contains visual feedback icons

## Accessibility Features
- **Label Association**: Proper radio-label relationship via ID
- **ARIA Labels**: Supports aria-label through injection
- **Group Navigation**: Standard radio group keyboard navigation
- **Required Indication**: Handles required field marking
- **Screen Reader**: Compatible with assistive technologies
- **Keyboard Support**: Arrow key navigation within radio groups

## Dependency Injection Integration
The component uses Vue's provide/inject pattern:
- **Form Integration**: Receives configuration from parent form components
- **Group Configuration**: Shares name and accessibility attributes
- **Required State**: Inherits required status from parent context
- **Accessibility**: Inherits ARIA attributes from form context

## CSS Classes
- `_c-checkbox-input`: Applied to the radio input element
- `_c-checkbox`: Applied to the label element (shared with checkbox)

## Internationalization
The Radio component does not include built-in internationalization. ARIA labels should be localized before injection.

## Examples

### Basic Radio Button
```vue
<codex-radio
  v-model="selectedOption"
  :value="'option1'"
  :id="'option1-radio'"
  :dusk="'option1-radio'"
/>
```

### Radio Group Selection
```vue
<template>
  <div class="radio-group">
    <fieldset>
      <legend>Choose Your Plan</legend>
      
      <div class="radio-option">
        <codex-radio
          v-model="selectedPlan"
          :value="'basic'"
          :id="'basic-plan'"
          :dusk="'basic-plan-radio'"
        />
        <label for="basic-plan">Basic Plan - $9.99/month</label>
      </div>
      
      <div class="radio-option">
        <codex-radio
          v-model="selectedPlan"
          :value="'premium'"
          :id="'premium-plan'"
          :dusk="'premium-plan-radio'"
        />
        <label for="premium-plan">Premium Plan - $19.99/month</label>
      </div>
      
      <div class="radio-option">
        <codex-radio
          v-model="selectedPlan"
          :value="'enterprise'"
          :id="'enterprise-plan'"
          :dusk="'enterprise-plan-radio'"
        />
        <label for="enterprise-plan">Enterprise Plan - $39.99/month</label>
      </div>
    </fieldset>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const selectedPlan = ref('basic')

provide('name', 'subscription-plan')
provide('required', true)
provide('ariaLabel', 'Subscription plan selection')
</script>
```

### Radio Group with Validation
```vue
<template>
  <form class="preference-form">
    <fieldset>
      <legend>Preferred Contact Method</legend>
      
      <div class="radio-options">
        <div class="radio-option">
          <codex-radio
            v-model="contactMethod"
            :value="'email'"
            :id="'contact-email'"
            :dusk="'contact-email-radio'"
          />
          <label for="contact-email">Email</label>
        </div>
        
        <div class="radio-option">
          <codex-radio
            v-model="contactMethod"
            :value="'phone'"
            :id="'contact-phone'"
            :dusk="'contact-phone-radio'"
          />
          <label for="contact-phone">Phone</label>
        </div>
        
        <div class="radio-option">
          <codex-radio
            v-model="contactMethod"
            :value="'mail'"
            :id="'contact-mail'"
            :dusk="'contact-mail-radio'"
          />
          <label for="contact-mail">Mail</label>
        </div>
      </div>
      
      <codex-error v-if="contactError" :error="contactError" />
    </fieldset>
  </form>
</template>

<script setup>
import { provide, ref, watch } from 'vue'

const contactMethod = ref('')
const contactError = ref('')

const validateContact = () => {
  if (!contactMethod.value) {
    contactError.value = 'Please select a preferred contact method'
  } else {
    contactError.value = ''
  }
}

provide('name', 'contact-method')
provide('required', true)
provide('ariaLabel', 'Preferred contact method')

watch(contactMethod, validateContact)
</script>
```

### Conditional Radio Options
```vue
<template>
  <div class="conditional-radio">
    <fieldset>
      <legend>Shipping Method</legend>
      
      <div class="radio-options">
        <div class="radio-option">
          <codex-radio
            v-model="shippingMethod"
            :value="'standard'"
            :id="'shipping-standard'"
            :dusk="'shipping-standard-radio'"
          />
          <label for="shipping-standard">
            Standard Shipping (5-7 days) - Free
          </label>
        </div>
        
        <div class="radio-option">
          <codex-radio
            v-model="shippingMethod"
            :value="'express'"
            :id="'shipping-express'"
            :dusk="'shipping-express-radio'"
          />
          <label for="shipping-express">
            Express Shipping (2-3 days) - $9.99
          </label>
        </div>
        
        <div v-if="isPremiumCustomer" class="radio-option">
          <codex-radio
            v-model="shippingMethod"
            :value="'overnight'"
            :id="'shipping-overnight'"
            :dusk="'shipping-overnight-radio'"
          />
          <label for="shipping-overnight">
            Overnight Shipping (1 day) - $19.99
          </label>
        </div>
      </div>
    </fieldset>
    
    <div v-if="shippingMethod === 'express'" class="shipping-note">
      Express shipping includes tracking and insurance.
    </div>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const shippingMethod = ref('standard')
const isPremiumCustomer = ref(true)

provide('name', 'shipping-method')
provide('required', true)
</script>
```

### Dynamic Radio Options
```vue
<template>
  <div class="dynamic-radio">
    <fieldset>
      <legend>Select Your Country</legend>
      
      <div class="radio-options">
        <div 
          v-for="country in availableCountries" 
          :key="country.code"
          class="radio-option"
        >
          <codex-radio
            v-model="selectedCountry"
            :value="country.code"
            :id="`country-${country.code}`"
            :dusk="`country-${country.code}-radio`"
          />
          <label :for="`country-${country.code}`">
            {{ country.name }}
          </label>
        </div>
      </div>
      
      <div v-if="selectedCountry" class="selection-info">
        Selected: {{ getCountryName(selectedCountry) }}
      </div>
    </fieldset>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const availableCountries = ref([
  { code: 'us', name: 'United States' },
  { code: 'ca', name: 'Canada' },
  { code: 'uk', name: 'United Kingdom' },
  { code: 'de', name: 'Germany' },
  { code: 'fr', name: 'France' }
])

const selectedCountry = ref('')

const getCountryName = (code) => {
  const country = availableCountries.value.find(c => c.code === code)
  return country ? country.name : ''
}

provide('name', 'country-selection')
provide('required', true)
provide('ariaLabel', 'Country selection')
</script>
```

### Radio Group in Form Steps
```vue
<template>
  <div class="form-steps">
    <div class="step-content">
      <fieldset>
        <legend>{{ currentStepTitle }}</legend>
        
        <div class="radio-options">
          <div 
            v-for="option in currentStepOptions" 
            :key="option.value"
            class="radio-option"
          >
            <codex-radio
              v-model="stepAnswers[currentStep]"
              :value="option.value"
              :id="`step${currentStep}-${option.value}`"
              :dusk="`step${currentStep}-${option.value}-radio`"
            />
            <label :for="`step${currentStep}-${option.value}`">
              {{ option.label }}
            </label>
          </div>
        </div>
      </fieldset>
    </div>
    
    <div class="step-navigation">
      <button @click="previousStep" :disabled="currentStep === 1">
        Previous
      </button>
      <button @click="nextStep" :disabled="!canProceed">
        {{ currentStep === totalSteps ? 'Complete' : 'Next' }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const currentStep = ref(1)
const totalSteps = ref(3)

const stepData = ref({
  1: {
    title: 'Experience Level',
    options: [
      { value: 'beginner', label: 'Beginner' },
      { value: 'intermediate', label: 'Intermediate' },
      { value: 'advanced', label: 'Advanced' }
    ]
  },
  2: {
    title: 'Preferred Framework',
    options: [
      { value: 'vue', label: 'Vue.js' },
      { value: 'react', label: 'React' },
      { value: 'angular', label: 'Angular' }
    ]
  },
  3: {
    title: 'Project Type',
    options: [
      { value: 'personal', label: 'Personal Project' },
      { value: 'commercial', label: 'Commercial Project' },
      { value: 'educational', label: 'Educational Project' }
    ]
  }
})

const stepAnswers = ref({})

const currentStepTitle = computed(() => stepData.value[currentStep.value].title)
const currentStepOptions = computed(() => stepData.value[currentStep.value].options)
const canProceed = computed(() => !!stepAnswers.value[currentStep.value])

const nextStep = () => {
  if (currentStep.value < totalSteps.value) {
    currentStep.value++
  }
}

const previousStep = () => {
  if (currentStep.value > 1) {
    currentStep.value--
  }
}

provide('name', computed(() => `step-${currentStep.value}`))
provide('required', true)
</script>
```

### Radio Group with Descriptions
```vue
<template>
  <div class="radio-with-descriptions">
    <fieldset>
      <legend>Choose Account Type</legend>
      
      <div class="radio-options">
        <div class="radio-option-card">
          <div class="radio-header">
            <codex-radio
              v-model="accountType"
              :value="'individual'"
              :id="'account-individual'"
              :dusk="'account-individual-radio'"
            />
            <label for="account-individual" class="account-title">
              Individual Account
            </label>
          </div>
          <div class="account-description">
            Perfect for personal use and small projects. 
            Includes basic features and support.
          </div>
        </div>
        
        <div class="radio-option-card">
          <div class="radio-header">
            <codex-radio
              v-model="accountType"
              :value="'business'"
              :id="'account-business'"
              :dusk="'account-business-radio'"
            />
            <label for="account-business" class="account-title">
              Business Account
            </label>
          </div>
          <div class="account-description">
            Designed for teams and businesses. Includes collaboration 
            tools, priority support, and advanced features.
          </div>
        </div>
        
        <div class="radio-option-card">
          <div class="radio-header">
            <codex-radio
              v-model="accountType"
              :value="'enterprise'"
              :id="'account-enterprise'"
              :dusk="'account-enterprise-radio'"
            />
            <label for="account-enterprise" class="account-title">
              Enterprise Account
            </label>
          </div>
          <div class="account-description">
            For large organizations with custom requirements. 
            Includes dedicated support, custom integrations, and SLA.
          </div>
        </div>
      </div>
    </fieldset>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const accountType = ref('')

provide('name', 'account-type')
provide('required', true)
provide('ariaLabel', 'Account type selection')
</script>
```

### Radio Group with Pricing
```vue
<template>
  <div class="pricing-radio">
    <fieldset>
      <legend>Select Billing Cycle</legend>
      
      <div class="pricing-options">
        <div 
          v-for="option in billingOptions" 
          :key="option.value"
          class="pricing-option"
          :class="{ 'recommended': option.recommended }"
        >
          <div class="pricing-header">
            <codex-radio
              v-model="billingCycle"
              :value="option.value"
              :id="`billing-${option.value}`"
              :dusk="`billing-${option.value}-radio`"
            />
            <label :for="`billing-${option.value}`" class="pricing-title">
              {{ option.title }}
            </label>
            <div v-if="option.recommended" class="recommended-badge">
              Recommended
            </div>
          </div>
          
          <div class="pricing-details">
            <div class="price">{{ option.price }}</div>
            <div class="savings" v-if="option.savings">
              Save {{ option.savings }}
            </div>
          </div>
        </div>
      </div>
    </fieldset>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const billingCycle = ref('monthly')

const billingOptions = ref([
  {
    value: 'monthly',
    title: 'Monthly',
    price: '$19.99/month',
    recommended: false
  },
  {
    value: 'annual',
    title: 'Annual',
    price: '$199.99/year',
    savings: '17%',
    recommended: true
  },
  {
    value: 'lifetime',
    title: 'Lifetime',
    price: '$499.99 once',
    savings: '79%',
    recommended: false
  }
])

provide('name', 'billing-cycle')
provide('required', true)
</script>
```

### Radio Group with Custom Styling
```vue
<template>
  <div class="custom-radio-group">
    <fieldset>
      <legend>Select Theme</legend>
      
      <div class="theme-options">
        <div 
          v-for="theme in themes" 
          :key="theme.value"
          class="theme-option"
        >
          <codex-radio
            v-model="selectedTheme"
            :value="theme.value"
            :id="`theme-${theme.value}`"
            :dusk="`theme-${theme.value}-radio`"
            class="custom-radio"
          />
          <label :for="`theme-${theme.value}`" class="theme-label">
            <div class="theme-preview" :style="{ backgroundColor: theme.color }"></div>
            <span class="theme-name">{{ theme.name }}</span>
          </label>
        </div>
      </div>
    </fieldset>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const selectedTheme = ref('light')

const themes = ref([
  { value: 'light', name: 'Light', color: '#ffffff' },
  { value: 'dark', name: 'Dark', color: '#1f2937' },
  { value: 'blue', name: 'Blue', color: '#3b82f6' },
  { value: 'green', name: 'Green', color: '#10b981' }
])

provide('name', 'theme-selection')
</script>

<style scoped>
.custom-radio .ri-checkbox-circle-fill {
  color: #3b82f6;
  font-size: 1.2rem;
}

.custom-radio .ri-checkbox-blank-circle-line {
  color: #6b7280;
  font-size: 1.2rem;
}

.theme-preview {
  width: 2rem;
  height: 2rem;
  border-radius: 0.25rem;
  border: 2px solid #e5e7eb;
}

.theme-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-weight: 500;
}
</style>
```

## CSS Classes
- `_c-checkbox-input`: Radio input element styling
- `_c-checkbox`: Label element styling (shared with checkbox component)

## Best Practices

### Recommended Usage Patterns
- Use clear and descriptive labels for radio options
- Provide unique IDs for proper label association
- Group related radio buttons with same name attribute
- Handle validation errors gracefully
- Use fieldset and legend for radio groups
- Provide meaningful ARIA labels for accessibility
- Use appropriate default selections when applicable
- Handle form integration through dependency injection

### Common Pitfalls to Avoid
- Not providing unique IDs for radio-label association
- Missing or unclear option labels
- Not grouping related radios with same name
- Forgetting fieldset/legend structure for groups
- Not handling validation errors visually
- Missing accessibility attributes
- Using radio buttons for multi-select (use checkboxes instead)

### Accessibility Considerations
- Always associate radios with labels via ID
- Use fieldset and legend for radio groups
- Provide clear and descriptive option labels
- Use ARIA labels for additional context
- Support keyboard navigation (arrow keys)
- Ensure sufficient color contrast for icons
- Support screen reader announcements
- Handle focus management within groups

### Radio Group Design
- Use fieldset and legend for semantic grouping
- Provide clear group titles with legend elements
- Organize options in logical order
- Use consistent styling across options
- Consider visual hierarchy for option importance
- Handle responsive design for option layouts
- Group related options together

### Value Management
- Use descriptive values that match your data model
- Keep values consistent and meaningful
- Handle default selections appropriately
- Coordinate radio values with backend expectations
- Consider value types for data consistency
- Handle empty or null states gracefully

### Form Integration
- Use dependency injection for form context sharing
- Coordinate with validation systems
- Handle form field relationships properly
- Provide clear form structure with radio groups
- Support form reset and clear functionality
- Handle form accessibility requirements
- Integrate with form submission handling

### Validation Handling
- Validate required radio selections
- Provide clear validation feedback
- Show errors near relevant radio groups
- Clear errors when valid selection is made
- Handle edge cases in radio validation
- Support group-level validation rules

### State Management
- Use reactive properties for radio values
- Handle state changes with v-model
- Manage validation states consistently
- Track radio selection states
- Handle component lifecycle properly
- Coordinate with parent component state

### User Experience
- Provide clear visual feedback for selection
- Use appropriate spacing between options
- Consider option descriptions when helpful
- Handle large option lists appropriately
- Support keyboard navigation within groups
- Provide instant feedback for selection changes

### Performance Considerations
- Minimize re-renders for large radio groups
- Use computed properties for derived radio states
- Handle dynamic option lists efficiently
- Optimize radio group updates
- Implement proper cleanup for watchers

## Component Registration
The component is registered as `codex-radio` in the application. 