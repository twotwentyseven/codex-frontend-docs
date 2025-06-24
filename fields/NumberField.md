# NumberField Component

## Overview
The NumberField component provides a specialized numeric input field with comprehensive form functionality including label management, hint system, helper text, and error handling. It extends the base field functionality with number-specific features and validation, ensuring proper numeric data entry with type safety through Vue's defineModel system.

## Basic Usage
```vue
<template>
  <div class="numeric-form">
    <codex-number-field
      v-model="quantity"
      :name="'quantity'"
      :type="'number'"
      :label="'Quantity'"
      :placeholder="'Enter quantity'"
      :required="true"
    />
  </div>
</template>

<script setup>
const quantity = ref(0)
</script>
```

## Key Features
- Dedicated number input type with numeric keyboard on mobile
- Automatic type coercion to Number type through defineModel
- Flexible layout configurations (auto, quarter, third, half, full)
- Label and hint system with tooltip support
- Helper text and error handling
- Accessibility features with ARIA labels
- Testing support with Dusk attributes
- Consistent form validation integration

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `name` | `String` | `required` | Field name for form submission |
| `type` | `String` | `required` | Input type (should be 'number') |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
| `placeholder` | `String` | `''` | Input placeholder text |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the input |
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

The component uses `defineModel({ type: Number })` to ensure proper type handling:

```vue
<template>
  <codex-number-field v-model="userAge" />
</template>

<script setup>
const userAge = ref(25)
// userAge automatically converts to Number type
console.log(typeof userAge.value) // 'number'
</script>
```

## Examples

### Product Quantity Form
```vue
<template>
  <div class="product-order">
    <h3>Order Details</h3>
    
    <div class="form-grid">
      <codex-number-field
        v-model="orderForm.quantity"
        :name="'quantity'"
        :type="'number'"
        :label="'Quantity'"
        :placeholder="'How many items?'"
        :layout="'4'"
        :required="true"
        :errors="quantityErrors"
        :helper-text="quantityHelperText"
      />
      
      <codex-number-field
        v-model="orderForm.unitPrice"
        :name="'unit_price'"
        :type="'number'"
        :label="'Unit Price'"
        :placeholder="'0.00'"
        :layout="'4'"
        :required="true"
        :helper-text="'Price per item'"
      />
      
      <div class="calculated-total">
        <strong>Total: ${{ calculatedTotal.toFixed(2) }}</strong>
      </div>
    </div>
    
    <button @click="submitOrder" :disabled="!isValidOrder">
      Place Order
    </button>
  </div>
</template>

<script setup>
const orderForm = reactive({
  quantity: 1,
  unitPrice: 0
})

const quantityErrors = ref([])

const quantityHelperText = computed(() => {
  if (orderForm.quantity <= 0) return 'Quantity must be greater than 0'
  if (orderForm.quantity > 100) return 'Maximum quantity is 100'
  return `${orderForm.quantity} item${orderForm.quantity > 1 ? 's' : ''} selected`
})

const calculatedTotal = computed(() => {
  return orderForm.quantity * orderForm.unitPrice
})

const isValidOrder = computed(() => {
  return orderForm.quantity > 0 && orderForm.unitPrice > 0
})

watch(() => orderForm.quantity, (newQuantity) => {
  if (newQuantity <= 0) {
    quantityErrors.value = ['Quantity must be greater than 0']
  } else if (newQuantity > 100) {
    quantityErrors.value = ['Maximum quantity is 100 items']
  } else {
    quantityErrors.value = []
  }
})

const submitOrder = async () => {
  try {
    await createOrder(orderForm)
    console.log('Order placed successfully')
  } catch (error) {
    console.error('Order failed:', error)
  }
}
</script>
```

### User Profile Age Fields
```vue
<template>
  <div class="profile-form">
    <h3>Personal Information</h3>
    
    <codex-number-field
      v-model="profile.age"
      :name="'age'"
      :type="'number'"
      :label="'Age'"
      :placeholder="'Your age'"
      :layout="'4'"
      :required="true"
      :errors="ageErrors"
      :tooltip-text="'Used for age verification and recommendations'"
      :tooltip-icon="'ri-information-line'"
    />
    
    <codex-number-field
      v-model="profile.yearsExperience"
      :name="'years_experience'"
      :type="'number'"
      :label="'Years of Experience'"
      :placeholder="'Professional experience'"
      :layout="'4'"
      :required="false"
      :helper-text="'Optional - helps us match you with relevant opportunities'"
    />
    
    <div class="age-verification" v-if="profile.age">
      <div v-if="profile.age < 18" class="warning">
        <i class="ri-warning-line"></i>
        <span>Parental consent may be required</span>
      </div>
      <div v-else-if="profile.age >= 65" class="info">
        <i class="ri-information-line"></i>
        <span>Senior discount may apply</span>
      </div>
    </div>
  </div>
</template>

<script setup>
const profile = reactive({
  age: null,
  yearsExperience: null
})

const ageErrors = ref([])

watch(() => profile.age, (newAge) => {
  ageErrors.value = []
  
  if (newAge !== null) {
    if (newAge < 13) {
      ageErrors.value = ['Minimum age is 13 years']
    } else if (newAge > 120) {
      ageErrors.value = ['Please enter a valid age']
    }
  }
})
</script>
```

### Financial Calculator
```vue
<template>
  <div class="loan-calculator">
    <h3>Loan Calculator</h3>
    
    <div class="calculator-inputs">
      <codex-number-field
        v-model="loanData.principal"
        :name="'principal'"
        :type="'number'"
        :label="'Loan Amount'"
        :placeholder="'10000'"
        :layout="'2'"
        :required="true"
        :helper-text="'Principal loan amount in dollars'"
      />
      
      <codex-number-field
        v-model="loanData.interestRate"
        :name="'interest_rate'"
        :type="'number'"
        :label="'Interest Rate (%)'"
        :placeholder="'5.5'"
        :layout="'2'"
        :required="true"
        :helper-text="'Annual percentage rate'"
      />
      
      <codex-number-field
        v-model="loanData.termYears"
        :name="'term_years'"
        :type="'number'"
        :label="'Loan Term (Years)'"
        :placeholder="'30'"
        :layout="'2'"
        :required="true"
        :helper-text="'Length of loan in years'"
      />
      
      <codex-number-field
        v-model="loanData.downPayment"
        :name="'down_payment'"
        :type="'number'"
        :label="'Down Payment'"
        :placeholder="'2000'"
        :layout="'2'"
        :required="false"
        :helper-text="'Optional upfront payment'"
      />
    </div>
    
    <div class="calculation-results" v-if="isValidCalculation">
      <div class="result-item">
        <span>Monthly Payment:</span>
        <strong>${{ monthlyPayment.toFixed(2) }}</strong>
      </div>
      <div class="result-item">
        <span>Total Interest:</span>
        <strong>${{ totalInterest.toFixed(2) }}</strong>
      </div>
      <div class="result-item">
        <span>Total Cost:</span>
        <strong>${{ totalCost.toFixed(2) }}</strong>
      </div>
    </div>
  </div>
</template>

<script setup>
const loanData = reactive({
  principal: 100000,
  interestRate: 5.5,
  termYears: 30,
  downPayment: 0
})

const isValidCalculation = computed(() => {
  return loanData.principal > 0 && 
         loanData.interestRate > 0 && 
         loanData.termYears > 0
})

const monthlyPayment = computed(() => {
  if (!isValidCalculation.value) return 0
  
  const principal = loanData.principal - (loanData.downPayment || 0)
  const monthlyRate = loanData.interestRate / 100 / 12
  const numPayments = loanData.termYears * 12
  
  return principal * (monthlyRate * Math.pow(1 + monthlyRate, numPayments)) / 
         (Math.pow(1 + monthlyRate, numPayments) - 1)
})

const totalInterest = computed(() => {
  return (monthlyPayment.value * loanData.termYears * 12) - 
         (loanData.principal - (loanData.downPayment || 0))
})

const totalCost = computed(() => {
  return loanData.principal + totalInterest.value
})
</script>
```

### Inventory Management
```vue
<template>
  <div class="inventory-form">
    <h3>Update Inventory</h3>
    
    <div class="inventory-grid">
      <codex-number-field
        v-model="inventory.currentStock"
        :name="'current_stock'"
        :type="'number'"
        :label="'Current Stock'"
        :placeholder="'0'"
        :layout="'3'"
        :required="true"
        :errors="stockErrors"
        :helper-text="currentStockStatus"
      />
      
      <codex-number-field
        v-model="inventory.minimumStock"
        :name="'minimum_stock'"
        :type="'number'"
        :label="'Minimum Stock Level'"
        :placeholder="'10'"
        :layout="'3'"
        :required="true"
        :helper-text="'Reorder threshold'"
      />
      
      <codex-number-field
        v-model="inventory.reorderQuantity"
        :name="'reorder_quantity'"
        :type="'number'"
        :label="'Reorder Quantity'"
        :placeholder="'50'"
        :layout="'3'"
        :required="true"
        :helper-text="'How many to order when restocking'"
      />
    </div>
    
    <div class="stock-adjustments">
      <h4>Stock Adjustments</h4>
      
      <div class="adjustment-controls">
        <codex-number-field
          v-model="adjustment.quantity"
          :name="'adjustment_quantity'"
          :type="'number'"
          :label="'Adjustment Quantity'"
          :placeholder="'0'"
          :layout="'2'"
          :helper-text="'Use negative numbers to reduce stock'"
        />
        
        <div class="adjustment-buttons">
          <button @click="addStock" :disabled="adjustment.quantity <= 0">
            Add Stock
          </button>
          <button @click="removeStock" :disabled="adjustment.quantity <= 0">
            Remove Stock
          </button>
        </div>
      </div>
    </div>
    
    <div class="stock-alerts" v-if="stockAlerts.length > 0">
      <div v-for="alert in stockAlerts" :key="alert.type" class="alert" :class="alert.type">
        <i :class="alert.icon"></i>
        <span>{{ alert.message }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
const inventory = reactive({
  currentStock: 25,
  minimumStock: 10,
  reorderQuantity: 50
})

const adjustment = reactive({
  quantity: 0
})

const stockErrors = ref([])

const currentStockStatus = computed(() => {
  if (inventory.currentStock <= 0) return 'Out of stock'
  if (inventory.currentStock <= inventory.minimumStock) return 'Low stock warning'
  return `${inventory.currentStock} units in stock`
})

const stockAlerts = computed(() => {
  const alerts = []
  
  if (inventory.currentStock <= 0) {
    alerts.push({
      type: 'error',
      icon: 'ri-error-warning-line',
      message: 'Item is out of stock'
    })
  } else if (inventory.currentStock <= inventory.minimumStock) {
    alerts.push({
      type: 'warning',
      icon: 'ri-alert-line',
      message: `Stock is below minimum threshold (${inventory.minimumStock})`
    })
  }
  
  return alerts
})

const addStock = () => {
  inventory.currentStock += adjustment.quantity
  adjustment.quantity = 0
  
  // Log stock adjustment
  console.log(`Added ${adjustment.quantity} units to stock`)
}

const removeStock = () => {
  const newStock = inventory.currentStock - adjustment.quantity
  
  if (newStock < 0) {
    stockErrors.value = ['Cannot reduce stock below zero']
    return
  }
  
  inventory.currentStock = newStock
  adjustment.quantity = 0
  stockErrors.value = []
  
  // Log stock adjustment
  console.log(`Removed ${adjustment.quantity} units from stock`)
}

watch(() => inventory.currentStock, (newStock) => {
  stockErrors.value = []
  
  if (newStock < 0) {
    stockErrors.value = ['Stock cannot be negative']
  }
})
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

### Number Input Validation
- Always validate numeric ranges appropriate for your use case
- Provide clear feedback for invalid numbers
- Consider minimum and maximum values in your validation
- Handle edge cases like negative numbers and decimals appropriately

### User Experience
- Use appropriate placeholder values that demonstrate expected format
- Provide helper text to clarify numeric formats or constraints
- Show calculated results immediately when dependent on user input
- Consider using step attributes for specific increment requirements

### Accessibility
- Provide clear labels that indicate expected numeric format
- Use ARIA labels to describe numeric constraints
- Ensure keyboard navigation works properly with number inputs
- Test with assistive technologies for proper number announcement

### Mobile Optimization
- Number input type automatically shows numeric keyboard on mobile
- Consider the precision needed (integers vs decimals)
- Test on various mobile devices for input behavior
- Provide adequate touch targets for increment/decrement buttons

### Validation Patterns
- Validate on input change for immediate feedback
- Handle both client-side and server-side validation
- Provide specific error messages for different validation failures
- Consider debouncing validation for performance on rapid input

### Type Safety
- Leverage Vue's `defineModel({ type: Number })` for automatic type conversion
- Handle null/undefined states appropriately
- Validate numeric types in computed properties and watchers
- Use proper TypeScript types if using TypeScript

### Performance
- Avoid unnecessary reactivity for calculated values
- Use computed properties for derived numeric calculations
- Implement proper debouncing for expensive calculations
- Cache calculation results when appropriate

## Component Registration
```javascript
// Global registration
app.component('CodexNumberField', NumberField)

// Local registration  
import NumberField from '@/components/fields/NumberField.vue'

export default {
  components: {
    CodexNumberField: NumberField
  }
}
``` 