# SelectField Component

## Overview
The SelectField component provides a comprehensive dropdown select input with support for both simple arrays and grouped options. It features dynamic option rendering, placeholder handling, default value selection, and comprehensive form validation integration. The component supports grouped optgroups, custom value/display mappings, and accessibility features while maintaining seamless integration with the form system.

## Basic Usage
```vue
<template>
  <div class="select-form">
    <codex-select-field
      v-model="selectedCountry"
      :name="'country'"
      :label="'Country'"
      :options="countryOptions"
      :placeholder="'Select a country'"
      :required="true"
    />
  </div>
</template>

<script setup>
const selectedCountry = ref('')

const countryOptions = [
  { value: 'us', displayValue: 'United States' },
  { value: 'uk', displayValue: 'United Kingdom' },
  { value: 'ca', displayValue: 'Canada' },
  { value: 'au', displayValue: 'Australia' }
]
</script>
```

## Key Features
- Simple array options and grouped optgroup support
- Custom value and display name mapping
- Placeholder handling with dynamic labeling
- Default to first option functionality
- Multiple selection support with constraints
- Grouped option limits and validation
- Flexible layout configurations
- Accessibility features with proper labeling
- Testing support with Dusk attributes

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `name` | `String` | `required` | Field name for form submission |

### Options Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `options` | `Array\|Object` | `[]` | Options array or grouped options object |
| `optionValue` | `String` | `'value'` | Property name for option values |
| `optionName` | `String` | `'displayValue'` | Property name for option display text |
| `defaultToFirstOption` | `Boolean` | `false` | Automatically select first option |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
| `placeholder` | `String` | `''` | Placeholder text for empty selection |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the select |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |

### Multiple Selection Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `multiple` | `Boolean` | `false` | Enable multiple selection |
| `limit` | `Number` | `null` | Maximum number of selections |
| `limitToOneOptionGroup` | `Boolean` | `false` | Limit selections to one optgroup |

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

## Model Values

The component supports multiple model bindings:

```vue
<template>
  <!-- Single selection -->
  <codex-select-field v-model="selectedValue" />
  
  <!-- Multiple selection -->
  <codex-select-field 
    v-model="selectedValues"
    v-model:selectedGroup="selectedGroup"
    :multiple="true"
  />
</template>

<script setup>
const selectedValue = ref('')
const selectedValues = ref([])
const selectedGroup = ref('')
</script>
```

## Option Formats

### Simple Array Options
```javascript
const simpleOptions = [
  { value: 'us', displayValue: 'United States' },
  { value: 'uk', displayValue: 'United Kingdom' },
  { value: 'ca', displayValue: 'Canada' }
]
```

### Grouped Options Object
```javascript
const groupedOptions = {
  northAmerica: {
    label: 'North America',
    options: [
      { value: 'us', displayValue: 'United States' },
      { value: 'ca', displayValue: 'Canada' },
      { value: 'mx', displayValue: 'Mexico' }
    ]
  },
  europe: {
    label: 'Europe',
    options: [
      { value: 'uk', displayValue: 'United Kingdom' },
      { value: 'fr', displayValue: 'France' },
      { value: 'de', displayValue: 'Germany' }
    ]
  }
}
```

### Options with Text Objects
```javascript
const richTextOptions = [
  { 
    value: 'premium', 
    displayValue: { 
      text: 'Premium Plan - $29.99/month' 
    } 
  },
  { 
    value: 'basic', 
    displayValue: 'Basic Plan - $9.99/month' 
  }
]
```

## Examples

### Country Selection Form
```vue
<template>
  <div class="location-form">
    <h3>Location Information</h3>
    
    <div class="form-grid">
      <codex-select-field
        v-model="location.region"
        :name="'region'"
        :label="'Region'"
        :options="regionOptions"
        :placeholder="'Choose your region'"
        :layout="'2'"
        :required="true"
        :defaultToFirstOption="false"
        @change="handleRegionChange"
      />
      
      <codex-select-field
        v-model="location.country"
        :name="'country'"
        :label="'Country'"
        :options="countryOptions"
        :placeholder="'Select country'"
        :layout="'2'"
        :required="true"
        :disabled="!location.region"
        :errors="locationErrors.country"
      />
      
      <codex-select-field
        v-model="location.state"
        :name="'state'"
        :label="'State/Province'"
        :options="stateOptions"
        :placeholder="'Select state or province'"
        :layout="'2'"
        :required="false"
        :helper-text="'Optional for most countries'"
      />
      
      <codex-select-field
        v-model="location.timezone"
        :name="'timezone'"
        :label="'Time Zone'"
        :options="timezoneOptions"
        :placeholder="'Select timezone'"
        :layout="'2'"
        :required="true"
        :defaultToFirstOption="true"
        :helper-text="'Used for scheduling and notifications'"
      />
    </div>
    
    <div class="location-summary" v-if="isLocationComplete">
      <h4>Selected Location</h4>
      <p>{{ locationSummary }}</p>
    </div>
  </div>
</template>

<script setup>
const location = reactive({
  region: '',
  country: '',
  state: '',
  timezone: ''
})

const locationErrors = ref({
  country: []
})

const regionOptions = [
  { value: 'north-america', displayValue: 'North America' },
  { value: 'europe', displayValue: 'Europe' },
  { value: 'asia-pacific', displayValue: 'Asia Pacific' },
  { value: 'latin-america', displayValue: 'Latin America' }
]

const countryOptions = computed(() => {
  const countryMap = {
    'north-america': [
      { value: 'us', displayValue: 'United States' },
      { value: 'ca', displayValue: 'Canada' },
      { value: 'mx', displayValue: 'Mexico' }
    ],
    'europe': [
      { value: 'uk', displayValue: 'United Kingdom' },
      { value: 'de', displayValue: 'Germany' },
      { value: 'fr', displayValue: 'France' },
      { value: 'es', displayValue: 'Spain' }
    ],
    'asia-pacific': [
      { value: 'au', displayValue: 'Australia' },
      { value: 'jp', displayValue: 'Japan' },
      { value: 'sg', displayValue: 'Singapore' }
    ],
    'latin-america': [
      { value: 'br', displayValue: 'Brazil' },
      { value: 'ar', displayValue: 'Argentina' },
      { value: 'cl', displayValue: 'Chile' }
    ]
  }
  
  return location.region ? countryMap[location.region] || [] : []
})

const stateOptions = computed(() => {
  if (location.country === 'us') {
    return [
      { value: 'ca', displayValue: 'California' },
      { value: 'ny', displayValue: 'New York' },
      { value: 'tx', displayValue: 'Texas' },
      { value: 'fl', displayValue: 'Florida' }
    ]
  } else if (location.country === 'ca') {
    return [
      { value: 'on', displayValue: 'Ontario' },
      { value: 'bc', displayValue: 'British Columbia' },
      { value: 'qc', displayValue: 'Quebec' }
    ]
  }
  return []
})

const timezoneOptions = computed(() => {
  const timezoneMap = {
    'us': [
      { value: 'America/New_York', displayValue: 'Eastern Time (ET)' },
      { value: 'America/Chicago', displayValue: 'Central Time (CT)' },
      { value: 'America/Denver', displayValue: 'Mountain Time (MT)' },
      { value: 'America/Los_Angeles', displayValue: 'Pacific Time (PT)' }
    ],
    'uk': [
      { value: 'Europe/London', displayValue: 'Greenwich Mean Time (GMT)' }
    ],
    'de': [
      { value: 'Europe/Berlin', displayValue: 'Central European Time (CET)' }
    ]
  }
  
  return location.country ? timezoneMap[location.country] || [] : []
})

const isLocationComplete = computed(() => {
  return location.region && location.country && location.timezone
})

const locationSummary = computed(() => {
  const regionName = regionOptions.find(r => r.value === location.region)?.displayValue
  const countryName = countryOptions.value.find(c => c.value === location.country)?.displayValue
  const timezoneName = timezoneOptions.value.find(t => t.value === location.timezone)?.displayValue
  
  return `${countryName}, ${regionName} - ${timezoneName}`
})

const handleRegionChange = () => {
  // Reset dependent fields when region changes
  location.country = ''
  location.state = ''
  location.timezone = ''
  locationErrors.value.country = []
}

watch(() => location.country, (newCountry) => {
  // Reset state when country changes
  location.state = ''
  
  // Auto-select timezone if only one option
  const timezones = timezoneOptions.value
  if (timezones.length === 1) {
    location.timezone = timezones[0].value
  } else {
    location.timezone = ''
  }
})
</script>
```

### Product Configuration Selector
```vue
<template>
  <div class="product-configurator">
    <h3>Configure Your Product</h3>
    
    <div class="configuration-grid">
      <codex-select-field
        v-model="config.category"
        :name="'category'"
        :label="'Product Category'"
        :options="categoryOptions"
        :placeholder="'Choose category'"
        :layout="'2'"
        :required="true"
        :defaultToFirstOption="false"
      />
      
      <codex-select-field
        v-model="config.size"
        :name="'size'"
        :label="'Size'"
        :options="sizeOptions"
        :placeholder="'Select size'"
        :layout="'4'"
        :required="true"
        :helper-text="sizeHelperText"
      />
      
      <codex-select-field
        v-model="config.color"
        :name="'color'"
        :label="'Color'"
        :options="colorOptions"
        :placeholder="'Choose color'"
        :layout="'4'"
        :required="true"
      />
      
      <codex-select-field
        v-model="config.features"
        :name="'features'"
        :label="'Additional Features'"
        :options="featureOptions"
        :placeholder="'Select features'"
        :multiple="true"
        :limit="3"
        :layout="'2'"
        :required="false"
        :helper-text="'Choose up to 3 additional features'"
      />
    </div>
    
    <div class="configuration-preview" v-if="isConfigurationValid">
      <h4>Configuration Summary</h4>
      <div class="config-details">
        <div class="config-item">
          <strong>Category:</strong> {{ getCategoryDisplayName() }}
        </div>
        <div class="config-item">
          <strong>Size:</strong> {{ getSizeDisplayName() }}
        </div>
        <div class="config-item">
          <strong>Color:</strong> {{ getColorDisplayName() }}
        </div>
        <div v-if="config.features.length > 0" class="config-item">
          <strong>Features:</strong> {{ getFeatureDisplayNames() }}
        </div>
        <div class="config-price">
          <strong>Total Price: {{ calculateTotalPrice() }}</strong>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const config = reactive({
  category: '',
  size: '',
  color: '',
  features: []
})

const categoryOptions = [
  { value: 'electronics', displayValue: 'Electronics' },
  { value: 'clothing', displayValue: 'Clothing' },
  { value: 'home', displayValue: 'Home & Garden' },
  { value: 'sports', displayValue: 'Sports & Outdoors' }
]

const sizeOptions = computed(() => {
  const sizeMap = {
    'electronics': [
      { value: 'small', displayValue: 'Compact' },
      { value: 'medium', displayValue: 'Standard' },
      { value: 'large', displayValue: 'Premium' }
    ],
    'clothing': [
      { value: 'xs', displayValue: 'Extra Small' },
      { value: 's', displayValue: 'Small' },
      { value: 'm', displayValue: 'Medium' },
      { value: 'l', displayValue: 'Large' },
      { value: 'xl', displayValue: 'Extra Large' }
    ],
    'home': [
      { value: 'small', displayValue: 'Apartment Size' },
      { value: 'medium', displayValue: 'Standard Home' },
      { value: 'large', displayValue: 'Large Home' }
    ],
    'sports': [
      { value: 'youth', displayValue: 'Youth' },
      { value: 'adult-s', displayValue: 'Adult Small' },
      { value: 'adult-m', displayValue: 'Adult Medium' },
      { value: 'adult-l', displayValue: 'Adult Large' }
    ]
  }
  
  return config.category ? sizeMap[config.category] || [] : []
})

const colorOptions = computed(() => {
  return [
    { value: 'black', displayValue: 'Black' },
    { value: 'white', displayValue: 'White' },
    { value: 'blue', displayValue: 'Blue' },
    { value: 'red', displayValue: 'Red' },
    { value: 'green', displayValue: 'Green' }
  ]
})

const featureOptions = computed(() => {
  const featureMap = {
    'electronics': [
      { value: 'warranty', displayValue: 'Extended Warranty (+$50)' },
      { value: 'setup', displayValue: 'Professional Setup (+$100)' },
      { value: 'support', displayValue: 'Priority Support (+$25)' }
    ],
    'clothing': [
      { value: 'monogram', displayValue: 'Custom Monogram (+$15)' },
      { value: 'gift-wrap', displayValue: 'Gift Wrapping (+$10)' },
      { value: 'rush', displayValue: 'Rush Delivery (+$20)' }
    ],
    'home': [
      { value: 'installation', displayValue: 'Professional Installation (+$200)' },
      { value: 'warranty', displayValue: 'Extended Warranty (+$75)' },
      { value: 'maintenance', displayValue: 'Maintenance Plan (+$150)' }
    ],
    'sports': [
      { value: 'customization', displayValue: 'Custom Colors (+$30)' },
      { value: 'engraving', displayValue: 'Engraving (+$25)' },
      { value: 'case', displayValue: 'Protective Case (+$40)' }
    ]
  }
  
  return config.category ? featureMap[config.category] || [] : []
})

const sizeHelperText = computed(() => {
  if (!config.category) return 'Select a category first'
  if (config.category === 'clothing') return 'See size chart for measurements'
  return 'Available sizes for selected category'
})

const isConfigurationValid = computed(() => {
  return config.category && config.size && config.color
})

const getCategoryDisplayName = () => {
  return categoryOptions.find(c => c.value === config.category)?.displayValue || ''
}

const getSizeDisplayName = () => {
  return sizeOptions.value.find(s => s.value === config.size)?.displayValue || ''
}

const getColorDisplayName = () => {
  return colorOptions.value.find(c => c.value === config.color)?.displayValue || ''
}

const getFeatureDisplayNames = () => {
  return config.features.map(featureValue => 
    featureOptions.value.find(f => f.value === featureValue)?.displayValue || ''
  ).join(', ')
}

const calculateTotalPrice = () => {
  let basePrice = 100 // Base price
  
  // Size price modifiers
  const sizePrices = { small: 0, medium: 50, large: 100, xs: 0, s: 0, m: 10, l: 20, xl: 30 }
  basePrice += sizePrices[config.size] || 0
  
  // Feature prices
  const featurePrices = {
    warranty: 50, setup: 100, support: 25, monogram: 15, 
    'gift-wrap': 10, rush: 20, installation: 200, 
    maintenance: 150, customization: 30, engraving: 25, case: 40
  }
  
  config.features.forEach(feature => {
    basePrice += featurePrices[feature] || 0
  })
  
  return `$${basePrice.toFixed(2)}`
}

// Reset dependent fields when category changes
watch(() => config.category, () => {
  config.size = ''
  config.color = ''
  config.features = []
})
</script>
```

### Team Assignment Selector
```vue
<template>
  <div class="team-assignment">
    <h3>Assign Team Members</h3>
    
    <div class="assignment-form">
      <codex-select-field
        v-model="assignment.department"
        :name="'department'"
        :label="'Department'"
        :options="departmentOptions"
        :placeholder="'Select department'"
        :layout="'2'"
        :required="true"
      />
      
      <codex-select-field
        v-model="assignment.team"
        :name="'team'"
        :label="'Team'"
        :options="teamOptions"
        :placeholder="'Choose team'"
        :layout="'2'"
        :required="true"
        :disabled="!assignment.department"
      />
      
      <codex-select-field
        v-model="assignment.members"
        v-model:selectedGroup="assignment.selectedGroup"
        :name="'members'"
        :label="'Team Members'"
        :options="memberOptions"
        :placeholder="'Select team members'"
        :multiple="true"
        :limit="5"
        :limitToOneOptionGroup="true"
        :layout="'1'"
        :required="true"
        :helper-text="'Select up to 5 members from the same skill group'"
        :errors="assignmentErrors.members"
      />
      
      <codex-select-field
        v-model="assignment.lead"
        :name="'team_lead'"
        :label="'Team Lead'"
        :options="leadOptions"
        :placeholder="'Choose team lead'"
        :layout="'2'"
        :required="true"
        :helper-text="'Must be one of the selected members'"
      />
      
      <codex-select-field
        v-model="assignment.project"
        :name="'project'"
        :label="'Project Assignment'"
        :options="projectOptions"
        :placeholder="'Assign to project'"
        :layout="'2'"
        :required="false"
        :defaultToFirstOption="false"
      />
    </div>
    
    <div class="assignment-summary" v-if="isAssignmentComplete">
      <h4>Assignment Summary</h4>
      <div class="summary-details">
        <p><strong>Department:</strong> {{ getDepartmentName() }}</p>
        <p><strong>Team:</strong> {{ getTeamName() }}</p>
        <p><strong>Members:</strong> {{ assignment.members.length }} selected</p>
        <p><strong>Team Lead:</strong> {{ getTeamLeadName() }}</p>
        <p v-if="assignment.project"><strong>Project:</strong> {{ getProjectName() }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
const assignment = reactive({
  department: '',
  team: '',
  members: [],
  selectedGroup: '',
  lead: '',
  project: ''
})

const assignmentErrors = ref({
  members: []
})

const departmentOptions = [
  { value: 'engineering', displayValue: 'Engineering' },
  { value: 'design', displayValue: 'Design' },
  { value: 'marketing', displayValue: 'Marketing' },
  { value: 'sales', displayValue: 'Sales' }
]

const teamOptions = computed(() => {
  const teamMap = {
    'engineering': [
      { value: 'frontend', displayValue: 'Frontend Development' },
      { value: 'backend', displayValue: 'Backend Development' },
      { value: 'devops', displayValue: 'DevOps' },
      { value: 'qa', displayValue: 'Quality Assurance' }
    ],
    'design': [
      { value: 'ux', displayValue: 'UX Design' },
      { value: 'visual', displayValue: 'Visual Design' },
      { value: 'product', displayValue: 'Product Design' }
    ],
    'marketing': [
      { value: 'digital', displayValue: 'Digital Marketing' },
      { value: 'content', displayValue: 'Content Marketing' },
      { value: 'social', displayValue: 'Social Media' }
    ],
    'sales': [
      { value: 'inside', displayValue: 'Inside Sales' },
      { value: 'enterprise', displayValue: 'Enterprise Sales' },
      { value: 'customer-success', displayValue: 'Customer Success' }
    ]
  }
  
  return assignment.department ? teamMap[assignment.department] || [] : []
})

const memberOptions = computed(() => {
  if (!assignment.team) return {}
  
  // Grouped by skill level
  return {
    senior: {
      label: 'Senior Level',
      options: [
        { value: 'john-doe', displayValue: 'John Doe (Senior)' },
        { value: 'jane-smith', displayValue: 'Jane Smith (Senior)' },
        { value: 'mike-johnson', displayValue: 'Mike Johnson (Senior)' }
      ]
    },
    mid: {
      label: 'Mid Level',
      options: [
        { value: 'sarah-wilson', displayValue: 'Sarah Wilson (Mid)' },
        { value: 'david-brown', displayValue: 'David Brown (Mid)' },
        { value: 'lisa-garcia', displayValue: 'Lisa Garcia (Mid)' }
      ]
    },
    junior: {
      label: 'Junior Level',
      options: [
        { value: 'alex-miller', displayValue: 'Alex Miller (Junior)' },
        { value: 'emma-davis', displayValue: 'Emma Davis (Junior)' },
        { value: 'ryan-taylor', displayValue: 'Ryan Taylor (Junior)' }
      ]
    }
  }
})

const leadOptions = computed(() => {
  // Only members who are selected can be team lead
  if (!assignment.members.length) return []
  
  const allMembers = Object.values(memberOptions.value)
    .flatMap(group => group.options)
  
  return assignment.members.map(memberId => 
    allMembers.find(member => member.value === memberId)
  ).filter(Boolean)
})

const projectOptions = [
  { value: 'project-a', displayValue: 'Project Alpha - Web Platform' },
  { value: 'project-b', displayValue: 'Project Beta - Mobile App' },
  { value: 'project-c', displayValue: 'Project Gamma - API Integration' },
  { value: 'project-d', displayValue: 'Project Delta - Data Analytics' }
]

const isAssignmentComplete = computed(() => {
  return assignment.department && 
         assignment.team && 
         assignment.members.length > 0 && 
         assignment.lead
})

const getDepartmentName = () => {
  return departmentOptions.find(d => d.value === assignment.department)?.displayValue || ''
}

const getTeamName = () => {
  return teamOptions.value.find(t => t.value === assignment.team)?.displayValue || ''
}

const getTeamLeadName = () => {
  return leadOptions.value.find(l => l.value === assignment.lead)?.displayValue || ''
}

const getProjectName = () => {
  return projectOptions.find(p => p.value === assignment.project)?.displayValue || ''
}

// Reset dependent fields when department changes
watch(() => assignment.department, () => {
  assignment.team = ''
  assignment.members = []
  assignment.lead = ''
  assignmentErrors.value.members = []
})

// Reset members when team changes
watch(() => assignment.team, () => {
  assignment.members = []
  assignment.lead = ''
  assignment.selectedGroup = ''
  assignmentErrors.value.members = []
})

// Reset lead when members change
watch(() => assignment.members, () => {
  if (!assignment.members.includes(assignment.lead)) {
    assignment.lead = ''
  }
  
  // Validate member selection
  if (assignment.members.length > 5) {
    assignmentErrors.value.members = ['Maximum 5 members allowed']
  } else {
    assignmentErrors.value.members = []
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
| `_c-placeholder` | Placeholder state styling |

## Best Practices

### Option Design
- Use clear, descriptive display values for options
- Organize options logically (alphabetically, by frequency, etc.)
- Provide meaningful grouping with optgroups when appropriate
- Consider the cognitive load of too many options

### Data Structure
- Use consistent value/displayValue mapping across options
- Keep option values simple and URL-safe
- Use grouped options for related but distinct categories
- Consider localization needs for display values

### User Experience
- Provide helpful placeholder text that guides selection
- Use defaultToFirstOption judiciously (avoid for critical choices)
- Implement proper cascading when options depend on other selections
- Show loading states for dynamic option loading

### Multiple Selection
- Set reasonable limits for multiple selections
- Use limitToOneOptionGroup when selections should be related
- Provide clear feedback about selection constraints
- Consider alternative UI patterns for many selections

### Accessibility
- Ensure proper label association with select elements
- Use optgroup labels that provide meaningful categorization
- Provide clear error messages for invalid selections
- Test with screen readers and keyboard navigation

### Performance
- Implement virtual scrolling for very large option lists
- Use computed properties for dependent option calculations
- Debounce option filtering for search functionality
- Cache option data when possible

### Validation
- Validate selections on change for immediate feedback
- Handle both client-side and server-side validation
- Provide specific error messages for different validation failures
- Consider progressive validation for complex dependent selections

## Internationalization

The component uses the following translation keys:

### Core Field Labels and Messages
| Key | Usage |
|-----|-------|
| `field.label` | Default field label when label prop uses translation key |
| `field.placeholder` | Default placeholder when placeholder_translation prop is used |
| `field.required` | Required field indicator |
| `field.optional` | Optional field indicator |

### Selection and Options
| Key | Usage |
|-----|-------|
| `select.choose_option` | Default placeholder for single selection |
| `select.choose_options` | Default placeholder for multiple selection |
| `select.no_options_available` | Message when no options are provided |
| `select.loading_options` | Loading state for dynamic options |
| `select.search_options` | Search functionality placeholder |

### Option Groups and Organization
| Key | Usage |
|-----|-------|
| `select.group_label` | Default group label when using optgroups |
| `select.ungrouped_options` | Label for options without group |
| `select.all_groups` | Label for selecting from all groups |

### Multiple Selection Features
| Key | Usage |
|-----|-------|
| `select.selected_count` | Selected items count display (with parameter) |
| `select.max_selections_reached` | Maximum selection limit message |
| `select.select_all` | Select all options action |
| `select.clear_all` | Clear all selections action |
| `select.remove_selection` | Remove individual selection |

### Validation and Error Messages
| Key | Usage |
|-----|-------|
| `error.field_required` | Required field validation error |
| `error.invalid_selection` | Invalid selection error |
| `error.min_selections` | Minimum selections not met error |
| `error.max_selections` | Maximum selections exceeded error |
| `error.invalid_option` | Invalid option selected error |

### Help and Instruction Text
| Key | Usage |
|-----|-------|
| `select.hint_text` | Default hint text for complex selections |
| `select.multiple_hint` | Hint for multiple selection fields |
| `select.grouped_hint` | Hint for grouped option fields |
| `select.dependent_hint` | Hint for dependent/cascading selects |

### Translation Usage Examples
```vue
<!-- Basic select with translation -->
<codex-select-field
  :label="$t('field.department')"
  :placeholder="$t('select.choose_option')"
  :options="departmentOptions"
  v-model="selectedDepartment"
  :hint="$t('select.hint_text')"
/>

<!-- Multiple selection with translation -->
<codex-select-field
  :label="$t('field.team_members')"
  :placeholder="$t('select.choose_options')"
  :options="memberOptions"
  v-model="selectedMembers"
  :multiple="true"
  :maxSelections="5"
  :hint="$t('select.multiple_hint')"
/>

<!-- Grouped options with translations -->
<codex-select-field
  :label="$t('field.skill_specialization')"
  :options="skillGroups"
  v-model="selectedSkill"
  :placeholder="$t('select.choose_option')"
/>

<!-- With translation for option groups -->
<template v-for="group in skillGroups" :key="group.label">
  <optgroup :label="$t(`skills.${group.key}`)">
    <option 
      v-for="option in group.options" 
      :key="option.value" 
      :value="option.value"
    >
      {{ $t(`skills.${option.translationKey}`) || option.displayValue }}
    </option>
  </optgroup>
</template>

<!-- Selection count display -->
<div v-if="multiple && selectedOptions.length" class="selection-summary">
  {{ $t('select.selected_count', { count: selectedOptions.length }) }}
  <button @click="clearAll">{{ $t('select.clear_all') }}</button>
</div>

<!-- Validation error display -->
<div v-if="errors.length" class="field-errors">
  <span v-for="error in errors" :key="error" class="error-message">
    {{ $t(`error.${error}`) }}
  </span>
</div>

<!-- Loading state -->
<div v-if="loadingOptions" class="loading-options">
  {{ $t('select.loading_options') }}
</div>

<!-- Empty state -->
<div v-if="!options.length && !loadingOptions" class="no-options">
  {{ $t('select.no_options_available') }}
</div>
```

### Field Translation Props Integration

The SelectField component supports the standard field translation pattern:

```vue
<!-- Using label_translation prop -->
<codex-select-field
  :label_translation="'user.role'"
  :placeholder_translation="'user.select_role'"
  :options="roleOptions"
  v-model="userRole"
/>

<!-- Fallback to direct translation in template -->
<codex-select-field
  :label="$t('user.role')"
  :placeholder="$t('user.select_role')"
  :options="roleOptions"
  v-model="userRole"
/>
```

## Component Registration
```javascript
// Global registration
app.component('CodexSelectField', SelectField)

// Local registration  
import SelectField from '@/components/fields/SelectField.vue'

export default {
  components: {
    CodexSelectField: SelectField
  }
}
``` 