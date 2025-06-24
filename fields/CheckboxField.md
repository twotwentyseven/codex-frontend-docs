# CheckboxField Component

## Overview
The CheckboxField component provides a comprehensive checkbox input with label management, hint system, helper text, and error handling. It supports flexible value binding with customizable true/false values, layout configurations, and accessibility features. The component integrates seamlessly with form validation systems while maintaining proper checkbox semantics and user experience.

## Basic Usage
```vue
<template>
  <div class="checkbox-form">
    <codex-checkbox-field
      v-model="agreedToTerms"
      :name="'terms_agreement'"
      :label="'I agree to the Terms and Conditions'"
      :required="true"
    />
  </div>
</template>

<script setup>
const agreedToTerms = ref(false)
</script>
```

## Key Features
- Standard checkbox input with customizable values
- Flexible true/false value configuration
- Label positioning alongside checkbox
- Hint system with tooltip support
- Helper text for additional context
- Error handling and validation integration
- Flexible layout configurations
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
| `label` | `String` | `''` | Checkbox label text |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the checkbox |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |

### Value Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `trueValue` | `String\|Boolean\|Number` | `true` | Value when checkbox is checked |
| `falseValue` | `String\|Boolean\|Number` | `false` | Value when checkbox is unchecked |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'1'` | Layout size (auto, 4, 3, 2, 1) |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `required` | `Boolean` | `true` | Whether checkbox must be checked |
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

The component uses `defineModel()` with support for custom true/false values:

```vue
<template>
  <!-- Boolean checkbox -->
  <codex-checkbox-field v-model="isActive" />
  
  <!-- Custom string values -->
  <codex-checkbox-field 
    v-model="status"
    :true-value="'enabled'"
    :false-value="'disabled'"
  />
  
  <!-- Numeric values -->
  <codex-checkbox-field 
    v-model="priority"
    :true-value="1"
    :false-value="0"
  />
</template>

<script setup>
const isActive = ref(false)      // true/false
const status = ref('disabled')   // 'enabled'/'disabled'
const priority = ref(0)          // 1/0
</script>
```

## Examples

### Terms and Conditions Agreement
```vue
<template>
  <div class="registration-form">
    <h3>Complete Your Registration</h3>
    
    <div class="agreements-section">
      <codex-checkbox-field
        v-model="agreements.terms"
        :name="'terms_agreement'"
        :label="'I agree to the Terms and Conditions'"
        :required="true"
        :errors="agreementErrors.terms"
        :link="'/terms'"
        :hint="'Read our terms and conditions'"
      />
      
      <codex-checkbox-field
        v-model="agreements.privacy"
        :name="'privacy_agreement'"
        :label="'I agree to the Privacy Policy'"
        :required="true"
        :errors="agreementErrors.privacy"
        :link="'/privacy'"
        :hint="'Learn how we protect your data'"
      />
      
      <codex-checkbox-field
        v-model="agreements.marketing"
        :name="'marketing_consent'"
        :label="'I would like to receive marketing communications'"
        :required="false"
        :helper-text="'Optional - you can change this later in your account settings'"
      />
      
      <codex-checkbox-field
        v-model="agreements.newsletter"
        :name="'newsletter_consent'"
        :label="'Subscribe to our newsletter'"
        :required="false"
        :helper-text="'Get weekly updates about new features and products'"
      />
    </div>
    
    <div class="form-actions">
      <button 
        @click="submitRegistration" 
        :disabled="!allRequiredAgreementsAccepted"
        class="register-btn"
      >
        Complete Registration
      </button>
    </div>
    
    <div v-if="agreementErrors.general.length > 0" class="general-errors">
      <div v-for="error in agreementErrors.general" :key="error" class="error-message">
        {{ error }}
      </div>
    </div>
  </div>
</template>

<script setup>
const agreements = reactive({
  terms: false,
  privacy: false,
  marketing: false,
  newsletter: false
})

const agreementErrors = ref({
  terms: [],
  privacy: [],
  general: []
})

const allRequiredAgreementsAccepted = computed(() => {
  return agreements.terms && agreements.privacy
})

const submitRegistration = async () => {
  // Reset errors
  agreementErrors.value = {
    terms: [],
    privacy: [],
    general: []
  }
  
  // Validate required agreements
  if (!agreements.terms) {
    agreementErrors.value.terms = ['You must agree to the Terms and Conditions']
  }
  
  if (!agreements.privacy) {
    agreementErrors.value.privacy = ['You must agree to the Privacy Policy']
  }
  
  if (!allRequiredAgreementsAccepted.value) {
    agreementErrors.value.general = ['Please accept all required agreements to continue']
    return
  }
  
  try {
    await registerUser({
      agreements: agreements
    })
    
    console.log('Registration completed successfully')
    router.push('/welcome')
  } catch (error) {
    agreementErrors.value.general = ['Registration failed. Please try again.']
  }
}
</script>
```

### Settings and Preferences
```vue
<template>
  <div class="user-preferences">
    <h3>Account Preferences</h3>
    
    <div class="preferences-sections">
      <div class="section">
        <h4>Notifications</h4>
        
        <codex-checkbox-field
          v-model="preferences.emailNotifications"
          :name="'email_notifications'"
          :label="'Email notifications'"
          :layout="'2'"
          :helper-text="'Receive important updates via email'"
        />
        
        <codex-checkbox-field
          v-model="preferences.pushNotifications"
          :name="'push_notifications'"
          :label="'Push notifications'"
          :layout="'2'"
          :helper-text="'Receive real-time notifications in your browser'"
        />
        
        <codex-checkbox-field
          v-model="preferences.smsNotifications"
          :name="'sms_notifications'"
          :label="'SMS notifications'"
          :layout="'2'"
          :helper-text="'Receive urgent notifications via text message'"
        />
      </div>
      
      <div class="section">
        <h4>Privacy Settings</h4>
        
        <codex-checkbox-field
          v-model="preferences.profileVisibility"
          :name="'profile_visibility'"
          :label="'Make my profile public'"
          :true-value="'public'"
          :false-value="'private'"
          :helper-text="'Other users can find and view your profile'"
        />
        
        <codex-checkbox-field
          v-model="preferences.dataSharing"
          :name="'data_sharing'"
          :label="'Allow data sharing for analytics'"
          :helper-text="'Help us improve our service by sharing anonymous usage data'"
        />
        
        <codex-checkbox-field
          v-model="preferences.thirdPartySharing"
          :name="'third_party_sharing'"
          :label="'Share data with trusted partners'"
          :helper-text="'Allow sharing with carefully selected partners for enhanced features'"
        />
      </div>
      
      <div class="section">
        <h4>Display Options</h4>
        
        <codex-checkbox-field
          v-model="preferences.darkMode"
          :name="'dark_mode'"
          :label="'Enable dark mode'"
          :layout="'2'"
          :helper-text="'Use dark theme for better viewing in low light'"
        />
        
        <codex-checkbox-field
          v-model="preferences.compactView"
          :name="'compact_view'"
          :label="'Use compact view'"
          :layout="'2'"
          :helper-text="'Show more content in less space'"
        />
        
        <codex-checkbox-field
          v-model="preferences.highContrast"
          :name="'high_contrast'"
          :label="'High contrast mode'"
          :layout="'2'"
          :helper-text="'Improve visibility with higher contrast colors'"
        />
      </div>
    </div>
    
    <div class="preferences-actions">
      <button @click="savePreferences" class="save-btn">
        Save Preferences
      </button>
      <button @click="resetToDefaults" class="reset-btn">
        Reset to Defaults
      </button>
    </div>
  </div>
</template>

<script setup>
const preferences = reactive({
  emailNotifications: true,
  pushNotifications: false,
  smsNotifications: false,
  profileVisibility: 'private',
  dataSharing: false,
  thirdPartySharing: false,
  darkMode: false,
  compactView: false,
  highContrast: false
})

const savePreferences = async () => {
  try {
    await updateUserPreferences(preferences)
    
    // Apply dark mode immediately
    if (preferences.darkMode) {
      document.documentElement.classList.add('dark-mode')
    } else {
      document.documentElement.classList.remove('dark-mode')
    }
    
    toast.success('Preferences saved successfully')
  } catch (error) {
    toast.error('Failed to save preferences')
  }
}

const resetToDefaults = () => {
  Object.assign(preferences, {
    emailNotifications: true,
    pushNotifications: false,
    smsNotifications: false,
    profileVisibility: 'private',
    dataSharing: false,
    thirdPartySharing: false,
    darkMode: false,
    compactView: false,
    highContrast: false
  })
}

// Watch for dark mode changes
watch(() => preferences.darkMode, (newValue) => {
  if (newValue) {
    document.documentElement.classList.add('dark-mode')
  } else {
    document.documentElement.classList.remove('dark-mode')
  }
})
</script>
```

### Feature Toggle Configuration
```vue
<template>
  <div class="feature-toggles">
    <h3>Feature Configuration</h3>
    
    <div class="feature-sections">
      <div class="section">
        <h4>Beta Features</h4>
        <p class="section-description">
          Enable experimental features that are currently in testing
        </p>
        
        <codex-checkbox-field
          v-model="features.newDashboard"
          :name="'new_dashboard'"
          :label="'New Dashboard Interface'"
          :true-value="1"
          :false-value="0"
          :helper-text="'Try our redesigned dashboard with improved navigation'"
          :tooltip-text="'This feature is in beta and may have some issues'"
          :tooltip-icon="'ri-flask-line'"
        />
        
        <codex-checkbox-field
          v-model="features.advancedReporting"
          :name="'advanced_reporting'"
          :label="'Advanced Reporting Tools'"
          :true-value="1"
          :false-value="0"
          :helper-text="'Access advanced analytics and custom reports'"
        />
        
        <codex-checkbox-field
          v-model="features.collaborativeEditing"
          :name="'collaborative_editing'"
          :label="'Real-time Collaborative Editing'"
          :true-value="1"
          :false-value="0"
          :helper-text="'Edit documents simultaneously with team members'"
        />
      </div>
      
      <div class="section">
        <h4>Performance Options</h4>
        
        <codex-checkbox-field
          v-model="features.fastMode"
          :name="'fast_mode'"
          :label="'Enable fast mode'"
          :helper-text="'Optimize for speed over visual effects'"
        />
        
        <codex-checkbox-field
          v-model="features.preloadContent"
          :name="'preload_content'"
          :label="'Preload content'"
          :helper-text="'Load content in advance for faster navigation'"
        />
        
        <codex-checkbox-field
          v-model="features.backgroundSync"
          :name="'background_sync'"
          :label="'Background synchronization'"
          :helper-text="'Keep data synchronized automatically'"
        />
      </div>
      
      <div class="section">
        <h4>Security Features</h4>
        
        <codex-checkbox-field
          v-model="features.twoFactorAuth"
          :name="'two_factor_auth'"
          :label="'Two-factor authentication'"
          :required="false"
          :helper-text="'Add an extra layer of security to your account'"
          :errors="securityErrors.twoFactor"
        />
        
        <codex-checkbox-field
          v-model="features.sessionTimeout"
          :name="'session_timeout'"
          :label="'Automatic session timeout'"
          :helper-text="'Automatically log out after period of inactivity'"
        />
        
        <codex-checkbox-field
          v-model="features.loginAlerts"
          :name="'login_alerts'"
          :label="'Login alerts'"
          :helper-text="'Get notified of new login attempts'"
        />
      </div>
    </div>
    
    <div class="feature-summary" v-if="enabledFeaturesCount > 0">
      <h4>Summary</h4>
      <p>{{ enabledFeaturesCount }} feature{{ enabledFeaturesCount > 1 ? 's' : '' }} enabled</p>
      
      <div class="enabled-features">
        <div v-for="feature in enabledFeaturesList" :key="feature" class="feature-tag">
          {{ feature }}
        </div>
      </div>
    </div>
    
    <div class="feature-actions">
      <button @click="applyFeatures" class="apply-btn">
        Apply Changes
      </button>
      <button @click="exportConfiguration" class="export-btn">
        Export Configuration
      </button>
    </div>
  </div>
</template>

<script setup>
const features = reactive({
  newDashboard: 0,
  advancedReporting: 0,
  collaborativeEditing: 0,
  fastMode: false,
  preloadContent: true,
  backgroundSync: true,
  twoFactorAuth: false,
  sessionTimeout: true,
  loginAlerts: true
})

const securityErrors = ref({
  twoFactor: []
})

const enabledFeaturesCount = computed(() => {
  return Object.values(features).filter(value => 
    value === true || value === 1 || value === 'enabled'
  ).length
})

const enabledFeaturesList = computed(() => {
  const enabled = []
  
  if (features.newDashboard) enabled.push('New Dashboard')
  if (features.advancedReporting) enabled.push('Advanced Reporting')
  if (features.collaborativeEditing) enabled.push('Collaborative Editing')
  if (features.fastMode) enabled.push('Fast Mode')
  if (features.preloadContent) enabled.push('Content Preloading')
  if (features.backgroundSync) enabled.push('Background Sync')
  if (features.twoFactorAuth) enabled.push('Two-Factor Auth')
  if (features.sessionTimeout) enabled.push('Session Timeout')
  if (features.loginAlerts) enabled.push('Login Alerts')
  
  return enabled
})

const applyFeatures = async () => {
  try {
    await updateFeatureConfiguration(features)
    
    toast.success('Feature configuration updated')
    
    // Reload page if certain features changed
    if (features.newDashboard || features.fastMode) {
      window.location.reload()
    }
  } catch (error) {
    toast.error('Failed to update feature configuration')
  }
}

const exportConfiguration = () => {
  const configJson = JSON.stringify(features, null, 2)
  const blob = new Blob([configJson], { type: 'application/json' })
  const url = URL.createObjectURL(blob)
  
  const link = document.createElement('a')
  link.href = url
  link.download = 'feature-configuration.json'
  link.click()
  
  URL.revokeObjectURL(url)
}

// Watch for two-factor auth changes
watch(() => features.twoFactorAuth, (newValue) => {
  if (newValue && !validateTwoFactorSetup()) {
    securityErrors.value.twoFactor = ['Please set up two-factor authentication in your security settings first']
    features.twoFactorAuth = false
  } else {
    securityErrors.value.twoFactor = []
  }
})

const validateTwoFactorSetup = () => {
  // Check if user has already configured 2FA
  return user.value?.hasTwoFactorAuth || false
}
</script>
```

### Survey and Questionnaire
```vue
<template>
  <div class="survey-form">
    <h3>Customer Satisfaction Survey</h3>
    
    <div class="survey-sections">
      <div class="section">
        <h4>Service Experience</h4>
        
        <codex-checkbox-field
          v-model="survey.satisfaction.overall"
          :name="'overall_satisfaction'"
          :label="'Overall, I am satisfied with the service'"
          :layout="'1'"
        />
        
        <codex-checkbox-field
          v-model="survey.satisfaction.support"
          :name="'support_satisfaction'"
          :label="'The support team was helpful and responsive'"
          :layout="'1'"
        />
        
        <codex-checkbox-field
          v-model="survey.satisfaction.speed"
          :name="'speed_satisfaction'"
          :label="'Issues were resolved in a timely manner'"
          :layout="'1'"
        />
      </div>
      
      <div class="section">
        <h4>Features Used</h4>
        <p class="section-description">
          Which features did you use during your experience?
        </p>
        
        <div class="feature-grid">
          <codex-checkbox-field
            v-model="survey.features.dashboard"
            :name="'used_dashboard'"
            :label="'Dashboard'"
            :layout="'3'"
          />
          
          <codex-checkbox-field
            v-model="survey.features.reports"
            :name="'used_reports'"
            :label="'Reports'"
            :layout="'3'"
          />
          
          <codex-checkbox-field
            v-model="survey.features.settings"
            :name="'used_settings'"
            :label="'Settings'"
            :layout="'3'"
          />
          
          <codex-checkbox-field
            v-model="survey.features.support"
            :name="'used_support'"
            :label="'Support Center'"
            :layout="'3'"
          />
          
          <codex-checkbox-field
            v-model="survey.features.integration"
            :name="'used_integration'"
            :label="'Integrations'"
            :layout="'3'"
          />
          
          <codex-checkbox-field
            v-model="survey.features.mobile"
            :name="'used_mobile'"
            :label="'Mobile App'"
            :layout="'3'"
          />
        </div>
      </div>
      
      <div class="section">
        <h4>Recommendations</h4>
        
        <codex-checkbox-field
          v-model="survey.recommendation.wouldRecommend"
          :name="'would_recommend'"
          :label="'I would recommend this service to others'"
          :layout="'1'"
          :helper-text="'Your recommendation helps us understand our impact'"
        />
        
        <codex-checkbox-field
          v-model="survey.recommendation.wouldContinue"
          :name="'would_continue'"
          :label="'I plan to continue using this service'"
          :layout="'1'"
        />
        
        <codex-checkbox-field
          v-model="survey.followUp"
          :name="'follow_up_consent'"
          :label="'I consent to follow-up contact regarding this survey'"
          :required="false"
          :helper-text="'We may contact you to discuss your feedback in more detail'"
        />
      </div>
    </div>
    
    <div class="survey-progress">
      <div class="progress-bar">
        <div class="progress-fill" :style="{ width: completionPercentage + '%' }"></div>
      </div>
      <span class="progress-text">{{ completionPercentage }}% Complete</span>
    </div>
    
    <div class="survey-actions">
      <button @click="saveDraft" class="draft-btn">
        Save Draft
      </button>
      <button @click="submitSurvey" :disabled="!isMinimumComplete" class="submit-btn">
        Submit Survey
      </button>
    </div>
  </div>
</template>

<script setup>
const survey = reactive({
  satisfaction: {
    overall: false,
    support: false,
    speed: false
  },
  features: {
    dashboard: false,
    reports: false,
    settings: false,
    support: false,
    integration: false,
    mobile: false
  },
  recommendation: {
    wouldRecommend: false,
    wouldContinue: false
  },
  followUp: false
})

const completionPercentage = computed(() => {
  const totalFields = 11 // Total number of checkboxes
  const completedFields = [
    ...Object.values(survey.satisfaction),
    ...Object.values(survey.features),
    ...Object.values(survey.recommendation)
  ].filter(Boolean).length
  
  return Math.round((completedFields / totalFields) * 100)
})

const isMinimumComplete = computed(() => {
  // Require at least satisfaction section to be complete
  return Object.values(survey.satisfaction).some(Boolean)
})

const saveDraft = async () => {
  try {
    await saveSurveyDraft(survey)
    toast.success('Survey draft saved')
  } catch (error) {
    toast.error('Failed to save draft')
  }
}

const submitSurvey = async () => {
  try {
    await submitCustomerSurvey(survey)
    toast.success('Thank you for your feedback!')
    router.push('/survey-complete')
  } catch (error) {
    toast.error('Failed to submit survey')
  }
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
| `_c-checkbox-container` | Container for checkbox and label |
| `_c-label-container` | Container for label and hint |

## Best Practices

### Checkbox Design
- Use clear, concise labels that explain what checking the box means
- Position labels to the right of checkboxes for better readability
- Group related checkboxes logically
- Use appropriate spacing between checkbox groups

### Value Configuration
- Use boolean values (true/false) for simple yes/no scenarios
- Use custom values (strings, numbers) when integrating with specific APIs
- Be consistent with value types across related checkboxes
- Document custom value meanings for team members

### Accessibility
- Ensure proper label association with checkbox inputs
- Use ARIA labels for additional context when needed
- Provide clear error messages for required checkboxes
- Test with screen readers and keyboard navigation

### User Experience
- Make checkbox targets large enough for easy clicking/tapping
- Provide clear visual feedback for checked/unchecked states
- Use helper text to clarify implications of checkbox choices
- Group related options to reduce cognitive load

### Validation
- Validate required checkboxes on form submission
- Provide immediate feedback for validation errors
- Use clear error messages that guide users
- Consider progressive validation for complex forms

### Legal and Compliance
- Use clear language for terms and privacy agreements
- Provide easy access to linked documents (terms, privacy policy)
- Separate required agreements from optional preferences
- Maintain audit trails for compliance requirements

### Performance
- Avoid unnecessary re-renders with proper reactive references
- Use computed properties for derived checkbox states
- Implement proper change tracking for form state
- Cache checkbox configurations when appropriate

## Component Registration
```javascript
// Global registration
app.component('CodexCheckboxField', CheckboxField)

// Local registration  
import CheckboxField from '@/components/fields/CheckboxField.vue'

export default {
  components: {
    CodexCheckboxField: CheckboxField
  }
}
``` 