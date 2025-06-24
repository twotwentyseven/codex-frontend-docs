# RadioField Component

## Overview
The RadioField component provides a radio button group input with comprehensive option management, validation, and form integration. It supports flexible option rendering, grouped radio buttons, layout configurations, and accessibility features while maintaining seamless integration with the form validation system through provide/inject patterns.

## Basic Usage
```vue
<template>
  <div class="radio-form">
    <codex-radio-field
      v-model="selectedOption"
      :name="'payment_method'"
      :label="'Payment Method'"
      :options="paymentOptions"
      :required="true"
    />
  </div>
</template>

<script setup>
const selectedOption = ref('')

const paymentOptions = [
  { value: 'credit', label: 'Credit Card' },
  { value: 'debit', label: 'Debit Card' },
  { value: 'paypal', label: 'PayPal' },
  { value: 'bank', label: 'Bank Transfer' }
]
</script>
```

## Key Features
- Radio button group with single selection
- Flexible option configuration and rendering
- Label and hint system with tooltip support
- Helper text and error handling
- Flexible layout configurations
- Accessibility features with proper grouping
- Testing support with Dusk attributes
- Integration with radio group components

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `name` | `String` | `required` | Field name for form submission |

### Options Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `options` | `Array` | `[]` | Array of radio button options |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
| `placeholder` | `String` | `''` | Placeholder text |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the radio group |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'1'` | Layout size (auto, 4, 3, 2, 1) |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `required` | `Boolean` | `true` | Whether field is required |
| `hasError` | `Boolean` | `false` | Whether field has error state |

### Error Handling Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `errors` | `Array` | `[]` | Array of error messages |
| `error` | `Boolean\|String` | `false` | Single error state or message |

### Testing Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dusk` | `String` | `''` | Browser testing identifier |
| `id` | `String` | `''` | HTML element ID |
| `type` | `String` | `'radio'` | Input type for radio |

### Tooltip Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `tooltipText` | `String` | `''` | Tooltip text content |
| `tooltipIcon` | `String` | `''` | Tooltip icon class |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `close` | `none` | Emitted when hint is closed |
| `customEvent` | `event` | Custom event from hint component |

## Model Value

The component uses `defineModel({ type: String })`:

```vue
<template>
  <codex-radio-field v-model="selection" :options="options" />
</template>

<script setup>
const selection = ref('')
// selection automatically updates with selected option value
</script>
```

## Examples

### Survey Form
```vue
<template>
  <div class="customer-survey">
    <h3>Customer Satisfaction Survey</h3>
    
    <div class="survey-questions">
      <codex-radio-field
        v-model="survey.satisfaction"
        :name="'satisfaction'"
        :label="'How satisfied are you with our service?'"
        :options="satisfactionOptions"
        :layout="'1'"
        :required="true"
        :errors="surveyErrors.satisfaction"
        :helper-text="'Please rate your overall experience'"
      />
      
      <codex-radio-field
        v-model="survey.recommendation"
        :name="'recommendation'"
        :label="'How likely are you to recommend us?'"
        :options="recommendationOptions"
        :layout="'1'"
        :required="true"
        :errors="surveyErrors.recommendation"
        :helper-text="'Scale from 1 (very unlikely) to 5 (very likely)'"
      />
      
      <codex-radio-field
        v-model="survey.frequency"
        :name="'frequency'"
        :label="'How often do you use our service?'"
        :options="frequencyOptions"
        :layout="'2'"
        :required="true"
        :helper-text="'Usage frequency helps us improve'"
      />
      
      <codex-radio-field
        v-model="survey.preferredContact"
        :name="'preferred_contact'"
        :label="'Preferred contact method'"
        :options="contactOptions"
        :layout="'2'"
        :required="false"
        :helper-text="'How should we reach you for follow-ups?'"
      />
    </div>
    
    <div class="survey-summary" v-if="surveyCompleteness > 0">
      <h4>Survey Progress</h4>
      <div class="progress-indicator">
        <div class="progress-bar">
          <div class="progress-fill" :style="{ width: surveyCompleteness + '%' }"></div>
        </div>
        <span>{{ surveyCompleteness }}% Complete</span>
      </div>
      
      <div class="answer-summary">
        <div v-if="survey.satisfaction" class="answer-item">
          <strong>Satisfaction:</strong> {{ getSatisfactionLabel() }}
        </div>
        <div v-if="survey.recommendation" class="answer-item">
          <strong>Recommendation:</strong> {{ getRecommendationLabel() }}
        </div>
        <div v-if="survey.frequency" class="answer-item">
          <strong>Usage:</strong> {{ getFrequencyLabel() }}
        </div>
      </div>
    </div>
    
    <div class="survey-actions">
      <button @click="submitSurvey" :disabled="!canSubmitSurvey" class="submit-btn">
        Submit Survey
      </button>
      <button @click="saveDraft" class="draft-btn">
        Save Draft
      </button>
    </div>
  </div>
</template>

<script setup>
const survey = reactive({
  satisfaction: '',
  recommendation: '',
  frequency: '',
  preferredContact: ''
})

const surveyErrors = ref({
  satisfaction: [],
  recommendation: []
})

const satisfactionOptions = [
  { value: 'very_satisfied', label: 'Very Satisfied' },
  { value: 'satisfied', label: 'Satisfied' },
  { value: 'neutral', label: 'Neutral' },
  { value: 'dissatisfied', label: 'Dissatisfied' },
  { value: 'very_dissatisfied', label: 'Very Dissatisfied' }
]

const recommendationOptions = [
  { value: '5', label: '5 - Extremely Likely' },
  { value: '4', label: '4 - Very Likely' },
  { value: '3', label: '3 - Somewhat Likely' },
  { value: '2', label: '2 - Not Very Likely' },
  { value: '1', label: '1 - Not Likely At All' }
]

const frequencyOptions = [
  { value: 'daily', label: 'Daily' },
  { value: 'weekly', label: 'Weekly' },
  { value: 'monthly', label: 'Monthly' },
  { value: 'occasionally', label: 'Occasionally' },
  { value: 'first_time', label: 'First Time User' }
]

const contactOptions = [
  { value: 'email', label: 'Email' },
  { value: 'phone', label: 'Phone Call' },
  { value: 'sms', label: 'Text Message' },
  { value: 'no_contact', label: 'No Follow-up Needed' }
]

const surveyCompleteness = computed(() => {
  const requiredFields = ['satisfaction', 'recommendation', 'frequency']
  const completedRequired = requiredFields.filter(field => survey[field]).length
  return Math.round((completedRequired / requiredFields.length) * 100)
})

const canSubmitSurvey = computed(() => {
  return survey.satisfaction && survey.recommendation && survey.frequency
})

const getSatisfactionLabel = () => {
  return satisfactionOptions.find(opt => opt.value === survey.satisfaction)?.label || ''
}

const getRecommendationLabel = () => {
  return recommendationOptions.find(opt => opt.value === survey.recommendation)?.label || ''
}

const getFrequencyLabel = () => {
  return frequencyOptions.find(opt => opt.value === survey.frequency)?.label || ''
}

const submitSurvey = async () => {
  try {
    await submitCustomerSurvey(survey)
    toast.success('Thank you for your feedback!')
    router.push('/survey-complete')
  } catch (error) {
    if (error.response?.data?.errors) {
      surveyErrors.value = error.response.data.errors
    }
    toast.error('Failed to submit survey')
  }
}

const saveDraft = () => {
  localStorage.setItem('surveyDraft', JSON.stringify(survey))
  toast.success('Survey progress saved')
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input-container` | Main container for the field |
| `_c-form-field--auto` | Auto-sized layout |
| `_c-form-field--quarter` | Quarter width layout (25%) |
| `_c-form-field--third` | Third width layout (33%) |
| `_c-form-field--half` | Half width layout (50%) |
| `_c-form-field--full` | Full width layout (100%) |
| `_c-label-container` | Container for label and hint |

## Best Practices

### Radio Button Design
- Use radio buttons for mutually exclusive selections
- Provide clear, descriptive labels for each option
- Order options logically (alphabetical, frequency, importance)
- Limit the number of options to avoid overwhelming users

### User Experience
- Pre-select the most common or recommended option when appropriate
- Use vertical layout for better readability with multiple options
- Provide clear visual feedback for selected state
- Group related options with proper spacing

### Accessibility
- Ensure proper fieldset and legend association
- Use ARIA labels for additional context
- Support keyboard navigation between options
- Test with screen readers and assistive technologies

### Validation
- Validate selection on form submission
- Provide clear error messages for required fields
- Consider progressive validation for dependent fields
- Handle edge cases gracefully

### Performance
- Use efficient option rendering for large lists
- Implement proper change detection
- Avoid unnecessary re-renders
- Cache option data when appropriate

## Component Registration
```javascript
// Global registration
app.component('CodexRadioField', RadioField)

// Local registration  
import RadioField from '@/components/fields/RadioField.vue'

export default {
  components: {
    CodexRadioField: RadioField
  }
}
``` 