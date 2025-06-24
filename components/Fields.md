# Field Components

## Overview
The Field components are a collection of form input elements that share a common structure and behavior while providing specific functionality for different input types. Each field component includes built-in support for labels, hints, helper text, error handling, and various layout options.

## Available Field Components
- `TextField` - Text input field
- `SelectField` - Dropdown selection field
- `CheckboxField` - Single checkbox field
- `CheckboxGroupField` - Group of checkboxes
- `RadioField` - Radio button field
- `NumberField` - Numeric input field
- `DateField` - Date input field
- `TelephoneField` - Phone number input field
- `VerificationField` - Verification code input field

## Basic Usage
```vue
<codex-text-field
  name="username"
  label="Username"
  type="text"
  required
/>

<codex-select-field
  name="country"
  label="Country"
  :options="countries"
  option-value="code"
  option-name="name"
/>
```

## Key Features
- Consistent layout and styling across all field types
- Built-in label support
- Optional hint and helper text
- Error handling and validation
- Flexible layout options
- Accessibility support
- Tooltip support
- Responsive design
- Form integration ready

## Common Props

### Base Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| name | String | Yes | - | Field identifier |
| label | String | No | '' | Field label text |
| required | Boolean | No | true | Whether the field is required |
| id | String | No | '' | Custom ID for the field |
| dusk | String | No | '' | Test identifier |

### Layout Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| layout | String | No | 'full' | Layout size ('auto', 'quarter', 'third', 'half', 'full') |

### Accessibility Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| ariaLabel | String | No | '' | ARIA label for accessibility |

### Help Text Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| hint | String | No | '' | Hint text displayed below label |
| tooltipText | String | No | '' | Tooltip content |
| tooltipIcon | String | No | '' | Custom tooltip icon |
| helperText | String | No | '' | Helper text below the field |

### Validation Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| errors | Array | No | [] | Array of error messages |
| hasError | Boolean | No | false | Error state flag |

## Component-Specific Props

### SelectField
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| settings | Array | No | [] | Array of options |
| optionValue | String | No | 'value' | Option value key |
| optionName | String | No | 'displayValue' | Option display text key |
| defaultToFirstOption | Boolean | No | false | Auto-select first option |

### DateField
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| format | String | No | 'YYYY-MM-DD' | Date format |
| min | String | No | - | Minimum allowed date |
| max | String | No | - | Maximum allowed date |

### NumberField
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| min | Number | No | - | Minimum value |
| max | Number | No | - | Maximum value |
| step | Number | No | 1 | Step increment |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| update:modelValue | value | Emitted when field value changes |
| close | - | Emitted when field closes (if applicable) |
| customEvent | - | Custom event handler |

## CSS Classes
- `_c-input-container`: Main container
- `_c-form-field--{size}`: Layout size classes
- `_c-label-container`: Label wrapper
- `_c-placeholder`: Placeholder state
- `_c-error`: Error state

## Examples

### Text Field with Helper Text
```vue
<codex-text-field
  name="email"
  label="Email Address"
  type="email"
  helper-text="We'll never share your email"
  :required="true"
/>
```

### Select Field with Custom Options
```vue
<codex-select-field
  name="category"
  label="Select Category"
  :options="[
    { value: 'electronics', displayValue: 'Electronics' },
    { value: 'clothing', displayValue: 'Clothing' }
  ]"
  option-value="value"
  option-name="displayValue"
/>
```

### Checkbox Group
```vue
<codex-checkbox-group-field
  name="preferences"
  label="Preferences"
  :options="[
    { value: 'email', displayValue: 'Email Updates' },
    { value: 'sms', displayValue: 'SMS Notifications' }
  ]"
/>
```

### Date Field with Range
```vue
<codex-date-field
  name="appointment"
  label="Appointment Date"
  format="YYYY-MM-DD"
  min="2024-01-01"
  max="2024-12-31"
/>
```

## Best Practices

### Recommended Usage
- Use consistent field types for similar data
- Implement proper form validation
- Provide clear, concise labels
- Use helper text for additional context
- Follow proper field grouping

### Accessibility Considerations
- Provide meaningful labels and ARIA attributes
- Ensure keyboard navigation support
- Use appropriate input types
- Maintain proper focus management
- Provide clear error messages

### Performance Considerations
- Avoid unnecessary field re-renders
- Use appropriate validation timing
- Consider form state management
- Optimize large option lists

### Common Pitfalls to Avoid
- Inconsistent validation messages
- Missing error states
- Unclear label text
- Poor mobile responsiveness
- Incomplete form validation

### State Management
- Handle field state changes properly
- Maintain form validation state
- Manage dependent field relationships
- Handle async validation appropriately

## Component Registration
All field components are registered with the `codex-` prefix followed by the field type (e.g., `codex-text-field`, `codex-select-field`).