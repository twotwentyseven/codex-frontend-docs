# VerificationField Component

## Overview
The VerificationField component provides a specialized input for verification codes, typically used for two-factor authentication, email verification, or SMS verification. It creates a series of individual input fields (usually 6) that automatically advance focus as the user types, providing an intuitive interface for entering verification tokens with proper validation and accessibility features.

## Basic Usage
```vue
<template>
  <div class="verification-form">
    <codex-verification-field
      v-model="verificationCode"
      :name="'verification_code'"
      :label="'Enter Verification Code'"
      :required="true"
      :helper-text="'Check your email for the 6-digit code'"
    />
  </div>
</template>

<script setup>
const verificationCode = ref('')
</script>
```

## Key Features
- Six individual input fields for verification codes
- Automatic focus advancement between fields
- Numeric input validation and sanitization
- Automatic code completion and blur handling
- Flexible layout configurations
- Label and hint system with tooltip support
- Helper text and error handling
- Accessibility features with proper labeling
- Testing support with Dusk attributes

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `name` | `String` | `required` | Field name for form submission |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
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
| `required` | `Boolean` | `false` | Whether field is required |
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
| `type` | `String` | `'verification'` | Input type for verification |

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

The component uses `defineModel({ type: String })`:

```vue
<template>
  <codex-verification-field v-model="code" />
</template>

<script setup>
const code = ref('')
// code automatically updates as user types in individual fields
// Results in a 6-character string like "123456"
</script>
```

## Examples

### Two-Factor Authentication Setup
```vue
<template>
  <div class="two-factor-setup">
    <h3>Enable Two-Factor Authentication</h3>
    
    <div class="setup-steps">
      <div class="step" :class="{ active: currentStep === 1, completed: currentStep > 1 }">
        <h4>Step 1: Install Authenticator App</h4>
        <p>Download and install an authenticator app like Google Authenticator or Authy</p>
        <div class="app-links">
          <a href="#" class="app-link">
            <i class="ri-apple-line"></i>
            iOS App Store
          </a>
          <a href="#" class="app-link">
            <i class="ri-google-play-line"></i>
            Google Play
          </a>
        </div>
        <button v-if="currentStep === 1" @click="currentStep = 2" class="next-btn">
          Next Step
        </button>
      </div>
      
      <div class="step" :class="{ active: currentStep === 2, completed: currentStep > 2 }">
        <h4>Step 2: Scan QR Code</h4>
        <div class="qr-section">
          <div class="qr-code">
            <img :src="qrCodeUrl" alt="QR Code for 2FA setup" />
          </div>
          <div class="manual-entry">
            <p>Can't scan? Enter this code manually:</p>
            <code class="secret-key">{{ secretKey }}</code>
            <button @click="copySecretKey" class="copy-btn">
              <i class="ri-file-copy-line"></i>
              Copy
            </button>
          </div>
        </div>
        <button v-if="currentStep === 2" @click="currentStep = 3" class="next-btn">
          I've Added the Account
        </button>
      </div>
      
      <div class="step" :class="{ active: currentStep === 3 }">
        <h4>Step 3: Verify Setup</h4>
        <p>Enter the 6-digit code from your authenticator app to verify the setup:</p>
        
        <codex-verification-field
          v-model="verification.setupCode"
          :name="'setup_verification'"
          :label="'Verification Code'"
          :required="true"
          :errors="verificationErrors.setupCode"
          :helper-text="'Enter the code from your authenticator app'"
          :layout="'1'"
        />
        
        <div class="verification-actions">
          <button @click="verifySetup" :disabled="!canVerifySetup" class="verify-btn">
            Verify and Enable 2FA
          </button>
          <button @click="currentStep = 2" class="back-btn">
            Back to QR Code
          </button>
        </div>
      </div>
    </div>
    
    <div v-if="setupComplete" class="setup-success">
      <div class="success-message">
        <i class="ri-check-line"></i>
        <h4>Two-Factor Authentication Enabled!</h4>
        <p>Your account is now protected with 2FA. Save these backup codes in a safe place:</p>
        
        <div class="backup-codes">
          <div v-for="code in backupCodes" :key="code" class="backup-code">
            {{ code }}
          </div>
        </div>
        
        <button @click="downloadBackupCodes" class="download-btn">
          Download Backup Codes
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
const currentStep = ref(1)
const setupComplete = ref(false)

const verification = reactive({
  setupCode: ''
})

const verificationErrors = ref({
  setupCode: []
})

const qrCodeUrl = ref('/api/2fa/qr-code')
const secretKey = ref('JBSWY3DPEHPK3PXP')
const backupCodes = ref([])

const canVerifySetup = computed(() => {
  return verification.setupCode.length === 6 && /^\d{6}$/.test(verification.setupCode)
})

const copySecretKey = async () => {
  try {
    await navigator.clipboard.writeText(secretKey.value)
    toast.success('Secret key copied to clipboard')
  } catch (error) {
    toast.error('Failed to copy secret key')
  }
}

const verifySetup = async () => {
  try {
    const response = await verify2FASetup({
      code: verification.setupCode,
      secret: secretKey.value
    })
    
    backupCodes.value = response.backupCodes
    setupComplete.value = true
    
    toast.success('Two-factor authentication enabled successfully!')
  } catch (error) {
    if (error.response?.data?.errors) {
      verificationErrors.value = error.response.data.errors
    } else {
      verificationErrors.value.setupCode = ['Invalid verification code. Please try again.']
    }
    
    toast.error('Verification failed. Please check your code.')
  }
}

const downloadBackupCodes = () => {
  const content = backupCodes.value.join('\n')
  const blob = new Blob([content], { type: 'text/plain' })
  const url = URL.createObjectURL(blob)
  
  const link = document.createElement('a')
  link.href = url
  link.download = '2fa-backup-codes.txt'
  link.click()
  
  URL.revokeObjectURL(url)
}
</script>
```

### Email Verification Process
```vue
<template>
  <div class="email-verification">
    <h3>Verify Your Email Address</h3>
    
    <div class="verification-steps">
      <div class="email-info">
        <div class="email-icon">
          <i class="ri-mail-line"></i>
        </div>
        <div class="email-details">
          <h4>Check Your Email</h4>
          <p>We sent a verification code to:</p>
          <strong>{{ userEmail }}</strong>
          <p class="email-note">The code will expire in {{ timeRemaining }} minutes</p>
        </div>
      </div>
      
      <div class="verification-input">
        <codex-verification-field
          v-model="emailVerification.code"
          :name="'email_verification'"
          :label="'Enter 6-Digit Code'"
          :required="true"
          :errors="emailVerificationErrors.code"
          :helper-text="emailHelperText"
          :layout="'1'"
        />
        
        <div class="verification-actions">
          <button @click="verifyEmail" :disabled="!canVerifyEmail" class="verify-btn">
            Verify Email
          </button>
          <button @click="resendCode" :disabled="!canResend" class="resend-btn">
            {{ resendButtonText }}
          </button>
        </div>
      </div>
      
      <div class="verification-options">
        <div class="option-item">
          <i class="ri-information-line"></i>
          <span>Didn't receive the email? Check your spam folder</span>
        </div>
        <div class="option-item">
          <i class="ri-edit-line"></i>
          <button @click="changeEmail" class="change-email-btn">
            Use a different email address
          </button>
        </div>
      </div>
    </div>
    
    <div v-if="verificationAttempts.length > 0" class="attempt-history">
      <h4>Verification Attempts</h4>
      <div class="attempts-list">
        <div v-for="attempt in verificationAttempts" :key="attempt.id" class="attempt-item" :class="attempt.status">
          <div class="attempt-info">
            <span class="attempt-time">{{ formatTime(attempt.timestamp) }}</span>
            <span class="attempt-status">{{ attempt.status }}</span>
          </div>
          <div v-if="attempt.error" class="attempt-error">
            {{ attempt.error }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const emailVerification = reactive({
  code: ''
})

const emailVerificationErrors = ref({
  code: []
})

const userEmail = ref('user@example.com')
const timeRemaining = ref(15)
const canResend = ref(false)
const resendCooldown = ref(60)
const verificationAttempts = ref([])

const canVerifyEmail = computed(() => {
  return emailVerification.code.length === 6 && /^\d{6}$/.test(emailVerification.code)
})

const emailHelperText = computed(() => {
  if (emailVerification.code.length === 0) {
    return 'Enter the 6-digit code from your email'
  }
  if (emailVerification.code.length < 6) {
    return `${emailVerification.code.length}/6 digits entered`
  }
  return 'Code complete - click verify'
})

const resendButtonText = computed(() => {
  if (canResend.value) {
    return 'Resend Code'
  }
  return `Resend in ${resendCooldown.value}s`
})

const verifyEmail = async () => {
  try {
    await verifyEmailAddress({
      email: userEmail.value,
      code: emailVerification.code
    })
    
    verificationAttempts.value.unshift({
      id: Date.now(),
      timestamp: new Date(),
      status: 'success'
    })
    
    toast.success('Email verified successfully!')
    router.push('/dashboard')
  } catch (error) {
    const errorMessage = error.response?.data?.message || 'Invalid verification code'
    
    verificationAttempts.value.unshift({
      id: Date.now(),
      timestamp: new Date(),
      status: 'failed',
      error: errorMessage
    })
    
    if (error.response?.data?.errors) {
      emailVerificationErrors.value = error.response.data.errors
    } else {
      emailVerificationErrors.value.code = [errorMessage]
    }
    
    // Clear the code for retry
    emailVerification.code = ''
    
    toast.error('Verification failed. Please try again.')
  }
}

const resendCode = async () => {
  try {
    await resendVerificationCode({ email: userEmail.value })
    
    toast.success('Verification code sent!')
    
    // Reset timer
    timeRemaining.value = 15
    canResend.value = false
    resendCooldown.value = 60
    
    // Start cooldown timer
    const cooldownTimer = setInterval(() => {
      resendCooldown.value--
      if (resendCooldown.value <= 0) {
        canResend.value = true
        clearInterval(cooldownTimer)
      }
    }, 1000)
    
  } catch (error) {
    toast.error('Failed to resend code. Please try again.')
  }
}

const changeEmail = () => {
  router.push('/change-email')
}

const formatTime = (timestamp) => {
  return new Date(timestamp).toLocaleTimeString()
}

// Start expiration timer
onMounted(() => {
  const expirationTimer = setInterval(() => {
    timeRemaining.value--
    if (timeRemaining.value <= 0) {
      clearInterval(expirationTimer)
      toast.warning('Verification code expired. Please request a new one.')
      canResend.value = true
    }
  }, 60000) // Update every minute
  
  // Start resend cooldown
  setTimeout(() => {
    canResend.value = true
  }, 60000) // 1 minute cooldown
})
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input-container` | Main container for the field |
| `_c-input-container-inner` | Inner container for input fields |
| `_c-form-field--auto` | Auto-sized layout |
| `_c-form-field--quarter` | Quarter width layout (25%) |
| `_c-form-field--third` | Third width layout (33%) |
| `_c-form-field--half` | Half width layout (50%) |
| `_c-form-field--full` | Full width layout (100%) |
| `_c-label-container` | Container for label and hint |

## Best Practices

### User Experience
- Provide clear instructions about where to find the verification code
- Show visual progress as users type (character count)
- Automatically advance focus between input fields
- Clear invalid codes and allow easy retry
- Provide resend functionality with appropriate cooldowns

### Security
- Implement reasonable attempt limits to prevent brute force
- Use secure code generation with sufficient entropy
- Set appropriate expiration times for codes
- Log verification attempts for security monitoring
- Clear codes from memory after successful verification

### Accessibility
- Ensure proper labeling for screen readers
- Support keyboard navigation between fields
- Provide clear error messages and instructions
- Test with assistive technologies
- Use appropriate ARIA attributes for the field group

### Validation
- Validate input as numeric only
- Prevent non-digit characters
- Provide immediate feedback for invalid formats
- Handle partial input gracefully
- Validate code length and format

### Mobile Optimization
- Trigger numeric keyboard on mobile devices
- Ensure input fields are appropriately sized for touch
- Test paste functionality for codes from SMS
- Consider auto-fill from SMS on supported devices
- Provide clear visual feedback for field focus

### Error Handling
- Provide specific error messages for different failure types
- Allow multiple attempts with clear feedback
- Implement progressive delays for repeated failures
- Offer alternative verification methods when available
- Handle network errors gracefully

## Component Registration
```javascript
// Global registration
app.component('CodexVerificationField', VerificationField)

// Local registration  
import VerificationField from '@/components/fields/VerificationField.vue'

export default {
  components: {
    CodexVerificationField: VerificationField
  }
}
``` 