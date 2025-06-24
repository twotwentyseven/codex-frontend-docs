# VerifyInput Component

## Overview
The VerifyInput component provides a specialized single-character input field designed for verification codes, PINs, and multi-step authentication workflows. It features automatic input sanitization, numeric-only validation, single character limitation, Vue dependency injection for configuration, and pattern-based validation with defineModel binding for seamless integration with verification forms.

## Basic Usage
```vue
<!-- Must be used within a provider that injects required values -->
<template>
  <div>
    <!-- Provider component that injects configuration -->
    <verification-provider
      :placeholder="'0'"
      :aria-label="'Enter verification digit'"
      :name="'verification_digit_1'"
      :id="'verify-1'"
    >
      <codex-verify-input v-model="verificationCode[0]" />
    </verification-provider>
  </div>
</template>
```

## Key Features
- Single character input limitation with automatic maxlength="1"
- Numeric-only input validation and sanitization
- Vue dependency injection for configuration management
- defineModel binding for reactive two-way data binding
- Real-time input sanitization and pattern validation
- Form integration with standard HTML attributes
- Accessibility support with ARIA labels and proper semantics
- Testing support with dusk attributes for automation

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| dusk | String | No | '' | Testing attribute for browser automation |

## Model Value
The component uses `defineModel` for two-way data binding:
- **Type**: String
- **Validation**: Automatically sanitized to numeric characters only
- **Length**: Limited to single character input
- **Pattern**: Validates against numeric pattern `/^([0-9])?$/`

## Injected Dependencies

The component relies on Vue's dependency injection system for configuration:

### Required Injections
| Injection Key | Type | Description |
|---------------|------|-------------|
| placeholder | String | Placeholder text for the input field |
| ariaLabel | String | ARIA label for accessibility |
| name | String | Name attribute for form submission |
| id | String | Unique identifier for the input |
| required | Boolean | Whether the field is required |

## Input Sanitization
The component implements comprehensive input sanitization:
- **Numeric Only**: Removes all non-numeric characters using `/[^0-9]/g` regex
- **Length Limit**: Automatically truncates input to single character
- **Pattern Validation**: Validates against numeric pattern before accepting input
- **Real-time Processing**: Sanitization occurs on every input event

## Validation Logic
Input validation follows a multi-step process:
1. **Raw Input**: Captures the raw input value from the event
2. **Character Filtering**: Removes non-numeric characters
3. **Length Truncation**: Limits to single character (configurable via count)
4. **Pattern Matching**: Validates against regex pattern
5. **Model Update**: Updates the model value with sanitized input

## Form Integration
The component integrates seamlessly with forms:
- **Standard Attributes**: Supports id, name, required attributes through injection
- **Input Type**: Uses type="text" with custom validation instead of type="number"
- **Accessibility**: Provides proper ARIA labeling and semantic structure
- **Form Submission**: Works with standard form submission and validation

## Component Exposure
The component exposes its input element reference:
```javascript
defineExpose({
  input
})
```
This allows parent components to access the input element for focus management and programmatic control.

## Use Cases
Suitable for:
- **Verification Codes**: SMS verification, email verification codes
- **PIN Entry**: Personal identification number input
- **OTP Fields**: One-time password character inputs
- **Security Codes**: Two-factor authentication inputs
- **Numeric Sequences**: Sequential number entry workflows

## Internationalization
The component receives internationalized text through dependency injection, allowing for localized placeholder and ARIA label text.

## Examples

### SMS Verification Code
```vue
<template>
  <form @submit="handleVerification">
    <h3>Enter Verification Code</h3>
    <p>Please enter the 4-digit code sent to your phone</p>
    
    <div class="verification-inputs">
      <verification-provider
        v-for="(digit, index) in verificationCode"
        :key="index"
        :placeholder="'0'"
        :aria-label="`Verification digit ${index + 1}`"
        :name="`verification_digit_${index + 1}`"
        :id="`verify-${index + 1}`"
        :required="true"
      >
        <codex-verify-input
          v-model="verificationCode[index]"
          :dusk="`verify-input-${index + 1}`"
          @input="handleDigitInput(index)"
          ref="verifyInputs"
        />
      </verification-provider>
    </div>
    
    <button type="submit" :disabled="!isCodeComplete">
      Verify Code
    </button>
  </form>
</template>

<script setup>
const verificationCode = ref(['', '', '', ''])
const verifyInputs = ref([])

const isCodeComplete = computed(() => 
  verificationCode.value.every(digit => digit !== '')
)

const handleDigitInput = (index) => {
  // Auto-focus next input when digit is entered
  if (verificationCode.value[index] && index < 3) {
    const nextInput = verifyInputs.value[index + 1]?.input
    nextInput?.focus()
  }
}

const handleVerification = () => {
  const code = verificationCode.value.join('')
  console.log('Verifying code:', code)
  // Submit verification logic
}
</script>

<style scoped>
.verification-inputs {
  display: flex;
  gap: 8px;
  justify-content: center;
  margin: 16px 0;
}
</style>
```

### PIN Entry System
```vue
<template>
  <div class="pin-entry">
    <h3>Enter Your PIN</h3>
    
    <div class="pin-inputs">
      <verification-provider
        v-for="(digit, index) in pinCode"
        :key="index"
        :placeholder="'•'"
        :aria-label="`PIN digit ${index + 1}`"
        :name="`pin_digit_${index + 1}`"
        :id="`pin-${index + 1}`"
        :required="true"
      >
        <codex-verify-input
          v-model="pinCode[index]"
          :dusk="`pin-input-${index + 1}`"
          @input="handlePinInput(index)"
          @keydown="handleKeydown($event, index)"
          ref="pinInputs"
          class="pin-input"
        />
      </verification-provider>
    </div>
    
    <div class="pin-actions">
      <button @click="clearPin">Clear</button>
      <button @click="submitPin" :disabled="!isPinComplete">
        Submit PIN
      </button>
    </div>
  </div>
</template>

<script setup>
const pinCode = ref(['', '', '', '', '', ''])
const pinInputs = ref([])

const isPinComplete = computed(() => 
  pinCode.value.every(digit => digit !== '')
)

const handlePinInput = (index) => {
  if (pinCode.value[index] && index < 5) {
    const nextInput = pinInputs.value[index + 1]?.input
    nextInput?.focus()
  }
}

const handleKeydown = (event, index) => {
  // Handle backspace to go to previous input
  if (event.key === 'Backspace' && !pinCode.value[index] && index > 0) {
    const prevInput = pinInputs.value[index - 1]?.input
    prevInput?.focus()
  }
}

const clearPin = () => {
  pinCode.value = ['', '', '', '', '', '']
  pinInputs.value[0]?.input?.focus()
}

const submitPin = () => {
  const pin = pinCode.value.join('')
  console.log('Submitting PIN:', pin)
  // Submit PIN logic
}
</script>

<style scoped>
.pin-inputs {
  display: flex;
  gap: 12px;
  justify-content: center;
  margin: 20px 0;
}

.pin-input {
  width: 48px;
  height: 48px;
  text-align: center;
  font-size: 24px;
  border: 2px solid #ccc;
  border-radius: 8px;
}
</style>
```

### Two-Factor Authentication
```vue
<template>
  <div class="two-factor-auth">
    <h3>Two-Factor Authentication</h3>
    <p>Enter the 6-digit code from your authenticator app</p>
    
    <div class="auth-code-inputs">
      <verification-provider
        v-for="(digit, index) in authCode"
        :key="index"
        :placeholder="'0'"
        :aria-label="`Authentication digit ${index + 1} of 6`"
        :name="`auth_digit_${index + 1}`"
        :id="`auth-${index + 1}`"
        :required="true"
      >
        <codex-verify-input
          v-model="authCode[index]"
          :dusk="`auth-input-${index + 1}`"
          @input="handleAuthInput(index)"
          @paste="handlePaste($event, index)"
          ref="authInputs"
        />
      </verification-provider>
    </div>
    
    <div v-if="error" class="error-message">
      {{ error }}
    </div>
    
    <div class="auth-actions">
      <button @click="verifyCode" :disabled="!isCodeValid" :class="{ loading: isVerifying }">
        {{ isVerifying ? 'Verifying...' : 'Verify Code' }}
      </button>
    </div>
    
    <div class="backup-options">
      <button @click="requestNewCode" class="link-button">
        Didn't receive a code? Request new one
      </button>
    </div>
  </div>
</template>

<script setup>
const authCode = ref(['', '', '', '', '', ''])
const authInputs = ref([])
const isVerifying = ref(false)
const error = ref('')

const isCodeValid = computed(() => 
  authCode.value.every(digit => digit !== '') && authCode.value.length === 6
)

const handleAuthInput = (index) => {
  error.value = '' // Clear error on input
  
  if (authCode.value[index] && index < 5) {
    const nextInput = authInputs.value[index + 1]?.input
    nextInput?.focus()
  }
  
  // Auto-verify when all digits are entered
  if (isCodeValid.value) {
    setTimeout(() => verifyCode(), 500)
  }
}

const handlePaste = (event, startIndex) => {
  event.preventDefault()
  const pastedData = event.clipboardData.getData('text')
  const digits = pastedData.replace(/\D/g, '').slice(0, 6).split('')
  
  digits.forEach((digit, index) => {
    if (startIndex + index < 6) {
      authCode.value[startIndex + index] = digit
    }
  })
  
  // Focus the last filled input or first empty one
  const lastIndex = Math.min(startIndex + digits.length, 5)
  authInputs.value[lastIndex]?.input?.focus()
}

const verifyCode = async () => {
  if (!isCodeValid.value) return
  
  isVerifying.value = true
  error.value = ''
  
  try {
    const code = authCode.value.join('')
    // Verify with backend
    await verifyTwoFactorCode(code)
    // Redirect on success
  } catch (err) {
    error.value = 'Invalid verification code. Please try again.'
    authCode.value = ['', '', '', '', '', '']
    authInputs.value[0]?.input?.focus()
  } finally {
    isVerifying.value = false
  }
}

const requestNewCode = async () => {
  try {
    await requestNewVerificationCode()
    // Show success message
  } catch (err) {
    error.value = 'Failed to request new code. Please try again.'
  }
}
</script>
```

### Dynamic Length Verification
```vue
<template>
  <div class="dynamic-verification">
    <h3>Enter Verification Code</h3>
    
    <div class="code-length-selector">
      <label>
        <input type="radio" v-model="codeLength" :value="4" />
        4 digits
      </label>
      <label>
        <input type="radio" v-model="codeLength" :value="6" />
        6 digits
      </label>
      <label>
        <input type="radio" v-model="codeLength" :value="8" />
        8 digits
      </label>
    </div>
    
    <div class="verification-inputs">
      <verification-provider
        v-for="index in codeLength"
        :key="index"
        :placeholder="'0'"
        :aria-label="`Verification digit ${index} of ${codeLength}`"
        :name="`verification_digit_${index}`"
        :id="`verify-${index}`"
        :required="true"
      >
        <codex-verify-input
          v-model="verificationCode[index - 1]"
          :dusk="`verify-input-${index}`"
          @input="handleDynamicInput(index - 1)"
          ref="dynamicInputs"
        />
      </verification-provider>
    </div>
    
    <div class="verification-status">
      <p>Progress: {{ filledDigits }}/{{ codeLength }}</p>
      <div class="progress-bar">
        <div 
          class="progress-fill" 
          :style="{ width: `${(filledDigits / codeLength) * 100}%` }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script setup>
const codeLength = ref(6)
const verificationCode = ref([])
const dynamicInputs = ref([])

const filledDigits = computed(() => 
  verificationCode.value.filter(digit => digit !== '').length
)

// Watch code length changes to reset the array
watch(codeLength, (newLength) => {
  verificationCode.value = Array(newLength).fill('')
})

const handleDynamicInput = (index) => {
  if (verificationCode.value[index] && index < codeLength.value - 1) {
    const nextInput = dynamicInputs.value[index + 1]?.input
    nextInput?.focus()
  }
}

// Initialize with default length
onMounted(() => {
  verificationCode.value = Array(codeLength.value).fill('')
})
</script>
```

### Verification with Timer
```vue
<template>
  <div class="timed-verification">
    <h3>Enter Verification Code</h3>
    <p>Code will expire in {{ formatTime(timeRemaining) }}</p>
    
    <div class="verification-inputs">
      <verification-provider
        v-for="(digit, index) in verificationCode"
        :key="index"
        :placeholder="'0'"
        :aria-label="`Verification digit ${index + 1}`"
        :name="`verification_digit_${index + 1}`"
        :id="`verify-${index + 1}`"
        :required="true"
      >
        <codex-verify-input
          v-model="verificationCode[index]"
          :disabled="timeRemaining <= 0"
          @input="handleTimedInput(index)"
          ref="timedInputs"
        />
      </verification-provider>
    </div>
    
    <div v-if="timeRemaining <= 0" class="expired-message">
      Verification code has expired. Please request a new one.
    </div>
    
    <div class="timer-actions">
      <button 
        v-if="timeRemaining <= 0" 
        @click="requestNewCode"
        :disabled="isRequesting"
      >
        {{ isRequesting ? 'Requesting...' : 'Request New Code' }}
      </button>
    </div>
  </div>
</template>

<script setup>
const verificationCode = ref(['', '', '', '', '', ''])
const timedInputs = ref([])
const timeRemaining = ref(300) // 5 minutes in seconds
const isRequesting = ref(false)
let timer = null

const formatTime = (seconds) => {
  const minutes = Math.floor(seconds / 60)
  const remainingSeconds = seconds % 60
  return `${minutes}:${remainingSeconds.toString().padStart(2, '0')}`
}

const startTimer = () => {
  timer = setInterval(() => {
    if (timeRemaining.value > 0) {
      timeRemaining.value--
    } else {
      clearInterval(timer)
    }
  }, 1000)
}

const handleTimedInput = (index) => {
  if (timeRemaining.value <= 0) return
  
  if (verificationCode.value[index] && index < 5) {
    const nextInput = timedInputs.value[index + 1]?.input
    nextInput?.focus()
  }
}

const requestNewCode = async () => {
  isRequesting.value = true
  try {
    await sendNewVerificationCode()
    verificationCode.value = ['', '', '', '', '', '']
    timeRemaining.value = 300
    startTimer()
    timedInputs.value[0]?.input?.focus()
  } catch (error) {
    console.error('Failed to request new code:', error)
  } finally {
    isRequesting.value = false
  }
}

onMounted(() => {
  startTimer()
})

onUnmounted(() => {
  if (timer) {
    clearInterval(timer)
  }
})
</script>
```

## CSS Classes
- `_c-input`: Base input styling applied to the verify input
- `_c-input-verify`: Specific styling for verification input fields

## Best Practices

### Recommended Usage Patterns
- Always use within a provider component that injects required configuration
- Implement auto-focus progression between input fields
- Handle keyboard navigation including backspace for previous field
- Provide clear visual feedback for completion state
- Use appropriate ARIA labels for screen reader accessibility
- Implement paste handling for user convenience
- Test with various input methods including voice input

### Common Pitfalls to Avoid
- Not sanitizing input properly leading to invalid characters
- Missing keyboard navigation between fields
- Not handling paste events for user convenience
- Using inappropriate input types that bypass validation
- Not providing proper accessibility attributes
- Missing focus management in verification workflows
- Not handling edge cases like rapid input or programmatic changes

### Accessibility Considerations
- Provide descriptive ARIA labels for each input field
- Support keyboard navigation between verification inputs
- Use proper input semantics for screen reader compatibility
- Provide clear instructions about expected input format
- Handle focus management appropriately during verification flow
- Test with screen readers for proper field announcement
- Ensure sufficient color contrast for input states

### Input Validation
- Implement real-time input sanitization to prevent invalid characters
- Use appropriate regex patterns for expected input format
- Handle edge cases in validation logic
- Provide immediate feedback for invalid input
- Validate complete codes before submission
- Handle clipboard paste events with proper validation
- Test validation with various input scenarios

### User Experience
- Implement auto-focus progression between fields
- Handle backspace navigation to previous fields
- Provide visual feedback for completion states
- Support paste operations for user convenience
- Clear invalid input immediately with appropriate feedback
- Handle different verification code lengths dynamically
- Provide retry mechanisms for failed verification

### Form Integration
- Coordinate with parent form validation systems
- Handle form submission appropriately
- Provide proper name attributes for form data
- Support form reset functionality
- Handle required field validation
- Integrate with form error handling
- Support form accessibility standards

### Security Considerations
- Ensure input sanitization prevents injection attacks
- Handle verification codes securely in memory
- Clear verification data appropriately after use
- Validate input length and format server-side
- Implement proper session management for verification flows
- Handle verification timeouts securely
- Log verification attempts appropriately for security monitoring

### Performance Optimization
- Minimize re-renders during input processing
- Use efficient event handling for input sanitization
- Optimize focus management operations
- Handle rapid input efficiently
- Cache provider injection values when appropriate
- Optimize validation regex operations
- Handle large verification flows efficiently

### Testing Strategy
- Test input sanitization with various character inputs
- Verify keyboard navigation works correctly
- Test paste handling with different clipboard formats
- Validate accessibility with screen readers
- Test with automated browser testing using dusk attributes
- Verify validation logic with edge cases
- Test verification flows with different code lengths

### Error Handling
- Handle provider injection failures gracefully
- Validate input patterns appropriately
- Provide clear error messages for invalid input
- Handle verification timeout scenarios
- Manage network errors during verification
- Handle edge cases in input processing
- Provide fallback behavior for critical failures

## Component Registration
The component is registered as `codex-verify-input` in the application. 