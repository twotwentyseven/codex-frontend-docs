# RadioGroup Component

## Overview
The RadioGroup component provides a collection of radio buttons for single selection from multiple options. It renders individual radio buttons with associated labels and manages the selection state across the group. The component is designed as a sub-component that works with option arrays and provides flexible layout configurations for different form arrangements.

## Basic Usage
```vue
<template>
  <div class="radio-group-form">
    <codex-radio-group
      v-model="selectedOption"
      :options="radioOptions"
      :layout="'2'"
      :dusk="'satisfaction-rating'"
      :id="'rating-group'"
    />
  </div>
</template>

<script setup>
const selectedOption = ref('')

const radioOptions = [
  { value: 'excellent', label: 'Excellent' },
  { value: 'good', label: 'Good' },
  { value: 'fair', label: 'Fair' },
  { value: 'poor', label: 'Poor' }
]
</script>
```

## Key Features
- Single selection radio button group
- Dynamic option rendering from array configuration
- Individual radio button with label association
- Flexible layout configurations for responsive design
- Automatic focus management and keyboard navigation
- Option disabling support for specific choices
- Form integration with proper value binding
- Testing support with Dusk attributes
- Injection-based configuration for parent communication

## Configuration Props

### Value Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `modelValue` | `String` | `undefined` | Currently selected radio value |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'full'` | Layout size (auto, 4, 3, 2, 1, full) |

### Testing Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dusk` | `String` | `''` | Browser testing identifier |

## Injected Props

The component receives configuration from parent via Vue's provide/inject:

| Injected Property | Type | Usage |
|------------------|------|-------|
| `placeholder` | `String` | Placeholder text for radio inputs |
| `id` | `String` | Base ID for radio button naming |
| `options` | `Array` | Array of radio button options |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `update:modelValue` | `String` | Emitted when radio selection changes |

## Option Configuration

Options should be provided as an array of objects:

```javascript
const options = [
  { 
    value: 'option1',           // Value stored in model
    label: 'Option 1',          // Label shown next to radio
    disabled: false             // Optional: disable specific option
  },
  { 
    value: 'option2', 
    label: 'Option 2' 
  }
]
```

## Model Value

The component manages a string value representing the selected radio option:

```vue
<template>
  <codex-radio-group v-model="selection" :options="options" />
</template>

<script setup>
const selection = ref('')
// selection contains the value of the selected radio option
// Only one option can be selected at a time
</script>
```

## Examples

### Customer Satisfaction Survey
```vue
<template>
  <div class="satisfaction-survey">
    <h3>Customer Satisfaction Survey</h3>
    
    <div class="survey-sections">
      <div class="survey-question">
        <h4>Overall Service Rating</h4>
        <p>How would you rate our overall service quality?</p>
        
        <codex-radio-group
          v-model="survey.overallRating"
          :options="ratingOptions"
          :layout="'2'"
          :dusk="'overall-rating'"
          :id="'overall-rating'"
        />
      </div>
      
      <div class="survey-question">
        <h4>Product Quality</h4>
        <p>How satisfied are you with the quality of our products?</p>
        
        <codex-radio-group
          v-model="survey.productQuality"
          :options="qualityOptions"
          :layout="'3'"
          :dusk="'product-quality'"
          :id="'product-quality'"
        />
      </div>
      
      <div class="survey-question">
        <h4>Recommendation Likelihood</h4>
        <p>How likely are you to recommend us to others?</p>
        
        <codex-radio-group
          v-model="survey.recommendationLikelihood"
          :options="likelihoodOptions"
          :layout="'1'"
          :dusk="'recommendation'"
          :id="'recommendation'"
        />
      </div>
      
      <div class="survey-question">
        <h4>Support Experience</h4>
        <p>If you contacted support, how was your experience?</p>
        
        <codex-radio-group
          v-model="survey.supportExperience"
          :options="supportOptions"
          :layout="'2'"
          :dusk="'support-experience'"
          :id="'support-experience'"
        />
      </div>
    </div>
    
    <div class="survey-summary" v-if="hasSurveyResponses">
      <h4>Your Responses</h4>
      
      <div class="response-grid">
        <div class="response-item" v-if="survey.overallRating">
          <h5>Overall Service</h5>
          <div class="response-value" :class="getResponseClass('overallRating')">
            {{ getResponseLabel(survey.overallRating, ratingOptions) }}
          </div>
          <div class="response-score">{{ getNumericScore(survey.overallRating, 'rating') }}/5</div>
        </div>
        
        <div class="response-item" v-if="survey.productQuality">
          <h5>Product Quality</h5>
          <div class="response-value" :class="getResponseClass('productQuality')">
            {{ getResponseLabel(survey.productQuality, qualityOptions) }}
          </div>
          <div class="response-score">{{ getNumericScore(survey.productQuality, 'quality') }}/5</div>
        </div>
        
        <div class="response-item" v-if="survey.recommendationLikelihood">
          <h5>Recommendation</h5>
          <div class="response-value" :class="getResponseClass('recommendationLikelihood')">
            {{ getResponseLabel(survey.recommendationLikelihood, likelihoodOptions) }}
          </div>
          <div class="response-score">NPS: {{ getNPSScore(survey.recommendationLikelihood) }}</div>
        </div>
        
        <div class="response-item" v-if="survey.supportExperience">
          <h5>Support</h5>
          <div class="response-value" :class="getResponseClass('supportExperience')">
            {{ getResponseLabel(survey.supportExperience, supportOptions) }}
          </div>
        </div>
      </div>
      
      <div class="overall-score">
        <h5>Overall Satisfaction Score</h5>
        <div class="score-display">
          <div class="score-number">{{ calculateOverallScore() }}</div>
          <div class="score-label">{{ getScoreLabel(calculateOverallScore()) }}</div>
        </div>
        <div class="score-breakdown">
          Based on {{ completedResponses }} completed responses
        </div>
      </div>
    </div>
    
    <div class="survey-actions">
      <button @click="clearSurvey" class="clear-btn">
        Clear Responses
      </button>
      <button @click="submitSurvey" :disabled="!isValidSurvey" class="submit-btn">
        Submit Survey
      </button>
    </div>
  </div>
</template>

<script setup>
const survey = reactive({
  overallRating: '',
  productQuality: '',
  recommendationLikelihood: '',
  supportExperience: ''
})

const ratingOptions = [
  { value: 'excellent', label: 'Excellent' },
  { value: 'very_good', label: 'Very Good' },
  { value: 'good', label: 'Good' },
  { value: 'fair', label: 'Fair' },
  { value: 'poor', label: 'Poor' }
]

const qualityOptions = [
  { value: 'outstanding', label: 'Outstanding Quality' },
  { value: 'high', label: 'High Quality' },
  { value: 'acceptable', label: 'Acceptable Quality' },
  { value: 'below_average', label: 'Below Average' },
  { value: 'poor', label: 'Poor Quality' }
]

const likelihoodOptions = [
  { value: 'definitely', label: 'Definitely Will Recommend' },
  { value: 'probably', label: 'Probably Will Recommend' },
  { value: 'might', label: 'Might Recommend' },
  { value: 'probably_not', label: 'Probably Won\'t Recommend' },
  { value: 'definitely_not', label: 'Definitely Won\'t Recommend' }
]

const supportOptions = [
  { value: 'exceptional', label: 'Exceptional Support' },
  { value: 'helpful', label: 'Helpful and Responsive' },
  { value: 'adequate', label: 'Adequate Support' },
  { value: 'slow', label: 'Slow Response' },
  { value: 'unhelpful', label: 'Unhelpful' },
  { value: 'no_contact', label: 'Haven\'t Contacted Support' }
]

const hasSurveyResponses = computed(() => {
  return Object.values(survey).some(response => response !== '')
})

const completedResponses = computed(() => {
  return Object.values(survey).filter(response => response !== '').length
})

const isValidSurvey = computed(() => {
  // Require at least overall rating and one other response
  return survey.overallRating !== '' && completedResponses.value >= 2
})

const getResponseLabel = (value, options) => {
  const option = options.find(opt => opt.value === value)
  return option ? option.label : value
}

const getResponseClass = (responseKey) => {
  const value = survey[responseKey]
  const positiveValues = ['excellent', 'very_good', 'outstanding', 'high', 'definitely', 'probably', 'exceptional', 'helpful']
  const negativeValues = ['poor', 'below_average', 'definitely_not', 'probably_not', 'slow', 'unhelpful']
  
  if (positiveValues.includes(value)) return 'positive'
  if (negativeValues.includes(value)) return 'negative'
  return 'neutral'
}

const getNumericScore = (value, type) => {
  const scoreMap = {
    rating: {
      excellent: 5, very_good: 4, good: 3, fair: 2, poor: 1
    },
    quality: {
      outstanding: 5, high: 4, acceptable: 3, below_average: 2, poor: 1
    }
  }
  
  return scoreMap[type] ? scoreMap[type][value] || 0 : 0
}

const getNPSScore = (value) => {
  const npsMap = {
    definitely: 10, probably: 8, might: 6, probably_not: 3, definitely_not: 1
  }
  return npsMap[value] || 0
}

const calculateOverallScore = () => {
  const scores = [
    getNumericScore(survey.overallRating, 'rating'),
    getNumericScore(survey.productQuality, 'quality')
  ].filter(score => score > 0)
  
  if (scores.length === 0) return 0
  return Math.round(scores.reduce((sum, score) => sum + score, 0) / scores.length * 20) // Convert to 100-point scale
}

const getScoreLabel = (score) => {
  if (score >= 80) return 'Highly Satisfied'
  if (score >= 60) return 'Satisfied'
  if (score >= 40) return 'Neutral'
  if (score >= 20) return 'Dissatisfied'
  return 'Highly Dissatisfied'
}

const clearSurvey = () => {
  Object.keys(survey).forEach(key => {
    survey[key] = ''
  })
  toast.info('Survey responses cleared')
}

const submitSurvey = async () => {
  try {
    await submitCustomerSurvey({
      responses: survey,
      overallScore: calculateOverallScore(),
      completedAt: new Date().toISOString()
    })
    
    toast.success('Thank you for your feedback!')
    router.push('/survey/thank-you')
  } catch (error) {
    toast.error('Failed to submit survey')
  }
}
</script>
```

### Product Configuration Form
```vue
<template>
  <div class="product-configuration">
    <h3>Product Configuration</h3>
    
    <div class="configuration-sections">
      <div class="config-section">
        <h4>Size Selection</h4>
        <p>Choose your preferred size</p>
        
        <codex-radio-group
          v-model="config.size"
          :options="sizeOptions"
          :layout="'4'"
          :dusk="'product-size'"
          :id="'product-size'"
        />
      </div>
      
      <div class="config-section">
        <h4>Color Options</h4>
        <p>Select your favorite color</p>
        
        <codex-radio-group
          v-model="config.color"
          :options="colorOptions"
          :layout="'3'"
          :dusk="'product-color'"
          :id="'product-color'"
        />
      </div>
      
      <div class="config-section">
        <h4>Material Choice</h4>
        <p>Choose the material type</p>
        
        <codex-radio-group
          v-model="config.material"
          :options="materialOptions"
          :layout="'2'"
          :dusk="'product-material'"
          :id="'product-material'"
        />
      </div>
      
      <div class="config-section">
        <h4>Shipping Method</h4>
        <p>How would you like to receive your order?</p>
        
        <codex-radio-group
          v-model="config.shipping"
          :options="shippingOptions"
          :layout="'1'"
          :dusk="'shipping-method'"
          :id="'shipping-method'"
        />
      </div>
    </div>
    
    <div class="configuration-preview" v-if="hasConfiguration">
      <h4>Configuration Summary</h4>
      
      <div class="preview-grid">
        <div class="preview-section">
          <h5>Product Details</h5>
          <div class="detail-list">
            <div class="detail-item" v-if="config.size">
              <span class="detail-label">Size:</span>
              <span class="detail-value">{{ getSizeLabel(config.size) }}</span>
            </div>
            <div class="detail-item" v-if="config.color">
              <span class="detail-label">Color:</span>
              <span class="detail-value">
                <span class="color-swatch" :style="{ backgroundColor: getColorCode(config.color) }"></span>
                {{ getColorLabel(config.color) }}
              </span>
            </div>
            <div class="detail-item" v-if="config.material">
              <span class="detail-label">Material:</span>
              <span class="detail-value">{{ getMaterialLabel(config.material) }}</span>
            </div>
          </div>
        </div>
        
        <div class="preview-section">
          <h5>Shipping & Pricing</h5>
          <div class="pricing-breakdown">
            <div class="price-item">
              <span class="price-label">Base Price:</span>
              <span class="price-value">${{ basePrice.toFixed(2) }}</span>
            </div>
            <div v-if="sizePriceAdjustment !== 0" class="price-item">
              <span class="price-label">Size Adjustment:</span>
              <span class="price-value">{{ sizePriceAdjustment > 0 ? '+' : '' }}${{ sizePriceAdjustment.toFixed(2) }}</span>
            </div>
            <div v-if="materialPriceAdjustment !== 0" class="price-item">
              <span class="price-label">Material Upgrade:</span>
              <span class="price-value">+${{ materialPriceAdjustment.toFixed(2) }}</span>
            </div>
            <div class="price-item shipping">
              <span class="price-label">Shipping:</span>
              <span class="price-value">${{ shippingCost.toFixed(2) }}</span>
            </div>
            <div class="price-item total">
              <span class="price-label">Total:</span>
              <span class="price-value">${{ totalPrice.toFixed(2) }}</span>
            </div>
          </div>
          
          <div class="shipping-info" v-if="config.shipping">
            <div class="shipping-method">
              <i class="ri-truck-line"></i>
              <span>{{ getShippingLabel(config.shipping) }}</span>
            </div>
            <div class="delivery-estimate">
              Estimated delivery: {{ getDeliveryEstimate(config.shipping) }}
            </div>
          </div>
        </div>
      </div>
      
      <div class="configuration-validation">
        <div v-if="configurationErrors.length > 0" class="validation-errors">
          <h6>Configuration Issues:</h6>
          <div v-for="error in configurationErrors" :key="error" class="error-item">
            <i class="ri-error-warning-line"></i>
            {{ error }}
          </div>
        </div>
        
        <div v-if="configurationWarnings.length > 0" class="validation-warnings">
          <h6>Please Note:</h6>
          <div v-for="warning in configurationWarnings" :key="warning" class="warning-item">
            <i class="ri-information-line"></i>
            {{ warning }}
          </div>
        </div>
      </div>
    </div>
    
    <div class="configuration-actions">
      <button @click="resetConfiguration" class="reset-btn">
        Reset Configuration
      </button>
      <button @click="saveConfiguration" :disabled="!isValidConfiguration" class="save-btn">
        Save Configuration
      </button>
      <button @click="addToCart" :disabled="!isCompleteConfiguration" class="add-cart-btn">
        Add to Cart - ${{ totalPrice.toFixed(2) }}
      </button>
    </div>
  </div>
</template>

<script setup>
const config = reactive({
  size: '',
  color: '',
  material: '',
  shipping: ''
})

// Option arrays would be defined here...
const sizeOptions = [
  { value: 'xs', label: 'Extra Small' },
  { value: 's', label: 'Small' },
  { value: 'm', label: 'Medium' },
  { value: 'l', label: 'Large' },
  { value: 'xl', label: 'Extra Large' }
]

const colorOptions = [
  { value: 'red', label: 'Classic Red' },
  { value: 'blue', label: 'Ocean Blue' },
  { value: 'green', label: 'Forest Green' },
  { value: 'black', label: 'Midnight Black' },
  { value: 'white', label: 'Pure White' }
]

const materialOptions = [
  { value: 'cotton', label: 'Premium Cotton' },
  { value: 'polyester', label: 'Polyester Blend' },
  { value: 'silk', label: 'Pure Silk' },
  { value: 'linen', label: 'Natural Linen' }
]

const shippingOptions = [
  { value: 'standard', label: 'Standard Shipping (5-7 days) - Free' },
  { value: 'express', label: 'Express Shipping (3-5 days) - $9.99' },
  { value: 'overnight', label: 'Overnight Delivery - $24.99' },
  { value: 'pickup', label: 'Store Pickup - Free' }
]

const basePrice = 29.99

const hasConfiguration = computed(() => {
  return Object.values(config).some(value => value !== '')
})

const isValidConfiguration = computed(() => {
  return config.size !== '' && config.color !== ''
})

const isCompleteConfiguration = computed(() => {
  return config.size !== '' && config.color !== '' && config.material !== '' && config.shipping !== ''
})

// Pricing calculations...
const sizePriceAdjustment = computed(() => {
  const adjustments = { xs: -2, s: 0, m: 0, l: 2, xl: 4 }
  return adjustments[config.size] || 0
})

const materialPriceAdjustment = computed(() => {
  const adjustments = { cotton: 5, polyester: 0, silk: 15, linen: 8 }
  return adjustments[config.material] || 0
})

const shippingCost = computed(() => {
  const costs = { standard: 0, express: 9.99, overnight: 24.99, pickup: 0 }
  return costs[config.shipping] || 0
})

const totalPrice = computed(() => {
  return basePrice + sizePriceAdjustment.value + materialPriceAdjustment.value + shippingCost.value
})

// Helper functions...
const getSizeLabel = (size) => {
  const option = sizeOptions.find(opt => opt.value === size)
  return option ? option.label : size
}

const getColorLabel = (color) => {
  const option = colorOptions.find(opt => opt.value === color)
  return option ? option.label : color
}

const getColorCode = (color) => {
  const colorCodes = {
    red: '#DC2626', blue: '#2563EB', green: '#16A34A',
    black: '#000000', white: '#FFFFFF'
  }
  return colorCodes[color] || '#6B7280'
}

// More helper functions and validation logic...

const configurationErrors = computed(() => {
  const errors = []
  
  if (config.material === 'silk' && config.shipping === 'overnight') {
    errors.push('Silk products require special handling and cannot be shipped overnight')
  }
  
  return errors
})

const configurationWarnings = computed(() => {
  const warnings = []
  
  if (config.size === 'xs' || config.size === 'xl') {
    warnings.push('This size may have extended processing time (additional 2-3 days)')
  }
  
  return warnings
})

const resetConfiguration = () => {
  Object.keys(config).forEach(key => {
    config[key] = ''
  })
  toast.info('Configuration reset')
}

const saveConfiguration = () => {
  localStorage.setItem('productConfiguration', JSON.stringify(config))
  toast.success('Configuration saved!')
}

const addToCart = async () => {
  try {
    await addProductToCart({
      configuration: config,
      basePrice,
      totalPrice: totalPrice.value,
      priceBreakdown: {
        base: basePrice,
        sizeAdjustment: sizePriceAdjustment.value,
        materialUpgrade: materialPriceAdjustment.value,
        shipping: shippingCost.value
      }
    })
    
    toast.success('Product added to cart!')
    router.push('/cart')
  } catch (error) {
    toast.error('Failed to add product to cart')
  }
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-checkbox-container` | Container for each radio button and label |
| `_c-form-field--auto` | Auto-sized layout |
| `_c-form-field--quarter` | Quarter width layout (25%) |
| `_c-form-field--third` | Third width layout (33%) |
| `_c-form-field--half` | Half width layout (50%) |
| `_c-form-field--full` | Full width layout (100%) |
| `_c-label-container` | Container for label element |

## Best Practices

### Option Management
- Keep radio button groups to a reasonable size (2-8 options typically)
- Use clear, descriptive labels for each option
- Order options logically (alphabetical, by frequency, or by preference)
- Consider using other input types for very large option sets
- Disable options that are contextually unavailable

### User Experience
- Always provide clear question or instruction text
- Group related radio buttons visually
- Show selected state clearly with appropriate styling
- Consider default selections for common use cases
- Provide context for technical or complex options

### Accessibility
- Ensure each radio button has a unique ID
- Associate labels properly with their radio buttons
- Use fieldset and legend for radio button groups
- Support keyboard navigation (arrow keys, tab, space)
- Test with screen readers and assistive technologies

### Validation
- Validate required radio button selections
- Provide clear error messages for missing selections
- Consider conditional validation based on selections
- Handle validation state changes appropriately
- Show validation feedback near the radio group

### Layout and Responsive Design
- Use appropriate layouts for different screen sizes
- Ensure radio buttons are touch-friendly on mobile
- Consider stacking vs. inline layouts based on content
- Test radio button spacing and alignment
- Maintain consistent styling across different layouts

### Performance
- Use efficient rendering for dynamic option updates
- Avoid unnecessary re-renders when possible
- Implement proper key management for option lists
- Consider lazy loading for complex option data
- Optimize event handling for large radio groups

## Component Registration
```javascript
// Global registration
app.component('CodexRadioGroup', RadioGroup)

// Local registration  
import RadioGroup from '@/components/fields/RadioGroup.vue'

export default {
  components: {
    CodexRadioGroup: RadioGroup
  }
}
``` 