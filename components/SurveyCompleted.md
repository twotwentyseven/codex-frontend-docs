# SurveyCompleted Component

## Overview
The SurveyCompleted component provides a simple interface for displaying completed survey responses with question and answer pairs. It features dynamic question type handling, flexible response formatting, and clean presentation of survey completion data for review and confirmation purposes.

## Basic Usage
```vue
<codex-survey-completed
  :survey="completedSurveyData"
/>
```

## Key Features
- Completed survey response display
- Dynamic question type handling
- Multiple response format support
- Question and answer pairing
- Clean response presentation
- Flexible component structure
- Minimal styling approach
- Simple data requirements

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| survey | Object | Yes | - | Survey object containing questions and responses |

## Events
Currently, the SurveyCompleted component does not emit custom events. It serves as a display component for completed survey data.

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content (overrides default title) -->
</template>
```

## Survey Object Structure
The survey prop expects an object with the following structure:
```javascript
{
  questions: [                    // Array of survey questions
    {
      key: String,               // Unique question identifier
      attributes: {              // Question configuration
        type: String,            // Question type ('checkbox', 'checkbox-group', 'text', etc.)
        question: String         // Question text
      }
    }
  ],
  responses: {                   // Object containing user responses
    [questionKey]: Mixed         // Response value (varies by question type)
  }
}
```

## Question Type Handling
The component supports multiple question types with appropriate response formatting:

### Checkbox Questions (`type: 'checkbox'`)
- **Response Format**: Boolean value
- **Display Logic**: Shows "Yes" for true, "No" for false
- **Layout**: Single question-response pair

### Checkbox Group Questions (`type: 'checkbox-group'`)
- **Response Format**: Array of selected values
- **Display Logic**: Iterates through array to show each selected option
- **Layout**: Question with multiple response items

### Other Question Types (text, radio, etc.)
- **Response Format**: Direct value display
- **Display Logic**: Shows response value as-is
- **Layout**: Simple question-response pair

## Response Display Logic
The component uses conditional rendering based on question type:

### Boolean Responses (Checkbox)
```vue
<span class="cdx_response">{{ survey.responses[question.key] ? 'Yes' : 'No' }}</span>
```

### Array Responses (Checkbox Group)
```vue
<span class="cdx_response" v-for="response in survey.responses[question.key]">{{ response }}</span>
```

### Direct Value Responses
```vue
<span class="cdx_response">{{ survey.responses[question.key] }}</span>
```

## Layout Structure
The component uses a simple flexbox column layout:
- **Container**: Main component container with flexible class binding
- **Header**: Survey title section
- **Form Fields**: Question and response pairs
- **Question Layout**: Flex column for each question-response pair

## Internationalization
The component uses the following translation keys:
- `survey.your_answers`: Default header title for survey responses

## Examples

### Basic Implementation
```vue
<codex-survey-completed
  :survey="surveyData"
/>
```

### Custom Header
```vue
<codex-survey-completed
  :survey="surveyData"
>
  <template #header>
    <div class="custom-survey-header">
      <h2>Survey Completion Summary</h2>
      <p>Thank you for completing our survey. Here are your responses:</p>
      <div class="completion-date">
        Completed on: {{ formatDate(survey.completed_at) }}
      </div>
    </div>
  </template>
</codex-survey-completed>
```

### In Survey Flow
```vue
<div class="survey-completion">
  <div class="completion-message">
    <h2>Survey Complete!</h2>
    <p>Your responses have been recorded.</p>
  </div>
  
  <codex-survey-completed
    :survey="completedSurvey"
    class="response-summary"
  />
  
  <div class="completion-actions">
    <button @click="downloadResponses">Download PDF</button>
    <button @click="submitAnotherSurvey">Take Another Survey</button>
    <button @click="returnToDashboard">Return to Dashboard</button>
  </div>
</div>
```

### With Response Formatting
```vue
<codex-survey-completed
  :survey="enhancedSurvey"
>
  <template #header>
    <div class="formatted-header">
      <h3>{{ survey.title }}</h3>
      <div class="survey-metadata">
        <span>Questions: {{ survey.questions.length }}</span>
        <span>Completion Rate: {{ calculateCompletionRate() }}%</span>
        <span>Time Taken: {{ survey.duration }}</span>
      </div>
    </div>
  </template>
</codex-survey-completed>

<script setup>
const enhancedSurvey = computed(() => ({
  ...props.survey,
  title: 'Customer Feedback Survey',
  duration: '3 minutes',
  completed_at: new Date()
}))

const calculateCompletionRate = () => {
  const totalQuestions = props.survey.questions.length
  const answeredQuestions = Object.keys(props.survey.responses).length
  return Math.round((answeredQuestions / totalQuestions) * 100)
}
</script>
```

### Custom Response Display
```vue
<div class="custom-survey-display">
  <codex-survey-completed
    :survey="survey"
    class="survey-responses"
  />
</div>

<style scoped>
.survey-responses .cdx_question {
  font-weight: 600;
  color: #2d3748;
  margin-bottom: 0.25rem;
}

.survey-responses .cdx_response {
  color: #4a5568;
  margin-bottom: 0.5rem;
  padding-left: 1rem;
}

.survey-responses .flex {
  border-bottom: 1px solid #e2e8f0;
  padding: 1rem 0;
}

.survey-responses .flex:last-child {
  border-bottom: none;
}
</style>
```

### In Modal or Review Interface
```vue
<div class="survey-review-modal">
  <div class="modal-header">
    <h3>Review Your Responses</h3>
    <button @click="closeModal">×</button>
  </div>
  
  <div class="modal-content">
    <codex-survey-completed
      :survey="surveyToReview"
    />
  </div>
  
  <div class="modal-actions">
    <button @click="editResponses">Edit Responses</button>
    <button @click="confirmSubmission">Confirm & Submit</button>
  </div>
</div>
```

### With Analytics Tracking
```vue
<codex-survey-completed
  :survey="survey"
  @mounted="trackSurveyReview"
/>

<script setup>
const trackSurveyReview = () => {
  analytics.track('survey_responses_reviewed', {
    survey_id: survey.id,
    question_count: survey.questions.length,
    response_count: Object.keys(survey.responses).length,
    completion_rate: calculateCompletionRate()
  })
}
</script>
```

### Grouped Question Display
```vue
<div class="grouped-survey-display">
  <div v-for="(group, groupName) in groupedQuestions" :key="groupName" class="question-group">
    <h4 class="group-title">{{ groupName }}</h4>
    
    <codex-survey-completed
      :survey="getGroupSurvey(group)"
      class="group-responses"
    />
  </div>
</div>

<script setup>
const groupedQuestions = computed(() => {
  return survey.questions.reduce((groups, question) => {
    const group = question.attributes.group || 'General'
    if (!groups[group]) groups[group] = []
    groups[group].push(question)
    return groups
  }, {})
})

const getGroupSurvey = (questions) => ({
  questions,
  responses: survey.responses
})
</script>
```

## CSS Classes
- `cdx_survey-completed`: Main component class (default)
- `cdx_title`: Header title styling
- `cdx_form-fields`: Form fields container
- `cdx_inputs`: Input fields styling
- `flex`: Flexbox utility class
- `flex-col`: Flex column direction
- `cdx_question`: Question text styling
- `cdx_response`: Response text styling

## Best Practices

### Recommended Usage Patterns
- Always provide proper survey object structure
- Handle missing response data gracefully
- Use appropriate question type handling
- Provide clear response formatting
- Consider response data privacy
- Implement proper data validation
- Use consistent styling across question types
- Handle edge cases in response display

### Common Pitfalls to Avoid
- Not handling missing survey data gracefully
- Missing validation for question types
- Forgetting to handle empty responses
- Not providing clear question-response pairing
- Missing responsive considerations
- Insufficient handling of complex response types
- Not considering data privacy requirements
- Missing accessibility considerations

### Accessibility Considerations
- Provide clear semantic structure for questions and responses
- Use appropriate ARIA attributes for survey data
- Ensure keyboard navigation if interactive elements are added
- Provide screen reader friendly survey information
- Include proper semantic HTML for survey structure
- Ensure adequate color contrast for text
- Handle focus management appropriately
- Use meaningful headings for survey sections

### Error Handling
- Handle missing survey data gracefully
- Provide fallbacks for malformed question objects
- Handle missing response data appropriately
- Display meaningful error states
- Validate question type compatibility
- Handle edge cases in response formatting
- Provide user feedback for data issues

### State Management
- Handle survey data changes appropriately
- Manage component lifecycle properly
- Handle data updates if survey is editable
- Coordinate with parent component state
- Manage survey display state effectively

### Performance Considerations
- Optimize survey data rendering
- Handle large survey datasets efficiently
- Minimize re-renders during data changes
- Implement proper cleanup if needed
- Cache formatted survey data appropriately
- Optimize response display calculations

### Data Handling
- Validate survey object structure
- Handle different question types appropriately
- Format responses consistently
- Handle empty or null responses gracefully
- Ensure data integrity in display
- Handle complex response structures
- Validate response data types

### Question Type Management
- Support all required question types
- Handle new question types gracefully
- Format responses appropriately for each type
- Provide consistent display logic
- Handle edge cases in question rendering
- Validate question configuration data
- Support extensible question type system

### Response Formatting
- Format boolean responses clearly (Yes/No)
- Handle array responses appropriately
- Display text responses safely
- Handle special characters in responses
- Format dates and numbers appropriately
- Handle long text responses
- Provide consistent formatting across types

### Survey Integration
- Coordinate with survey creation systems
- Handle survey completion workflows
- Integrate with survey analytics
- Support survey review and editing flows
- Handle survey versioning if applicable
- Coordinate with survey submission systems
- Track survey completion metrics

## Component Registration
The component is registered as `survey-completed` in the application. 