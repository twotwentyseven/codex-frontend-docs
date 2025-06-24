# RepeatingTextField Component

## Overview
The RepeatingTextField component provides a dynamic input field system that allows users to add and remove multiple text inputs of the same type. It features a checkbox toggle to show/hide the inputs, dynamic field addition/removal, and comprehensive form integration. This component is ideal for collecting multiple values of the same type, such as skills, interests, phone numbers, or email addresses.

## Basic Usage
```vue
<template>
  <div class="repeating-form">
    <codex-repeating-text-field
      v-model="skills"
      :field="skillsField"
      :key="'skills'"
      :name="'skills'"
      :type="'text'"
      :label="'Add Skills'"
      :required="false"
    />
  </div>
</template>

<script setup>
const skills = ref([''])

const skillsField = {
  add_another_button_text: 'Add Another Skill'
}
</script>
```

## Key Features
- Dynamic addition and removal of text input fields
- Checkbox toggle to show/hide input fields
- Profile mode for persistent display
- Array-based model binding with automatic management
- Custom "Add Another" button text configuration
- Flexible layout configurations
- Label and hint system with tooltip support
- Helper text and error handling
- Readonly state management
- Testing support with Dusk attributes

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `field` | `Object` | `required` | Field configuration object |
| `key` | `String` | `required` | Unique key for the field |
| `name` | `String` | `required` | Field name for form submission |
| `type` | `String` | `required` | Input type for text fields |

### Field Configuration
| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `add_another_button_text` | `String` | `'Add Another'` | Text for the add button |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
| `placeholder` | `String` | `''` | Placeholder text for inputs |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the field |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'1'` | Layout size (auto, 4, 3, 2, 1) |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `required` | `Boolean` | `true` | Whether field is required |
| `readonly` | `Boolean` | `false` | Whether fields are readonly |
| `disabled` | `Boolean` | `false` | Whether field is disabled |
| `hasError` | `Boolean\|String` | `false` | Whether field has error state |
| `profile` | `Boolean` | `false` | Profile mode (always show if has values) |
| `showLabel` | `Boolean` | `true` | Whether to show label |

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

The component uses `defineModel({ type: Array, default: () => [''] })`:

```vue
<template>
  <codex-repeating-text-field v-model="items" />
</template>

<script setup>
const items = ref([''])
// items automatically updates as user adds/removes fields
// Always maintains at least one empty string entry
</script>
```

## Examples

### Skills and Expertise Form
```vue
<template>
  <div class="skills-form">
    <h3>Professional Profile</h3>
    
    <div class="skills-sections">
      <codex-repeating-text-field
        v-model="profile.technicalSkills"
        :field="{ add_another_button_text: 'Add Technical Skill' }"
        :key="'technical_skills'"
        :name="'technical_skills'"
        :type="'text'"
        :label="'Technical Skills'"
        :placeholder="'e.g., JavaScript, Python, React'"
        :layout="'2'"
        :required="false"
        :errors="profileErrors.technicalSkills"
        :helper-text="'List your technical competencies'"
      />
      
      <codex-repeating-text-field
        v-model="profile.softSkills"
        :field="{ add_another_button_text: 'Add Soft Skill' }"
        :key="'soft_skills'"
        :name="'soft_skills'"
        :type="'text'"
        :label="'Soft Skills'"
        :placeholder="'e.g., Leadership, Communication, Problem Solving'"
        :layout="'2'"
        :required="false"
        :helper-text="'Highlight your interpersonal abilities'"
      />
      
      <codex-repeating-text-field
        v-model="profile.languages"
        :field="{ add_another_button_text: 'Add Language' }"
        :key="'languages'"
        :name="'languages'"
        :type="'text'"
        :label="'Languages'"
        :placeholder="'e.g., English (Native), Spanish (Fluent)'"
        :layout="'2'"
        :required="false"
        :helper-text="'Include proficiency level'"
      />
      
      <codex-repeating-text-field
        v-model="profile.certifications"
        :field="{ add_another_button_text: 'Add Certification' }"
        :key="'certifications'"
        :name="'certifications'"
        :type="'text'"
        :label="'Certifications'"
        :placeholder="'e.g., AWS Solutions Architect, PMP'"
        :layout="'1'"
        :required="false"
        :helper-text="'Professional certifications and qualifications'"
      />
      
      <codex-repeating-text-field
        v-model="profile.hobbies"
        :field="{ add_another_button_text: 'Add Hobby' }"
        :key="'hobbies'"
        :name="'hobbies'"
        :type="'text'"
        :label="'Hobbies & Interests'"
        :placeholder="'e.g., Photography, Hiking, Chess'"
        :layout="'3'"
        :required="false"
        :helper-text="'Personal interests and activities'"
      />
    </div>
    
    <div class="profile-summary" v-if="hasProfileData">
      <h4>Profile Summary</h4>
      
      <div class="summary-section" v-if="nonEmptySkills('technicalSkills').length > 0">
        <h5>Technical Skills ({{ nonEmptySkills('technicalSkills').length }})</h5>
        <div class="skill-tags">
          <span v-for="skill in nonEmptySkills('technicalSkills')" :key="skill" class="skill-tag technical">
            {{ skill }}
          </span>
        </div>
      </div>
      
      <div class="summary-section" v-if="nonEmptySkills('softSkills').length > 0">
        <h5>Soft Skills ({{ nonEmptySkills('softSkills').length }})</h5>
        <div class="skill-tags">
          <span v-for="skill in nonEmptySkills('softSkills')" :key="skill" class="skill-tag soft">
            {{ skill }}
          </span>
        </div>
      </div>
      
      <div class="summary-section" v-if="nonEmptySkills('languages').length > 0">
        <h5>Languages ({{ nonEmptySkills('languages').length }})</h5>
        <div class="skill-tags">
          <span v-for="lang in nonEmptySkills('languages')" :key="lang" class="skill-tag language">
            {{ lang }}
          </span>
        </div>
      </div>
      
      <div class="summary-section" v-if="nonEmptySkills('certifications').length > 0">
        <h5>Certifications ({{ nonEmptySkills('certifications').length }})</h5>
        <div class="certification-list">
          <div v-for="cert in nonEmptySkills('certifications')" :key="cert" class="certification-item">
            <i class="ri-award-line"></i>
            {{ cert }}
          </div>
        </div>
      </div>
    </div>
    
    <div class="profile-actions">
      <button @click="generateResume" :disabled="!hasMinimumSkills" class="generate-btn">
        Generate Resume
      </button>
      <button @click="saveProfile" class="save-btn">
        Save Profile
      </button>
      <button @click="previewProfile" class="preview-btn">
        Preview Profile
      </button>
    </div>
  </div>
</template>

<script setup>
const profile = reactive({
  technicalSkills: [''],
  softSkills: [''],
  languages: [''],
  certifications: [''],
  hobbies: ['']
})

const profileErrors = ref({
  technicalSkills: [],
  softSkills: []
})

const hasProfileData = computed(() => {
  return Object.values(profile).some(skillArray => 
    skillArray.some(skill => skill.trim().length > 0)
  )
})

const hasMinimumSkills = computed(() => {
  const techSkills = nonEmptySkills('technicalSkills').length
  const softSkills = nonEmptySkills('softSkills').length
  return techSkills >= 3 && softSkills >= 2
})

const nonEmptySkills = (skillType) => {
  return profile[skillType].filter(skill => skill.trim().length > 0)
}

const generateResume = async () => {
  try {
    const cleanProfile = Object.keys(profile).reduce((clean, key) => {
      clean[key] = nonEmptySkills(key)
      return clean
    }, {})
    
    const resumeData = await generateResumeFromProfile(cleanProfile)
    
    // Download or display generated resume
    toast.success('Resume generated successfully!')
  } catch (error) {
    toast.error('Failed to generate resume')
  }
}

const saveProfile = async () => {
  try {
    const cleanProfile = Object.keys(profile).reduce((clean, key) => {
      clean[key] = nonEmptySkills(key)
      return clean
    }, {})
    
    await saveUserProfile(cleanProfile)
    toast.success('Profile saved successfully!')
  } catch (error) {
    if (error.response?.data?.errors) {
      profileErrors.value = error.response.data.errors
    }
    toast.error('Failed to save profile')
  }
}

const previewProfile = () => {
  router.push('/profile/preview')
}
</script>
```

### Contact Information Manager
```vue
<template>
  <div class="contact-manager">
    <h3>Contact Information</h3>
    
    <div class="contact-sections">
      <codex-repeating-text-field
        v-model="contacts.emailAddresses"
        :field="{ add_another_button_text: 'Add Email Address' }"
        :key="'email_addresses'"
        :name="'email_addresses'"
        :type="'email'"
        :label="'Email Addresses'"
        :placeholder="'Enter email address'"
        :layout="'2'"
        :required="true"
        :errors="contactErrors.emailAddresses"
        :helper-text="'Primary email will be used for notifications'"
      />
      
      <codex-repeating-text-field
        v-model="contacts.phoneNumbers"
        :field="{ add_another_button_text: 'Add Phone Number' }"
        :key="'phone_numbers'"
        :name="'phone_numbers'"
        :type="'tel'"
        :label="'Phone Numbers'"
        :placeholder="'Enter phone number'"
        :layout="'2'"
        :required="false"
        :helper-text="'Include country code for international numbers'"
      />
      
      <codex-repeating-text-field
        v-model="contacts.socialProfiles"
        :field="{ add_another_button_text: 'Add Social Profile' }"
        :key="'social_profiles'"
        :name="'social_profiles'"
        :type="'url'"
        :label="'Social Media Profiles'"
        :placeholder="'https://linkedin.com/in/yourname'"
        :layout="'1'"
        :required="false"
        :helper-text="'LinkedIn, Twitter, GitHub, etc.'"
      />
      
      <codex-repeating-text-field
        v-model="contacts.websites"
        :field="{ add_another_button_text: 'Add Website' }"
        :key="'websites'"
        :name="'websites'"
        :type="'url'"
        :label="'Personal Websites'"
        :placeholder="'https://yourwebsite.com'"
        :layout="'2'"
        :required="false"
        :helper-text="'Portfolio, blog, or personal websites'"
      />
    </div>
    
    <div class="contact-preview" v-if="hasContactInfo">
      <h4>Contact Summary</h4>
      
      <div class="contact-section" v-if="validEmails.length > 0">
        <h5><i class="ri-mail-line"></i> Email Addresses</h5>
        <div class="contact-list">
          <div v-for="(email, index) in validEmails" :key="email" class="contact-item">
            <span class="contact-value">{{ email }}</span>
            <span v-if="index === 0" class="contact-badge primary">Primary</span>
          </div>
        </div>
      </div>
      
      <div class="contact-section" v-if="validPhones.length > 0">
        <h5><i class="ri-phone-line"></i> Phone Numbers</h5>
        <div class="contact-list">
          <div v-for="phone in validPhones" :key="phone" class="contact-item">
            <span class="contact-value">{{ phone }}</span>
          </div>
        </div>
      </div>
      
      <div class="contact-section" v-if="validSocial.length > 0">
        <h5><i class="ri-global-line"></i> Social Profiles</h5>
        <div class="contact-list">
          <div v-for="profile in validSocial" :key="profile" class="contact-item">
            <a :href="profile" target="_blank" class="contact-link">
              {{ getSocialPlatform(profile) }}
              <i class="ri-external-link-line"></i>
            </a>
          </div>
        </div>
      </div>
      
      <div class="contact-section" v-if="validWebsites.length > 0">
        <h5><i class="ri-window-line"></i> Websites</h5>
        <div class="contact-list">
          <div v-for="website in validWebsites" :key="website" class="contact-item">
            <a :href="website" target="_blank" class="contact-link">
              {{ getDomainName(website) }}
              <i class="ri-external-link-line"></i>
            </a>
          </div>
        </div>
      </div>
    </div>
    
    <div class="contact-actions">
      <button @click="validateAllContacts" class="validate-btn">
        Validate Contacts
      </button>
      <button @click="exportContacts" class="export-btn">
        Export vCard
      </button>
      <button @click="saveContacts" :disabled="!hasValidPrimaryEmail" class="save-btn">
        Save Contacts
      </button>
    </div>
  </div>
</template>

<script setup>
const contacts = reactive({
  emailAddresses: [''],
  phoneNumbers: [''],
  socialProfiles: [''],
  websites: ['']
})

const contactErrors = ref({
  emailAddresses: []
})

const hasContactInfo = computed(() => {
  return Object.values(contacts).some(contactArray => 
    contactArray.some(contact => contact.trim().length > 0)
  )
})

const validEmails = computed(() => {
  return contacts.emailAddresses.filter(email => {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    return email.trim() && emailRegex.test(email.trim())
  })
})

const validPhones = computed(() => {
  return contacts.phoneNumbers.filter(phone => phone.trim().length > 0)
})

const validSocial = computed(() => {
  return contacts.socialProfiles.filter(profile => {
    try {
      new URL(profile.trim())
      return true
    } catch {
      return false
    }
  })
})

const validWebsites = computed(() => {
  return contacts.websites.filter(website => {
    try {
      new URL(website.trim())
      return true
    } catch {
      return false
    }
  })
})

const hasValidPrimaryEmail = computed(() => {
  return validEmails.value.length > 0
})

const getSocialPlatform = (url) => {
  try {
    const domain = new URL(url).hostname.toLowerCase()
    if (domain.includes('linkedin')) return 'LinkedIn'
    if (domain.includes('twitter')) return 'Twitter'
    if (domain.includes('github')) return 'GitHub'
    if (domain.includes('instagram')) return 'Instagram'
    return domain.replace('www.', '')
  } catch {
    return url
  }
}

const getDomainName = (url) => {
  try {
    return new URL(url).hostname.replace('www.', '')
  } catch {
    return url
  }
}

const validateAllContacts = async () => {
  try {
    const allContacts = {
      emails: validEmails.value,
      phones: validPhones.value,
      social: validSocial.value,
      websites: validWebsites.value
    }
    
    const validation = await validateContactInformation(allContacts)
    
    if (validation.allValid) {
      toast.success('All contact information is valid')
    } else {
      toast.warning('Some contact information needs correction')
      contactErrors.value = validation.errors || {}
    }
  } catch (error) {
    toast.error('Failed to validate contacts')
  }
}

const exportContacts = () => {
  const vCardData = generateVCard({
    emails: validEmails.value,
    phones: validPhones.value,
    urls: [...validSocial.value, ...validWebsites.value]
  })
  
  const blob = new Blob([vCardData], { type: 'text/vcard' })
  const url = URL.createObjectURL(blob)
  
  const link = document.createElement('a')
  link.href = url
  link.download = 'contacts.vcf'
  link.click()
  
  URL.revokeObjectURL(url)
}

const saveContacts = async () => {
  try {
    const cleanContacts = {
      emailAddresses: validEmails.value,
      phoneNumbers: validPhones.value,
      socialProfiles: validSocial.value,
      websites: validWebsites.value
    }
    
    await saveContactInformation(cleanContacts)
    toast.success('Contact information saved successfully!')
  } catch (error) {
    if (error.response?.data?.errors) {
      contactErrors.value = error.response.data.errors
    }
    toast.error('Failed to save contact information')
  }
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input-container` | Main container for the field |
| `_c-checkbox-container` | Container for toggle checkbox |
| `_c-form-field--auto` | Auto-sized layout |
| `_c-form-field--quarter` | Quarter width layout (25%) |
| `_c-form-field--third` | Third width layout (33%) |
| `_c-form-field--half` | Half width layout (50%) |
| `_c-form-field--full` | Full width layout (100%) |
| `_c-label-container` | Container for label and hint |
| `_c-show-password` | Styling for delete button |
| `_c-hint` | Styling for add another button |

## Best Practices

### Data Management
- Always maintain at least one input field (even if empty)
- Filter out empty values before submitting to backend
- Validate each individual field appropriately for its type
- Handle array operations (add/remove) efficiently
- Preserve user input during dynamic field operations

### User Experience
- Provide clear "Add Another" button text for context
- Show field count or summary when appropriate
- Allow easy removal of individual fields
- Use appropriate input types (email, tel, url) for validation
- Provide helpful placeholder text for each field type

### Validation
- Validate individual fields according to their type
- Provide field-specific error messages
- Consider minimum/maximum field count requirements
- Handle partial validation (some fields valid, others not)
- Show validation status for each field individually

### Performance
- Use efficient array operations for add/remove
- Debounce validation for better performance
- Avoid unnecessary re-renders during field operations
- Consider virtual scrolling for very large lists
- Implement proper key management for dynamic fields

### Accessibility
- Ensure proper ARIA labels for dynamic fields
- Support keyboard navigation for add/remove operations
- Provide clear instructions for screen reader users
- Test with assistive technologies
- Use semantic HTML for better accessibility

### Mobile Optimization
- Ensure touch targets are appropriately sized
- Test add/remove functionality on mobile devices
- Consider swipe gestures for field removal
- Optimize layout for smaller screens
- Test with mobile keyboards for different input types

## Component Registration
```javascript
// Global registration
app.component('CodexRepeatingTextField', RepeatingTextField)

// Local registration  
import RepeatingTextField from '@/components/fields/RepeatingTextField.vue'

export default {
  components: {
    CodexRepeatingTextField: RepeatingTextField
  }
}
``` 