# Select Component

## Overview
The Select component provides a flexible dropdown selection interface that supports both native HTML select and custom-styled dropdown options. It features multi-select capabilities, grouped options, keyboard navigation, position-aware dropdown placement, dependency injection for configuration, and comprehensive accessibility support with ARIA attributes.

## Basic Usage
```vue
<!-- Must be used within a provider that injects required values -->
<template>
  <div>
    <!-- Provider component that injects select configuration -->
    <select-provider
      :options="selectOptions"
      :placeholder="'Choose an option'"
      :aria-label="'Select an option'"
      :name="'select_field'"
      :id="'select-1'"
      :option-value="'value'"
      :option-name="'displayValue'"
    >
      <codex-select v-model="selectedValue" />
    </select-provider>
  </div>
</template>
```

## Key Features
- Dual rendering modes: native HTML select and custom dropdown
- Single and multi-select functionality with tag-based display
- Grouped option support with visual categorization
- Position-aware dropdown placement (above/below)
- Keyboard navigation and accessibility compliance
- Vue dependency injection for flexible configuration
- Custom option rendering with HTML content support
- Responsive dropdown positioning with viewport detection

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| modelValue | Array\|String\|Number | No | [] | Selected value(s) for the select |
| selectedGroup | Any | No | null | Currently selected option group |
| dusk | String | No | '' | Testing attribute for browser automation |

## Injected Dependencies

The component relies on Vue's dependency injection system for configuration:

### Required Injections
| Injection Key | Type | Description |
|---------------|------|-------------|
| options | Array | Array of selectable options |
| placeholder | String | Placeholder text when no option is selected |
| ariaLabel | String | ARIA label for accessibility |
| name | String | Name attribute for form submission |
| id | String | Unique identifier for the select |
| optionValue | String | Property name for option values (default: 'value') |
| optionName | String | Property name for option display text (default: 'displayValue') |

### Optional Injections
| Injection Key | Type | Default | Description |
|---------------|------|---------|-------------|
| required | Boolean | false | Whether the field is required |
| multiple | Boolean | false | Enable multi-select functionality |
| hasError | Boolean | false | Whether the field has validation errors |
| hasGroupedOption | Boolean | false | Whether options are grouped |
| limit | Number | null | Maximum number of visible options in native select |
| limitToOneOptionGroup | Boolean | false | Limit selection to one option group |
| defaultToFirstOption | Boolean | false | Auto-select first option by default |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| update:modelValue | value: Any | Emitted when selection changes |
| update:selectedGroup | group: Any | Emitted when selected group changes |
| emitChange | value: Any | Emitted on selection change for additional handling |

## Option Structure
The component supports flexible option structures:

### Simple Options
```javascript
[
  { value: 'option1', displayValue: 'Option 1' },
  { value: 'option2', displayValue: 'Option 2' }
]
```

### Grouped Options
```javascript
[
  {
    label: 'Group 1',
    options: [
      { value: 'g1_option1', displayValue: 'Group 1 Option 1' },
      { value: 'g1_option2', displayValue: 'Group 1 Option 2' }
    ]
  },
  {
    label: 'Group 2',
    options: [
      { value: 'g2_option1', displayValue: 'Group 2 Option 1' }
    ]
  }
]
```

## Dropdown Positioning
The component implements intelligent dropdown positioning:
- **Viewport Detection**: Calculates available space above and below
- **Dynamic Placement**: Positions dropdown above or below based on space
- **Responsive Width**: Matches parent element width
- **Max Height**: Limits dropdown height with scrolling
- **Z-Index Management**: Ensures dropdown appears above other elements

## Multi-Select Features
When multiple selection is enabled:
- **Tag Display**: Selected options shown as removable tags
- **Checkbox Indicators**: Visual checkboxes for grouped options
- **Remove Functionality**: Click to remove individual selections
- **Group Limitations**: Optional restriction to single option group

## Keyboard Navigation
The component supports comprehensive keyboard interaction:
- **Tab Navigation**: Focus management between elements
- **Arrow Keys**: Navigate through options
- **Enter/Space**: Select/deselect options
- **Escape**: Close dropdown
- **Focus Management**: Proper focus indication and trapping

## Accessibility Features
Comprehensive accessibility support:
- **ARIA Attributes**: Proper role, expanded, selected states
- **Screen Reader**: Full compatibility with assistive technologies
- **Keyboard Navigation**: Complete keyboard accessibility
- **Focus Management**: Logical tab order and focus indication
- **Label Association**: Proper label/control relationships

## Use Cases
Suitable for:
- **Form Fields**: Standard form dropdown selections
- **Multi-Select Lists**: Tag-based multi-selection interfaces
- **Categorized Options**: Grouped option selections
- **Search Filters**: Filtering with multiple criteria
- **Settings Panels**: Configuration option selection

## Internationalization
The component receives internationalized text through dependency injection, allowing for localized placeholder and option text.

## Examples

### Basic Single Select
```vue
<template>
  <form @submit="handleSubmit">
    <select-provider
      :options="countryOptions"
      :placeholder="'Select a country'"
      :aria-label="'Choose your country'"
      :name="'country'"
      :id="'country-select'"
      :option-value="'code'"
      :option-name="'name'"
    >
      <codex-select v-model="selectedCountry" />
    </select-provider>
  </form>
</template>

<script setup>
const selectedCountry = ref('')
const countryOptions = ref([
  { code: 'us', name: 'United States' },
  { code: 'ca', name: 'Canada' },
  { code: 'uk', name: 'United Kingdom' },
  { code: 'de', name: 'Germany' },
  { code: 'fr', name: 'France' }
])

const handleSubmit = () => {
  console.log('Selected country:', selectedCountry.value)
}
</script>
```

### Multi-Select with Tags
```vue
<template>
  <div class="skill-selector">
    <h3>Select Your Skills</h3>
    
    <select-provider
      :options="skillOptions"
      :placeholder="'Choose skills'"
      :aria-label="'Select your skills'"
      :name="'skills'"
      :id="'skills-select'"
      :multiple="true"
      :option-value="'id'"
      :option-name="'name'"
    >
      <codex-select v-model="selectedSkills" />
    </select-provider>
    
    <div v-if="selectedSkills.length > 0" class="selected-summary">
      <p>Selected {{ selectedSkills.length }} skill(s)</p>
    </div>
  </div>
</template>

<script setup>
const selectedSkills = ref([])
const skillOptions = ref([
  { id: 'js', name: 'JavaScript' },
  { id: 'vue', name: 'Vue.js' },
  { id: 'react', name: 'React' },
  { id: 'python', name: 'Python' },
  { id: 'node', name: 'Node.js' },
  { id: 'php', name: 'PHP' }
])

watch(selectedSkills, (newSkills) => {
  console.log('Selected skills:', newSkills)
})
</script>
```

### Grouped Options Select
```vue
<template>
  <div class="categorized-products">
    <h3>Select Product</h3>
    
    <select-provider
      :options="productOptions"
      :placeholder="'Choose a product'"
      :aria-label="'Select product from categories'"
      :name="'product'"
      :id="'product-select'"
      :has-grouped-option="true"
      :option-value="'id'"
      :option-name="'name'"
    >
      <codex-select v-model="selectedProduct" />
    </select-provider>
  </div>
</template>

<script setup>
const selectedProduct = ref('')
const productOptions = ref([
  {
    label: 'Electronics',
    options: [
      { id: 'laptop', name: 'Laptop Computer' },
      { id: 'phone', name: 'Smartphone' },
      { id: 'tablet', name: 'Tablet Device' }
    ]
  },
  {
    label: 'Clothing',
    options: [
      { id: 'shirt', name: 'T-Shirt' },
      { id: 'pants', name: 'Jeans' },
      { id: 'shoes', name: 'Sneakers' }
    ]
  },
  {
    label: 'Books',
    options: [
      { id: 'fiction', name: 'Fiction Novel' },
      { id: 'technical', name: 'Technical Manual' }
    ]
  }
])
</script>
```

### Multi-Select with Group Limitation
```vue
<template>
  <div class="department-roles">
    <h3>Select Roles (One Department Only)</h3>
    
    <select-provider
      :options="roleOptions"
      :placeholder="'Choose roles'"
      :aria-label="'Select roles from one department'"
      :name="'roles'"
      :id="'roles-select'"
      :multiple="true"
      :has-grouped-option="true"
      :limit-to-one-option-group="true"
      :option-value="'id'"
      :option-name="'title'"
    >
      <codex-select 
        v-model="selectedRoles" 
        @update:selectedGroup="handleGroupChange"
      />
    </select-provider>
    
    <div v-if="currentDepartment" class="department-info">
      <p>Current Department: {{ currentDepartment }}</p>
    </div>
  </div>
</template>

<script setup>
const selectedRoles = ref([])
const currentDepartment = ref('')

const roleOptions = ref([
  {
    label: 'Engineering',
    options: [
      { id: 'frontend', title: 'Frontend Developer' },
      { id: 'backend', title: 'Backend Developer' },
      { id: 'devops', title: 'DevOps Engineer' }
    ]
  },
  {
    label: 'Design',
    options: [
      { id: 'ui', title: 'UI Designer' },
      { id: 'ux', title: 'UX Designer' },
      { id: 'graphic', title: 'Graphic Designer' }
    ]
  },
  {
    label: 'Marketing',
    options: [
      { id: 'content', title: 'Content Manager' },
      { id: 'social', title: 'Social Media Manager' }
    ]
  }
])

const handleGroupChange = (group) => {
  currentDepartment.value = group?.label || ''
}
</script>
```

### Select with Error Handling
```vue
<template>
  <form @submit="handleSubmit" class="form-with-validation">
    <div class="form-field">
      <select-provider
        :options="categoryOptions"
        :placeholder="'Select category'"
        :aria-label="'Choose category'"
        :name="'category'"
        :id="'category-select'"
        :required="true"
        :has-error="validationErrors.category"
        :option-value="'id'"
        :option-name="'label'"
      >
        <codex-select v-model="formData.category" />
      </select-provider>
      
      <div v-if="validationErrors.category" class="error-message">
        Category selection is required
      </div>
    </div>
    
    <button type="submit">Submit Form</button>
  </form>
</template>

<script setup>
const formData = ref({
  category: ''
})

const validationErrors = ref({
  category: false
})

const categoryOptions = ref([
  { id: 'tech', label: 'Technology' },
  { id: 'business', label: 'Business' },
  { id: 'education', label: 'Education' },
  { id: 'health', label: 'Healthcare' }
])

const handleSubmit = (event) => {
  event.preventDefault()
  
  // Validate form
  validationErrors.value.category = !formData.value.category
  
  if (!validationErrors.value.category) {
    console.log('Form submitted:', formData.value)
  }
}
</script>
```

### Dynamic Options Loading
```vue
<template>
  <div class="dynamic-select">
    <h3>Select Location</h3>
    
    <div v-if="loadingOptions" class="loading-state">
      Loading options...
    </div>
    
    <select-provider
      v-else
      :options="locationOptions"
      :placeholder="'Choose location'"
      :aria-label="'Select your location'"
      :name="'location'"
      :id="'location-select'"
      :has-grouped-option="true"
      :option-value="'id'"
      :option-name="'name'"
    >
      <codex-select v-model="selectedLocation" />
    </select-provider>
    
    <button v-if="!loadingOptions" @click="refreshOptions">
      Refresh Options
    </button>
  </div>
</template>

<script setup>
const selectedLocation = ref('')
const locationOptions = ref([])
const loadingOptions = ref(true)

const loadLocationOptions = async () => {
  loadingOptions.value = true
  
  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    locationOptions.value = [
      {
        label: 'North America',
        options: [
          { id: 'us-ny', name: 'New York, USA' },
          { id: 'us-ca', name: 'Los Angeles, USA' },
          { id: 'ca-on', name: 'Toronto, Canada' }
        ]
      },
      {
        label: 'Europe',
        options: [
          { id: 'uk-lon', name: 'London, UK' },
          { id: 'de-ber', name: 'Berlin, Germany' },
          { id: 'fr-par', name: 'Paris, France' }
        ]
      }
    ]
  } catch (error) {
    console.error('Failed to load options:', error)
  } finally {
    loadingOptions.value = false
  }
}

const refreshOptions = () => {
  selectedLocation.value = ''
  loadLocationOptions()
}

onMounted(() => {
  loadLocationOptions()
})
</script>
```

### Search-Enabled Select
```vue
<template>
  <div class="searchable-select">
    <h3>Search and Select</h3>
    
    <div class="search-input">
      <input
        v-model="searchTerm"
        placeholder="Search options..."
        @input="filterOptions"
      />
    </div>
    
    <select-provider
      :options="filteredOptions"
      :placeholder="'Choose from filtered results'"
      :aria-label="'Select from search results'"
      :name="'search_select'"
      :id="'search-select'"
      :option-value="'id'"
      :option-name="'name'"
    >
      <codex-select v-model="selectedOption" />
    </select-provider>
    
    <div v-if="searchTerm && filteredOptions.length === 0" class="no-results">
      No options match your search
    </div>
  </div>
</template>

<script setup>
const selectedOption = ref('')
const searchTerm = ref('')
const filteredOptions = ref([])

const allOptions = ref([
  { id: 'apple', name: 'Apple' },
  { id: 'banana', name: 'Banana' },
  { id: 'cherry', name: 'Cherry' },
  { id: 'date', name: 'Date' },
  { id: 'elderberry', name: 'Elderberry' },
  { id: 'fig', name: 'Fig' },
  { id: 'grape', name: 'Grape' }
])

const filterOptions = () => {
  if (!searchTerm.value) {
    filteredOptions.value = allOptions.value
    return
  }
  
  const term = searchTerm.value.toLowerCase()
  filteredOptions.value = allOptions.value.filter(option =>
    option.name.toLowerCase().includes(term)
  )
}

// Initialize with all options
onMounted(() => {
  filteredOptions.value = allOptions.value
})

watch(searchTerm, filterOptions)
</script>
```

## CSS Classes
- `_c-input-container-inner`: Container wrapper for select elements
- `_c-select`: Base select styling
- `_c-native-select`: Native HTML select styling
- `_c-custom-select`: Custom dropdown select styling
- `_c-select-active`: Applied when dropdown is open
- `_c-multi-select`: Applied for multi-select functionality
- `_c-selected-options`: Container for selected option display
- `_c-selected-option`: Individual selected option styling
- `_c-selected-option-tag`: Tag styling for multi-select
- `_c-select-dropdown`: Dropdown container styling
- `_c-select-dropdown--above`: Applied when dropdown opens above
- `_c-select-option`: Individual option styling
- `_c-select-option-active`: Applied to selected/active options
- `_c-select-option-highlighted`: Applied to keyboard-highlighted options
- `_c-select-option-group`: Option group container
- `_c-select-option-group-label`: Group label styling
- `_c-error`: Applied when hasError injection is true

## Best Practices

### Recommended Usage Patterns
- Always use within a provider component that injects required configuration
- Provide meaningful placeholder text for user guidance
- Use appropriate option value and display name properties
- Implement proper validation for required fields
- Handle loading states when options are fetched dynamically
- Test keyboard navigation and accessibility compliance
- Use grouped options for large option sets with clear categorization

### Common Pitfalls to Avoid
- Not providing proper provider context for dependency injection
- Using complex objects as option values without proper serialization
- Missing keyboard navigation support in custom implementations
- Not handling viewport boundaries for dropdown positioning
- Forgetting to clear selections when options change
- Not providing proper ARIA labels for accessibility
- Using inappropriate option structures for grouped selections

### Accessibility Considerations
- Provide descriptive ARIA labels for the select element
- Ensure proper focus management within dropdown
- Support full keyboard navigation (arrows, enter, escape)
- Use semantic HTML structure for screen reader compatibility
- Provide clear indication of selected state
- Handle focus trapping appropriately when dropdown is open
- Test with screen readers for proper option announcement

### Performance Considerations
- Optimize option filtering for large datasets
- Use virtual scrolling for very long option lists
- Implement debouncing for search functionality
- Cache computed option data when appropriate
- Minimize re-renders during option changes
- Handle large grouped option sets efficiently

### Form Integration
- Coordinate with parent form validation systems
- Provide proper name attributes for form submission
- Handle form reset functionality appropriately
- Support required field validation
- Integrate with form error handling systems
- Provide proper form field labeling

### Multi-Select Handling
- Design clear visual indication for selected items
- Provide easy removal mechanism for selected options
- Handle tag overflow in multi-select displays
- Implement proper keyboard navigation for tag removal
- Consider maximum selection limits when appropriate
- Handle group-based selection restrictions properly

### Dropdown Positioning
- Test dropdown behavior near viewport edges
- Handle scrolling containers appropriately
- Implement responsive dropdown sizing
- Consider mobile device constraints
- Test dropdown positioning in various layouts
- Handle z-index conflicts with other elements

### Option Management
- Validate option data structure consistency
- Handle dynamic option loading states
- Implement proper option caching strategies
- Handle option updates without losing selections
- Provide fallback for missing option data
- Handle edge cases like empty option sets

### Error Handling
- Provide clear error messages for validation failures
- Handle network errors during option loading
- Validate option data integrity
- Handle edge cases in option selection
- Provide fallback behavior for missing dependencies
- Test error recovery scenarios

## Component Registration
The component is registered as `codex-select` in the application. 