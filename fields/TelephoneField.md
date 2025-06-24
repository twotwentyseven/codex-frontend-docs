# TelephoneField Component

## Overview
The TelephoneField component provides an international telephone number input with comprehensive country selection, automatic formatting, and validation. It integrates with intl-tel-input library for country detection, phone number formatting, and validation with internationalization support. The component provides real-time validation feedback and supports multiple input formats while maintaining form integration.

## Basic Usage
```vue
<template>
  <div class="phone-form">
    <codex-telephone-field
      v-model="phoneNumber"
      :name="'phone'"
      :label="'Phone Number'"
      :required="true"
    />
  </div>
</template>

<script setup>
const phoneNumber = ref('')
</script>
```

## Key Features
- International telephone number input with country selection
- Automatic country detection and flag display
- Real-time phone number formatting and validation
- Support for national and international formats
- Integrated error handling with i18n messages
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
| `label` | `String` | `''` | Field label text |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the input |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |

### Phone Configuration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `type` | `String` | `'telephone'` | Input type for telephone |

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
| `changeNumber` | `number` | Emitted when formatted number changes |
| `changeValidity` | `isValid` | Emitted when validation state changes |
| `changeCountry` | `country` | Emitted when country selection changes |

## Model Value

The component uses `defineModel({ type: [String, Number] })`:

```vue
<template>
  <codex-telephone-field v-model="phoneNumber" />
</template>

<script setup>
const phoneNumber = ref('')
// phoneNumber automatically updates with formatted international number
</script>
```

## Validation

The component provides automatic validation with internationalized error messages:

- **Invalid Number**: General invalid format
- **Invalid Country Code**: Country code not recognized
- **Too Short**: Number too short for country
- **Too Long**: Number too long for country

## Examples

### Contact Information Form
```vue
<template>
  <div class="contact-form">
    <h3>Contact Information</h3>
    
    <div class="contact-fields">
      <codex-telephone-field
        v-model="contact.primaryPhone"
        :name="'primary_phone'"
        :label="'Primary Phone Number'"
        :layout="'2'"
        :required="true"
        :errors="contactErrors.primaryPhone"
        :helper-text="'Your main contact number'"
        :tooltip-text="'We\'ll use this for important updates'"
        :tooltip-icon="'ri-information-line'"
      />
      
      <codex-telephone-field
        v-model="contact.secondaryPhone"
        :name="'secondary_phone'"
        :label="'Secondary Phone (Optional)'"
        :layout="'2'"
        :required="false"
        :errors="contactErrors.secondaryPhone"
        :helper-text="'Backup contact number'"
      />
      
      <codex-telephone-field
        v-model="contact.workPhone"
        :name="'work_phone'"
        :label="'Work Phone'"
        :layout="'2'"
        :required="false"
        :helper-text="'Business contact number'"
      />
      
      <codex-telephone-field
        v-model="contact.emergencyContact"
        :name="'emergency_contact'"
        :label="'Emergency Contact'"
        :layout="'2'"
        :required="true"
        :errors="contactErrors.emergencyContact"
        :helper-text="'Someone we can reach in case of emergency'"
      />
    </div>
    
    <div class="phone-preferences">
      <h4>Communication Preferences</h4>
      <div class="preference-grid">
        <div class="preference-item">
          <input type="checkbox" v-model="preferences.smsNotifications" id="sms-notifications">
          <label for="sms-notifications">Allow SMS notifications</label>
          <span v-if="contact.primaryPhone" class="phone-display">{{ formatPhoneForDisplay(contact.primaryPhone) }}</span>
        </div>
        
        <div class="preference-item">
          <input type="checkbox" v-model="preferences.callReminders" id="call-reminders">
          <label for="call-reminders">Phone call reminders</label>
          <span v-if="contact.primaryPhone" class="phone-display">{{ formatPhoneForDisplay(contact.primaryPhone) }}</span>
        </div>
        
        <div class="preference-item">
          <input type="checkbox" v-model="preferences.workHoursCalls" id="work-hours">
          <label for="work-hours">Work hours calls only</label>
          <span class="time-range">9 AM - 6 PM local time</span>
        </div>
      </div>
    </div>
    
    <div class="validation-summary" v-if="phoneValidationSummary.length > 0">
      <h4>Phone Number Status</h4>
      <div v-for="item in phoneValidationSummary" :key="item.field" class="validation-item" :class="item.status">
        <i :class="item.icon"></i>
        <span><strong>{{ item.label }}:</strong> {{ item.message }}</span>
      </div>
    </div>
    
    <div class="contact-actions">
      <button @click="validateAllPhones" class="validate-btn">
        Validate All Numbers
      </button>
      <button @click="saveContact" :disabled="!isContactValid" class="save-btn">
        Save Contact Information
      </button>
    </div>
  </div>
</template>

<script setup>
const contact = reactive({
  primaryPhone: '',
  secondaryPhone: '',
  workPhone: '',
  emergencyContact: ''
})

const preferences = reactive({
  smsNotifications: false,
  callReminders: false,
  workHoursCalls: true
})

const contactErrors = ref({
  primaryPhone: [],
  secondaryPhone: [],
  emergencyContact: []
})

const phoneValidationSummary = computed(() => {
  const summary = []
  
  if (contact.primaryPhone) {
    summary.push({
      field: 'primaryPhone',
      label: 'Primary Phone',
      status: contactErrors.value.primaryPhone.length === 0 ? 'valid' : 'invalid',
      icon: contactErrors.value.primaryPhone.length === 0 ? 'ri-check-line' : 'ri-error-warning-line',
      message: contactErrors.value.primaryPhone.length === 0 ? 'Valid number' : contactErrors.value.primaryPhone[0]
    })
  }
  
  if (contact.emergencyContact) {
    summary.push({
      field: 'emergencyContact',
      label: 'Emergency Contact',
      status: contactErrors.value.emergencyContact.length === 0 ? 'valid' : 'invalid',
      icon: contactErrors.value.emergencyContact.length === 0 ? 'ri-check-line' : 'ri-error-warning-line',
      message: contactErrors.value.emergencyContact.length === 0 ? 'Valid number' : contactErrors.value.emergencyContact[0]
    })
  }
  
  return summary
})

const isContactValid = computed(() => {
  return contact.primaryPhone && 
         contact.emergencyContact &&
         contactErrors.value.primaryPhone.length === 0 &&
         contactErrors.value.emergencyContact.length === 0
})

const formatPhoneForDisplay = (phoneNumber) => {
  // Format phone number for display purposes
  if (!phoneNumber) return ''
  
  // This would typically use the same formatting library as the component
  return phoneNumber
}

const validateAllPhones = async () => {
  try {
    const validation = await validatePhoneNumbers({
      primaryPhone: contact.primaryPhone,
      secondaryPhone: contact.secondaryPhone,
      workPhone: contact.workPhone,
      emergencyContact: contact.emergencyContact
    })
    
    // Update error states based on validation results
    contactErrors.value = validation.errors || {}
    
    if (validation.allValid) {
      toast.success('All phone numbers are valid')
    } else {
      toast.warning('Some phone numbers need correction')
    }
  } catch (error) {
    toast.error('Failed to validate phone numbers')
  }
}

const saveContact = async () => {
  try {
    await saveContactInformation({
      ...contact,
      preferences
    })
    
    toast.success('Contact information saved successfully')
  } catch (error) {
    if (error.response?.data?.errors) {
      contactErrors.value = error.response.data.errors
    }
    toast.error('Failed to save contact information')
  }
}
</script>
```

### Multi-Location Business Registration
```vue
<template>
  <div class="business-registration">
    <h3>Business Contact Information</h3>
    
    <div class="business-locations">
      <div class="location-section">
        <h4>Primary Business Location</h4>
        
        <div class="location-form">
          <codex-telephone-field
            v-model="business.mainOffice.phone"
            :name="'main_office_phone'"
            :label="'Main Office Phone'"
            :layout="'2'"
            :required="true"
            :errors="businessErrors.mainOffice?.phone"
            :helper-text="'Primary business line'"
          />
          
          <codex-telephone-field
            v-model="business.mainOffice.fax"
            :name="'main_office_fax'"
            :label="'Fax Number'"
            :layout="'2'"
            :required="false"
            :helper-text="'Optional fax line'"
          />
          
          <codex-telephone-field
            v-model="business.mainOffice.mobile"
            :name="'main_office_mobile'"
            :label="'Mobile Contact'"
            :layout="'2'"
            :required="false"
            :helper-text="'Mobile business line'"
          />
          
          <codex-telephone-field
            v-model="business.mainOffice.emergency"
            :name="'main_office_emergency'"
            :label="'After Hours Contact'"
            :layout="'2'"
            :required="true"
            :errors="businessErrors.mainOffice?.emergency"
            :helper-text="'24/7 emergency contact'"
          />
        </div>
      </div>
      
      <div class="location-section" v-for="(location, index) in business.additionalLocations" :key="index">
        <h4>{{ location.name || `Location ${index + 1}` }}</h4>
        
        <div class="location-form">
          <codex-telephone-field
            v-model="location.phone"
            :name="`location_${index}_phone`"
            :label="'Location Phone'"
            :layout="'2'"
            :required="true"
            :errors="businessErrors.additionalLocations?.[index]?.phone"
            :helper-text="`Phone for ${location.name || 'this location'}`"
          />
          
          <codex-telephone-field
            v-model="location.mobile"
            :name="`location_${index}_mobile`"
            :label="'Location Mobile'"
            :layout="'2'"
            :required="false"
            :helper-text="'Optional mobile contact'"
          />
          
          <button @click="removeLocation(index)" class="remove-location-btn">
            Remove Location
          </button>
        </div>
      </div>
      
      <button @click="addLocation" class="add-location-btn">
        Add Another Location
      </button>
    </div>
    
    <div class="department-contacts">
      <h4>Department Contacts</h4>
      
      <div class="department-grid">
        <codex-telephone-field
          v-model="business.departments.sales"
          :name="'sales_phone'"
          :label="'Sales Department'"
          :layout="'3'"
          :required="false"
          :helper-text="'Sales inquiries'"
        />
        
        <codex-telephone-field
          v-model="business.departments.support"
          :name="'support_phone'"
          :label="'Customer Support'"
          :layout="'3'"
          :required="false"
          :helper-text="'Customer service'"
        />
        
        <codex-telephone-field
          v-model="business.departments.technical"
          :name="'technical_phone'"
          :label="'Technical Support'"
          :layout="'3'"
          :required="false"
          :helper-text="'Technical assistance'"
        />
        
        <codex-telephone-field
          v-model="business.departments.billing"
          :name="'billing_phone'"
          :label="'Billing Department'"
          :layout="'3'"
          :required="false"
          :helper-text="'Billing and payments'"
        />
        
        <codex-telephone-field
          v-model="business.departments.hr"
          :name="'hr_phone'"
          :label="'Human Resources'"
          :layout="'3'"
          :required="false"
          :helper-text="'HR inquiries'"
        />
        
        <codex-telephone-field
          v-model="business.departments.management"
          :name="'management_phone'"
          :label="'Management'"
          :layout="'3'"
          :required="false"
          :helper-text="'Executive contact'"
        />
      </div>
    </div>
    
    <div class="phone-directory" v-if="hasValidPhoneNumbers">
      <h4>Business Phone Directory</h4>
      <div class="directory-list">
        <div class="directory-section">
          <h5>Main Office</h5>
          <div v-if="business.mainOffice.phone" class="directory-item">
            <span class="label">Main Line:</span>
            <span class="number">{{ business.mainOffice.phone }}</span>
          </div>
          <div v-if="business.mainOffice.mobile" class="directory-item">
            <span class="label">Mobile:</span>
            <span class="number">{{ business.mainOffice.mobile }}</span>
          </div>
          <div v-if="business.mainOffice.emergency" class="directory-item">
            <span class="label">After Hours:</span>
            <span class="number">{{ business.mainOffice.emergency }}</span>
          </div>
        </div>
        
        <div v-if="validDepartmentNumbers.length > 0" class="directory-section">
          <h5>Departments</h5>
          <div v-for="dept in validDepartmentNumbers" :key="dept.name" class="directory-item">
            <span class="label">{{ dept.label }}:</span>
            <span class="number">{{ dept.number }}</span>
          </div>
        </div>
      </div>
    </div>
    
    <div class="registration-actions">
      <button @click="validateAllBusinessPhones" class="validate-btn">
        Validate All Numbers
      </button>
      <button @click="generatePhoneDirectory" class="directory-btn">
        Generate Directory
      </button>
      <button @click="saveBusinessRegistration" :disabled="!isBusinessRegistrationValid" class="save-btn">
        Complete Registration
      </button>
    </div>
  </div>
</template>

<script setup>
const business = reactive({
  mainOffice: {
    phone: '',
    fax: '',
    mobile: '',
    emergency: ''
  },
  additionalLocations: [],
  departments: {
    sales: '',
    support: '',
    technical: '',
    billing: '',
    hr: '',
    management: ''
  }
})

const businessErrors = ref({
  mainOffice: {},
  additionalLocations: [],
  departments: {}
})

const hasValidPhoneNumbers = computed(() => {
  return business.mainOffice.phone || 
         Object.values(business.departments).some(phone => phone) ||
         business.additionalLocations.some(loc => loc.phone)
})

const validDepartmentNumbers = computed(() => {
  const departments = [
    { name: 'sales', label: 'Sales', number: business.departments.sales },
    { name: 'support', label: 'Customer Support', number: business.departments.support },
    { name: 'technical', label: 'Technical Support', number: business.departments.technical },
    { name: 'billing', label: 'Billing', number: business.departments.billing },
    { name: 'hr', label: 'Human Resources', number: business.departments.hr },
    { name: 'management', label: 'Management', number: business.departments.management }
  ]
  
  return departments.filter(dept => dept.number)
})

const isBusinessRegistrationValid = computed(() => {
  return business.mainOffice.phone && 
         business.mainOffice.emergency &&
         !Object.values(businessErrors.value.mainOffice).some(errors => errors.length > 0)
})

const addLocation = () => {
  business.additionalLocations.push({
    name: '',
    phone: '',
    mobile: ''
  })
}

const removeLocation = (index) => {
  business.additionalLocations.splice(index, 1)
  businessErrors.value.additionalLocations.splice(index, 1)
}

const validateAllBusinessPhones = async () => {
  try {
    const allPhones = {
      mainOffice: business.mainOffice,
      additionalLocations: business.additionalLocations,
      departments: business.departments
    }
    
    const validation = await validateBusinessPhoneNumbers(allPhones)
    businessErrors.value = validation.errors || {}
    
    if (validation.allValid) {
      toast.success('All business phone numbers are valid')
    } else {
      toast.warning('Some phone numbers need correction')
    }
  } catch (error) {
    toast.error('Failed to validate phone numbers')
  }
}

const generatePhoneDirectory = () => {
  // Generate a formatted phone directory
  const directory = {
    businessName: 'Your Business Name',
    mainOffice: business.mainOffice,
    departments: validDepartmentNumbers.value,
    additionalLocations: business.additionalLocations.filter(loc => loc.phone)
  }
  
  // This could download a file, show in modal, etc.
  console.log('Generated Phone Directory:', directory)
}

const saveBusinessRegistration = async () => {
  try {
    await saveBusinessContactInformation(business)
    toast.success('Business registration completed successfully')
    router.push('/business/dashboard')
  } catch (error) {
    if (error.response?.data?.errors) {
      businessErrors.value = error.response.data.errors
    }
    toast.error('Failed to complete business registration')
  }
}
</script>
```

## Internationalization

The component uses the following translation keys for validation error messages:

### Validation Error Keys
| Key | Usage |
|-----|-------|
| `errors.invalid_number` | General invalid number format error |
| `errors.invalid_country_code` | Invalid country code error |
| `errors.too_short` | Number too short for the selected country |
| `errors.too_long` | Number too long for the selected country |

### Error Mapping
The component maps different validation error codes to appropriate translation keys:
```javascript
const errorMap = [
    t('errors.invalid_number'),      // Error code 0
    t('errors.invalid_country_code'), // Error code 1
    t('errors.too_short'),           // Error code 2
    t('errors.too_long'),            // Error code 3
    t('errors.invalid_number'),      // Error code 4
];
```

### Translation Usage Examples
```vue
<!-- Error message display -->
<div v-if="errorMessage" class="field-error">
  {{ errorMessage }} <!-- Contains t('errors.invalid_number') etc. -->
</div>

<!-- Field with custom error translations -->
<codex-telephone-field
  :label="$t('customer.phone_number')"
  :placeholder="$t('customer.phone_placeholder')"
  v-model="phoneNumber"
  :errors="fieldErrors.phone"
/>
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
| `_c-placeholder` | Placeholder state styling |
| `_c-input` | Telephone input styling |
| `_c-error` | Error state styling |

## Best Practices

### International Support
- Use appropriate country selection defaults based on user location
- Support both national and international number formats
- Provide clear visual feedback for country selection
- Consider regional preferences for number formatting

### Validation
- Implement real-time validation for immediate feedback
- Use localized error messages for better user understanding
- Validate numbers according to country-specific rules
- Provide specific error messages for different validation failures

### User Experience
- Show country flags for visual recognition
- Format numbers automatically as users type
- Allow copy/paste of numbers in various formats
- Provide clear examples of expected number formats

### Accessibility
- Ensure proper labeling for screen readers
- Support keyboard navigation for country selection
- Provide text alternatives for flag images
- Test with assistive technologies

### Data Handling
- Store numbers in a consistent international format
- Consider privacy implications of phone number storage
- Validate numbers on both client and server side
- Handle different number formats gracefully

### Mobile Optimization
- Trigger numeric keyboard on mobile devices
- Ensure touch targets are appropriately sized
- Test on various mobile devices and orientations
- Consider mobile-specific input patterns

### Performance
- Load country data efficiently
- Cache validation results when appropriate
- Debounce validation for better performance
- Minimize network requests for validation

## Component Registration
```javascript
// Global registration
app.component('CodexTelephoneField', TelephoneField)

// Local registration  
import TelephoneField from '@/components/fields/TelephoneField.vue'

export default {
  components: {
    CodexTelephoneField: TelephoneField
  }
}
``` 