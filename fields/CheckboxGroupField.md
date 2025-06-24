# CheckboxGroupField Component

## Overview
The CheckboxGroupField component provides a group of checkbox inputs for multiple selection scenarios. It supports both traditional checkbox layout and button-style variants, allowing users to select multiple options from a predefined list. The component manages an array of selected values and provides comprehensive form integration with validation, error handling, and accessibility features.

## Basic Usage
```vue
<template>
  <div class="checkbox-group-form">
    <codex-checkbox-group-field
      v-model="selectedOptions"
      :name="'preferences'"
      :label="'Select your preferences'"
      :options="preferenceOptions"
      :required="true"
    />
  </div>
</template>

<script setup>
const selectedOptions = ref([])

const preferenceOptions = [
  { value: 'email', displayValue: 'Email Notifications' },
  { value: 'sms', displayValue: 'SMS Notifications' },
  { value: 'push', displayValue: 'Push Notifications' },
  { value: 'newsletter', displayValue: 'Newsletter' }
]
</script>
```

## Key Features
- Multiple checkbox selection with array value management
- Traditional checkbox and button variant support
- Dynamic option rendering from array configuration
- Custom true/false values for individual checkboxes
- Flexible layout configurations (auto, quarter, third, half, full)
- Label and hint system with tooltip support
- Helper text and error handling
- Form validation integration
- Accessibility features with proper grouping
- Testing support with Dusk attributes

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `name` | `String` | `required` | Field name for form submission |

### Options Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `options` | `Array` | `[]` | Array of checkbox options |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the checkbox group |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |
| `placeholder` | `String` | `''` | Placeholder text |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'1'` | Layout size (auto, 4, 3, 2, 1) |
| `variant` | `String` | `'default'` | Checkbox style (default, button) |

### Value Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `trueValue` | `String\|Boolean\|Number` | `true` | Value when checkbox is checked |
| `falseValue` | `String\|Boolean\|Number` | `false` | Value when checkbox is unchecked |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `required` | `Boolean` | `true` | Whether at least one selection is required |
| `hasError` | `Boolean` | `false` | Whether field has error state |
| `type` | `String` | `'checkbox-group'` | Input type identifier |

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

The component uses `defineModel({ type: Array, default: () => [] })`:

```vue
<template>
  <codex-checkbox-group-field v-model="selections" :options="options" />
</template>

<script setup>
const selections = ref([])
// selections automatically updates as user checks/unchecks options
// Contains array of selected option values
</script>
```

## Option Configuration

Options should be provided as an array of objects:

```javascript
const options = [
  { 
    value: 'option1',           // Value stored in model
    displayValue: 'Option 1',   // Label shown to user
    disabled: false             // Optional: disable specific option
  },
  { 
    value: 'option2', 
    displayValue: 'Option 2' 
  }
]
```

## Examples

### User Preferences Form
```vue
<template>
  <div class="user-preferences">
    <h3>Notification Preferences</h3>
    
    <div class="preferences-form">
      <codex-checkbox-group-field
        v-model="preferences.notifications"
        :name="'notification_types'"
        :label="'How would you like to be notified?'"
        :options="notificationOptions"
        :layout="'2'"
        :required="true"
        :errors="preferencesErrors.notifications"
        :helper-text="'Select at least one notification method'"
        :variant="'default'"
      />
      
      <codex-checkbox-group-field
        v-model="preferences.contentTypes"
        :name="'content_types'"
        :label="'What content interests you?'"
        :options="contentOptions"
        :layout="'2'"
        :required="false"
        :helper-text="'Choose topics you want to hear about'"
        :variant="'button'"
      />
      
      <codex-checkbox-group-field
        v-model="preferences.features"
        :name="'beta_features'"
        :label="'Beta Features'"
        :options="betaFeatureOptions"
        :layout="'1'"
        :required="false"
        :helper-text="'Try experimental features before general release'"
      />
      
      <codex-checkbox-group-field
        v-model="preferences.frequency"
        :name="'email_frequency'"
        :label="'Email Frequency'"
        :options="frequencyOptions"
        :layout="'3'"
        :required="false"
        :helper-text="'How often should we send emails?'"
      />
    </div>
    
    <div class="preferences-summary" v-if="hasPreferences">
      <h4>Your Preferences</h4>
      
      <div class="summary-section" v-if="preferences.notifications.length > 0">
        <h5>Notifications</h5>
        <div class="preference-tags">
          <span v-for="type in selectedNotificationLabels" :key="type" class="preference-tag notification">
            {{ type }}
          </span>
        </div>
      </div>
      
      <div class="summary-section" v-if="preferences.contentTypes.length > 0">
        <h5>Content Interests</h5>
        <div class="preference-tags">
          <span v-for="content in selectedContentLabels" :key="content" class="preference-tag content">
            {{ content }}
          </span>
        </div>
      </div>
      
      <div class="summary-section" v-if="preferences.features.length > 0">
        <h5>Beta Features</h5>
        <div class="preference-tags">
          <span v-for="feature in selectedFeatureLabels" :key="feature" class="preference-tag feature">
            {{ feature }}
          </span>
        </div>
      </div>
      
      <div class="preference-statistics">
        <div class="stat-item">
          <span class="stat-value">{{ totalSelections }}</span>
          <span class="stat-label">Total Preferences</span>
        </div>
        <div class="stat-item">
          <span class="stat-value">{{ customizationLevel }}%</span>
          <span class="stat-label">Customization Level</span>
        </div>
      </div>
    </div>
    
    <div class="preferences-actions">
      <button @click="resetPreferences" class="reset-btn">
        Reset to Defaults
      </button>
      <button @click="savePreferences" :disabled="!canSavePreferences" class="save-btn">
        Save Preferences
      </button>
    </div>
  </div>
</template>

<script setup>
const preferences = reactive({
  notifications: [],
  contentTypes: [],
  features: [],
  frequency: []
})

const preferencesErrors = ref({
  notifications: []
})

const notificationOptions = [
  { value: 'email', displayValue: 'Email Notifications' },
  { value: 'sms', displayValue: 'SMS Text Messages' },
  { value: 'push', displayValue: 'Push Notifications' },
  { value: 'in_app', displayValue: 'In-App Notifications' }
]

const contentOptions = [
  { value: 'news', displayValue: 'News & Updates' },
  { value: 'tutorials', displayValue: 'Tutorials & Guides' },
  { value: 'products', displayValue: 'Product Announcements' },
  { value: 'events', displayValue: 'Events & Webinars' },
  { value: 'community', displayValue: 'Community Posts' },
  { value: 'offers', displayValue: 'Special Offers' }
]

const betaFeatureOptions = [
  { value: 'advanced_analytics', displayValue: 'Advanced Analytics Dashboard' },
  { value: 'ai_assistant', displayValue: 'AI Writing Assistant' },
  { value: 'dark_mode', displayValue: 'Dark Mode Theme' },
  { value: 'mobile_app', displayValue: 'Mobile App Beta' }
]

const frequencyOptions = [
  { value: 'immediate', displayValue: 'Immediate' },
  { value: 'daily', displayValue: 'Daily Digest' },
  { value: 'weekly', displayValue: 'Weekly Summary' },
  { value: 'monthly', displayValue: 'Monthly Newsletter' }
]

const hasPreferences = computed(() => {
  return Object.values(preferences).some(prefArray => prefArray.length > 0)
})

const selectedNotificationLabels = computed(() => {
  return getSelectedLabels(preferences.notifications, notificationOptions)
})

const selectedContentLabels = computed(() => {
  return getSelectedLabels(preferences.contentTypes, contentOptions)
})

const selectedFeatureLabels = computed(() => {
  return getSelectedLabels(preferences.features, betaFeatureOptions)
})

const totalSelections = computed(() => {
  return Object.values(preferences).reduce((total, prefArray) => total + prefArray.length, 0)
})

const customizationLevel = computed(() => {
  const totalOptions = notificationOptions.length + contentOptions.length + betaFeatureOptions.length + frequencyOptions.length
  return Math.round((totalSelections.value / totalOptions) * 100)
})

const canSavePreferences = computed(() => {
  return preferences.notifications.length > 0 // At least one notification method required
})

const getSelectedLabels = (selectedValues, options) => {
  return selectedValues.map(value => {
    const option = options.find(opt => opt.value === value)
    return option ? option.displayValue : value
  })
}

const resetPreferences = () => {
  Object.keys(preferences).forEach(key => {
    preferences[key] = []
  })
  
  // Set some sensible defaults
  preferences.notifications = ['email']
  preferences.frequency = ['weekly']
  
  toast.info('Preferences reset to defaults')
}

const savePreferences = async () => {
  try {
    await saveUserPreferences(preferences)
    toast.success('Preferences saved successfully!')
  } catch (error) {
    if (error.response?.data?.errors) {
      preferencesErrors.value = error.response.data.errors
    }
    toast.error('Failed to save preferences')
  }
}
</script>
```

### Skills Assessment Form
```vue
<template>
  <div class="skills-assessment">
    <h3>Skills Assessment</h3>
    
    <div class="assessment-sections">
      <div class="skill-category">
        <h4>Technical Skills</h4>
        
        <codex-checkbox-group-field
          v-model="skills.programmingLanguages"
          :name="'programming_languages'"
          :label="'Programming Languages'"
          :options="programmingOptions"
          :layout="'3'"
          :required="false"
          :helper-text="'Select languages you are proficient in'"
          :variant="'button'"
        />
        
        <codex-checkbox-group-field
          v-model="skills.frameworks"
          :name="'frameworks'"
          :label="'Frameworks & Libraries'"
          :options="frameworkOptions"
          :layout="'3'"
          :required="false"
          :helper-text="'Choose frameworks you have experience with'"
          :variant="'button'"
        />
        
        <codex-checkbox-group-field
          v-model="skills.databases"
          :name="'databases'"
          :label="'Databases'"
          :options="databaseOptions"
          :layout="'2'"
          :required="false"
          :helper-text="'Database technologies you have worked with'"
        />
      </div>
      
      <div class="skill-category">
        <h4>Soft Skills</h4>
        
        <codex-checkbox-group-field
          v-model="skills.communication"
          :name="'communication_skills'"
          :label="'Communication Skills'"
          :options="communicationOptions"
          :layout="'2'"
          :required="false"
          :helper-text="'Your communication strengths'"
        />
        
        <codex-checkbox-group-field
          v-model="skills.leadership"
          :name="'leadership_skills'"
          :label="'Leadership Experience'"
          :options="leadershipOptions"
          :layout="'2'"
          :required="false"
          :helper-text="'Leadership roles and experiences'"
        />
      </div>
      
      <div class="skill-category">
        <h4>Industry Experience</h4>
        
        <codex-checkbox-group-field
          v-model="skills.industries"
          :name="'industry_experience'"
          :label="'Industry Sectors'"
          :options="industryOptions"
          :layout="'3'"
          :required="false"
          :helper-text="'Industries you have worked in'"
        />
        
        <codex-checkbox-group-field
          v-model="skills.certifications"
          :name="'certifications'"
          :label="'Professional Certifications'"
          :options="certificationOptions"
          :layout="'2'"
          :required="false"
          :helper-text="'Current professional certifications'"
        />
      </div>
    </div>
    
    <div class="skills-analysis" v-if="hasSelectedSkills">
      <h4>Skills Analysis</h4>
      
      <div class="analysis-grid">
        <div class="analysis-card">
          <h5>Technical Proficiency</h5>
          <div class="skill-count">{{ technicalSkillsCount }}</div>
          <div class="skill-level" :class="technicalSkillLevel.class">
            {{ technicalSkillLevel.label }}
          </div>
        </div>
        
        <div class="analysis-card">
          <h5>Soft Skills</h5>
          <div class="skill-count">{{ softSkillsCount }}</div>
          <div class="skill-level" :class="softSkillLevel.class">
            {{ softSkillLevel.label }}
          </div>
        </div>
        
        <div class="analysis-card">
          <h5>Industry Breadth</h5>
          <div class="skill-count">{{ skills.industries.length }}</div>
          <div class="skill-level" :class="industryBreadthLevel.class">
            {{ industryBreadthLevel.label }}
          </div>
        </div>
        
        <div class="analysis-card">
          <h5>Certifications</h5>
          <div class="skill-count">{{ skills.certifications.length }}</div>
          <div class="skill-level" :class="certificationLevel.class">
            {{ certificationLevel.label }}
          </div>
        </div>
      </div>
      
      <div class="skill-recommendations" v-if="skillRecommendations.length > 0">
        <h5>Skill Development Recommendations</h5>
        <div class="recommendations-list">
          <div v-for="recommendation in skillRecommendations" :key="recommendation.skill" class="recommendation-item">
            <i class="ri-lightbulb-line"></i>
            <span><strong>{{ recommendation.skill }}:</strong> {{ recommendation.reason }}</span>
          </div>
        </div>
      </div>
    </div>
    
    <div class="assessment-actions">
      <button @click="generateResume" :disabled="!hasMinimumSkills" class="generate-btn">
        Generate Skills-Based Resume
      </button>
      <button @click="findMatchingJobs" :disabled="!hasSelectedSkills" class="jobs-btn">
        Find Matching Jobs
      </button>
      <button @click="saveAssessment" class="save-btn">
        Save Assessment
      </button>
    </div>
  </div>
</template>

<script setup>
const skills = reactive({
  programmingLanguages: [],
  frameworks: [],
  databases: [],
  communication: [],
  leadership: [],
  industries: [],
  certifications: []
})

// Options arrays would be defined here...
const programmingOptions = [
  { value: 'javascript', displayValue: 'JavaScript' },
  { value: 'python', displayValue: 'Python' },
  { value: 'java', displayValue: 'Java' },
  { value: 'csharp', displayValue: 'C#' },
  { value: 'php', displayValue: 'PHP' },
  { value: 'ruby', displayValue: 'Ruby' }
]

// More options arrays...

const hasSelectedSkills = computed(() => {
  return Object.values(skills).some(skillArray => skillArray.length > 0)
})

const technicalSkillsCount = computed(() => {
  return skills.programmingLanguages.length + skills.frameworks.length + skills.databases.length
})

const softSkillsCount = computed(() => {
  return skills.communication.length + skills.leadership.length
})

const technicalSkillLevel = computed(() => {
  const count = technicalSkillsCount.value
  if (count >= 10) return { label: 'Expert', class: 'expert' }
  if (count >= 6) return { label: 'Advanced', class: 'advanced' }
  if (count >= 3) return { label: 'Intermediate', class: 'intermediate' }
  if (count > 0) return { label: 'Beginner', class: 'beginner' }
  return { label: 'No skills selected', class: 'none' }
})

const softSkillLevel = computed(() => {
  const count = softSkillsCount.value
  if (count >= 6) return { label: 'Strong', class: 'strong' }
  if (count >= 3) return { label: 'Good', class: 'good' }
  if (count > 0) return { label: 'Basic', class: 'basic' }
  return { label: 'No skills selected', class: 'none' }
})

const hasMinimumSkills = computed(() => {
  return technicalSkillsCount.value >= 3 && softSkillsCount.value >= 2
})

const skillRecommendations = computed(() => {
  const recommendations = []
  
  if (skills.programmingLanguages.includes('javascript') && !skills.frameworks.some(f => ['react', 'vue', 'angular'].includes(f))) {
    recommendations.push({
      skill: 'Modern JavaScript Framework',
      reason: 'Complement your JavaScript skills with React, Vue, or Angular'
    })
  }
  
  return recommendations
})

const generateResume = () => {
  // Generate skills-based resume
  router.push('/resume/generate?skills=true')
}

const findMatchingJobs = () => {
  // Find jobs matching selected skills
  router.push('/jobs/search?skills=' + encodeURIComponent(JSON.stringify(skills)))
}

const saveAssessment = async () => {
  try {
    await saveSkillsAssessment(skills)
    toast.success('Skills assessment saved!')
  } catch (error) {
    toast.error('Failed to save assessment')
  }
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input-container` | Main container for default variant |
| `_c-btn-container` | Main container for button variant |
| `_c-w-auto` | Auto width styling |
| `_c-checkbox-container` | Container for checkbox items |
| `_c-checkbox-button` | Container for button-style checkboxes |
| `_c-form-field--auto` | Auto-sized layout |
| `_c-form-field--quarter` | Quarter width layout (25%) |
| `_c-form-field--third` | Third width layout (33%) |
| `_c-form-field--half` | Half width layout (50%) |
| `_c-form-field--full` | Full width layout (100%) |
| `_c-label-container` | Container for label and hint |

## Best Practices

### Option Management
- Provide clear, descriptive labels for each option
- Use logical grouping and ordering of options
- Consider option dependencies and mutual exclusions
- Keep option lists to a reasonable length for UX
- Use button variant for aesthetic appeal with fewer options

### User Experience
- Group related options together logically
- Provide helpful descriptions for complex options
- Show selection counts and summaries when useful
- Allow bulk selection/deselection when appropriate
- Use progressive disclosure for large option sets

### Validation
- Validate minimum/maximum selection requirements
- Provide clear feedback for validation errors
- Consider conditional validation based on other form fields
- Handle edge cases like all options disabled
- Validate on both client and server side

### Accessibility
- Ensure proper fieldset and legend association
- Use ARIA labels for option groups
- Support keyboard navigation between options
- Test with screen readers and assistive technologies
- Provide clear instructions for selection requirements

### Performance
- Use efficient array operations for selection changes
- Debounce validation for better performance
- Consider virtual scrolling for very large option lists
- Optimize rendering for dynamic option updates
- Implement proper key management for option lists

### Data Handling
- Store selections as arrays of option values
- Handle empty selections gracefully
- Validate option values against available options
- Consider data normalization for consistent storage
- Handle option updates without losing user selections

## Component Registration
```javascript
// Global registration
app.component('CodexCheckboxGroupField', CheckboxGroupField)

// Local registration  
import CheckboxGroupField from '@/components/fields/CheckboxGroupField.vue'

export default {
  components: {
    CodexCheckboxGroupField: CheckboxGroupField
  }
}
``` 