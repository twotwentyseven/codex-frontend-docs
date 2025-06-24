# TextAreaField Component

## Overview
The TextAreaField component provides a multi-line text input area with comprehensive form functionality including resizable text area, character counting, layout management, and validation integration. It supports flexible sizing, readonly states, and seamless integration with the form validation system while maintaining proper textarea semantics and accessibility features.

## Basic Usage
```vue
<template>
  <div class="textarea-form">
    <codex-text-area-field
      v-model="description"
      :name="'description'"
      :type="'textarea'"
      :label="'Description'"
      :placeholder="'Enter a detailed description'"
      :required="true"
    />
  </div>
</template>

<script setup>
const description = ref('')
</script>
```

## Key Features
- Multi-line text input with automatic resizing
- Character and word counting capabilities
- Flexible layout configurations (auto, quarter, third, half, full)
- Label and hint system with tooltip support
- Helper text and error handling
- Readonly state management
- Accessibility features with ARIA labels
- Testing support with Dusk attributes

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `name` | `String` | `required` | Field name for form submission |
| `type` | `String` | `required` | Input type (should be 'textarea') |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
| `placeholder` | `String` | `''` | Textarea placeholder text |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the textarea |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'1'` | Layout size (auto, 4, 3, 2, 1) |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `required` | `Boolean` | `true` | Whether field is required |
| `readonly` | `Boolean` | `false` | Whether field is readonly |
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

The component uses `defineModel()` to create a two-way binding:

```vue
<template>
  <codex-text-area-field v-model="content" />
</template>

<script setup>
const content = ref('')
// content automatically updates when textarea changes
</script>
```

## Examples

### Blog Post Editor
```vue
<template>
  <div class="blog-editor">
    <h3>Create New Blog Post</h3>
    
    <div class="editor-form">
      <codex-text-area-field
        v-model="post.title"
        :name="'title'"
        :type="'text'"
        :label="'Post Title'"
        :placeholder="'Enter an engaging title'"
        :layout="'1'"
        :required="true"
        :errors="postErrors.title"
        :maxlength="100"
      />
      
      <codex-text-area-field
        v-model="post.excerpt"
        :name="'excerpt'"
        :type="'textarea'"
        :label="'Post Excerpt'"
        :placeholder="'Write a brief summary of your post'"
        :layout="'1'"
        :required="true"
        :errors="postErrors.excerpt"
        :helper-text="excerptHelperText"
        :maxlength="300"
      />
      
      <codex-text-area-field
        v-model="post.content"
        :name="'content'"
        :type="'textarea'"
        :label="'Post Content'"
        :placeholder="'Write your full blog post here...'"
        :layout="'1'"
        :required="true"
        :errors="postErrors.content"
        :helper-text="contentHelperText"
        :rows="15"
      />
      
      <codex-text-area-field
        v-model="post.tags"
        :name="'tags'"
        :type="'textarea'"
        :label="'Tags'"
        :placeholder="'Enter tags separated by commas'"
        :layout="'2'"
        :required="false"
        :helper-text="'Use relevant keywords for better discoverability'"
        :rows="3"
      />
      
      <codex-text-area-field
        v-model="post.metaDescription"
        :name="'meta_description'"
        :type="'textarea'"
        :label="'SEO Meta Description'"
        :placeholder="'Describe your post for search engines'"
        :layout="'2'"
        :required="false"
        :helper-text="metaDescriptionHelperText"
        :maxlength="160"
        :rows="3"
      />
    </div>
    
    <div class="post-preview" v-if="isPostValid">
      <h4>Preview</h4>
      <div class="preview-card">
        <h5>{{ post.title }}</h5>
        <p class="excerpt">{{ post.excerpt }}</p>
        <div class="content-preview">
          {{ post.content.substring(0, 200) }}{{ post.content.length > 200 ? '...' : '' }}
        </div>
        <div v-if="post.tags" class="tags-preview">
          <span v-for="tag in tagsList" :key="tag" class="tag">{{ tag.trim() }}</span>
        </div>
      </div>
    </div>
    
    <div class="editor-actions">
      <button @click="saveDraft" class="draft-btn">
        Save as Draft
      </button>
      <button @click="publishPost" :disabled="!isPostValid" class="publish-btn">
        Publish Post
      </button>
    </div>
  </div>
</template>

<script setup>
const post = reactive({
  title: '',
  excerpt: '',
  content: '',
  tags: '',
  metaDescription: ''
})

const postErrors = ref({
  title: [],
  excerpt: [],
  content: []
})

const excerptHelperText = computed(() => {
  const length = post.excerpt.length
  if (length === 0) return 'Write a compelling summary'
  if (length < 50) return `${length}/300 characters - Add more detail`
  if (length > 250) return `${length}/300 characters - Consider shortening`
  return `${length}/300 characters - Good length`
})

const contentHelperText = computed(() => {
  const wordCount = post.content.trim() ? post.content.trim().split(/\s+/).length : 0
  const charCount = post.content.length
  
  if (wordCount === 0) return 'Start writing your post content'
  if (wordCount < 300) return `${wordCount} words - Consider adding more content`
  if (wordCount > 2000) return `${wordCount} words - Very long post`
  return `${wordCount} words, ${charCount} characters`
})

const metaDescriptionHelperText = computed(() => {
  const length = post.metaDescription.length
  if (length === 0) return 'Optional but recommended for SEO'
  if (length < 120) return `${length}/160 characters - Could be longer`
  if (length > 160) return `${length}/160 characters - Too long for search results`
  return `${length}/160 characters - Perfect length`
})

const isPostValid = computed(() => {
  return post.title.trim() && 
         post.excerpt.trim() && 
         post.content.trim() && 
         post.content.trim().split(/\s+/).length >= 100
})

const tagsList = computed(() => {
  return post.tags ? post.tags.split(',').filter(tag => tag.trim()) : []
})

const saveDraft = async () => {
  try {
    await saveBlogPostDraft(post)
    toast.success('Draft saved successfully')
  } catch (error) {
    toast.error('Failed to save draft')
  }
}

const publishPost = async () => {
  try {
    await publishBlogPost(post)
    toast.success('Post published successfully!')
    router.push('/blog/posts')
  } catch (error) {
    if (error.response?.data?.errors) {
      postErrors.value = error.response.data.errors
    }
    toast.error('Failed to publish post')
  }
}
</script>
```

### Customer Feedback Form
```vue
<template>
  <div class="feedback-form">
    <h3>We Value Your Feedback</h3>
    
    <div class="feedback-sections">
      <div class="section">
        <h4>Overall Experience</h4>
        
        <codex-text-area-field
          v-model="feedback.overallExperience"
          :name="'overall_experience'"
          :type="'textarea'"
          :label="'How was your overall experience?'"
          :placeholder="'Please describe your experience with our service...'"
          :layout="'1'"
          :required="true"
          :errors="feedbackErrors.overallExperience"
          :helper-text="overallExperienceHelper"
          :rows="4"
        />
      </div>
      
      <div class="section">
        <h4>Specific Areas</h4>
        
        <div class="feedback-grid">
          <codex-text-area-field
            v-model="feedback.productQuality"
            :name="'product_quality'"
            :type="'textarea'"
            :label="'Product Quality'"
            :placeholder="'Comments about product quality...'"
            :layout="'2'"
            :required="false"
            :helper-text="'What did you think of our product?'"
            :rows="3"
          />
          
          <codex-text-area-field
            v-model="feedback.customerService"
            :name="'customer_service'"
            :type="'textarea'"
            :label="'Customer Service'"
            :placeholder="'Comments about customer service...'"
            :layout="'2'"
            :required="false"
            :helper-text="'How was your interaction with our team?'"
            :rows="3"
          />
          
          <codex-text-area-field
            v-model="feedback.deliveryExperience"
            :name="'delivery_experience'"
            :type="'textarea'"
            :label="'Delivery Experience'"
            :placeholder="'Comments about delivery...'"
            :layout="'2'"
            :required="false"
            :helper-text="'How was the delivery process?'"
            :rows="3"
          />
          
          <codex-text-area-field
            v-model="feedback.websiteUsability"
            :name="'website_usability'"
            :type="'textarea'"
            :label="'Website Usability'"
            :placeholder="'Comments about our website...'"
            :layout="'2'"
            :required="false"
            :helper-text="'How easy was it to navigate our website?'"
            :rows="3"
          />
        </div>
      </div>
      
      <div class="section">
        <h4>Suggestions & Improvements</h4>
        
        <codex-text-area-field
          v-model="feedback.improvements"
          :name="'improvements'"
          :type="'textarea'"
          :label="'What could we improve?'"
          :placeholder="'Please share any suggestions for improvement...'"
          :layout="'1'"
          :required="false"
          :helper-text="'Your suggestions help us serve you better'"
          :rows="4"
        />
        
        <codex-text-area-field
          v-model="feedback.additionalComments"
          :name="'additional_comments'"
          :type="'textarea'"
          :label="'Additional Comments'"
          :placeholder="'Any other thoughts or comments...'"
          :layout="'1'"
          :required="false"
          :helper-text="'Feel free to share anything else'"
          :rows="3"
        />
      </div>
    </div>
    
    <div class="feedback-summary" v-if="feedbackCompleteness > 0">
      <h4>Feedback Summary</h4>
      <div class="completeness-indicator">
        <div class="progress-bar">
          <div class="progress-fill" :style="{ width: feedbackCompleteness + '%' }"></div>
        </div>
        <span>{{ feedbackCompleteness }}% Complete</span>
      </div>
      
      <div class="word-count-summary">
        <p><strong>Total Words:</strong> {{ totalWordCount }}</p>
        <p><strong>Sections Completed:</strong> {{ completedSections }}/{{ totalSections }}</p>
      </div>
    </div>
    
    <div class="feedback-actions">
      <button @click="submitFeedback" :disabled="!canSubmitFeedback" class="submit-btn">
        Submit Feedback
      </button>
      <button @click="saveDraft" class="draft-btn">
        Save for Later
      </button>
    </div>
  </div>
</template>

<script setup>
const feedback = reactive({
  overallExperience: '',
  productQuality: '',
  customerService: '',
  deliveryExperience: '',
  websiteUsability: '',
  improvements: '',
  additionalComments: ''
})

const feedbackErrors = ref({
  overallExperience: []
})

const overallExperienceHelper = computed(() => {
  const wordCount = feedback.overallExperience.trim() ? 
    feedback.overallExperience.trim().split(/\s+/).length : 0
  
  if (wordCount === 0) return 'Please share your experience'
  if (wordCount < 10) return `${wordCount} words - Please provide more detail`
  if (wordCount > 200) return `${wordCount} words - Consider being more concise`
  return `${wordCount} words - Good detail level`
})

const totalWordCount = computed(() => {
  const allText = Object.values(feedback).join(' ')
  return allText.trim() ? allText.trim().split(/\s+/).length : 0
})

const completedSections = computed(() => {
  return Object.values(feedback).filter(value => value.trim().length > 0).length
})

const totalSections = computed(() => {
  return Object.keys(feedback).length
})

const feedbackCompleteness = computed(() => {
  return Math.round((completedSections.value / totalSections.value) * 100)
})

const canSubmitFeedback = computed(() => {
  return feedback.overallExperience.trim().length > 0 &&
         feedback.overallExperience.trim().split(/\s+/).length >= 10
})

const submitFeedback = async () => {
  try {
    await submitCustomerFeedback(feedback)
    toast.success('Thank you for your feedback!')
    router.push('/feedback-confirmation')
  } catch (error) {
    if (error.response?.data?.errors) {
      feedbackErrors.value = error.response.data.errors
    }
    toast.error('Failed to submit feedback')
  }
}

const saveDraft = () => {
  localStorage.setItem('feedbackDraft', JSON.stringify(feedback))
  toast.success('Feedback saved for later')
}

// Auto-save draft every 30 seconds
let autoSaveInterval
onMounted(() => {
  // Load saved draft
  const savedDraft = localStorage.getItem('feedbackDraft')
  if (savedDraft) {
    Object.assign(feedback, JSON.parse(savedDraft))
  }
  
  // Start auto-save
  autoSaveInterval = setInterval(() => {
    if (totalWordCount.value > 0) {
      localStorage.setItem('feedbackDraft', JSON.stringify(feedback))
    }
  }, 30000)
})

onUnmounted(() => {
  if (autoSaveInterval) {
    clearInterval(autoSaveInterval)
  }
})
</script>
```

### Technical Documentation Writer
```vue
<template>
  <div class="documentation-editor">
    <h3>Technical Documentation</h3>
    
    <div class="doc-editor">
      <codex-text-area-field
        v-model="documentation.overview"
        :name="'overview'"
        :type="'textarea'"
        :label="'Overview'"
        :placeholder="'Provide a high-level overview of the feature or system...'"
        :layout="'1'"
        :required="true"
        :errors="docErrors.overview"
        :helper-text="overviewHelper"
        :rows="6"
      />
      
      <div class="doc-sections">
        <div class="section-grid">
          <codex-text-area-field
            v-model="documentation.requirements"
            :name="'requirements'"
            :type="'textarea'"
            :label="'Requirements'"
            :placeholder="'List system requirements, dependencies, prerequisites...'"
            :layout="'2'"
            :required="true"
            :helper-text="'What is needed before implementation?'"
            :rows="8"
          />
          
          <codex-text-area-field
            v-model="documentation.implementation"
            :name="'implementation'"
            :type="'textarea'"
            :label="'Implementation Details'"
            :placeholder="'Describe implementation steps, code examples, configuration...'"
            :layout="'2'"
            :required="true"
            :helper-text="'Step-by-step implementation guide'"
            :rows="8"
          />
          
          <codex-text-area-field
            v-model="documentation.apiReference"
            :name="'api_reference'"
            :type="'textarea'"
            :label="'API Reference'"
            :placeholder="'Document API endpoints, parameters, responses...'"
            :layout="'2'"
            :required="false"
            :helper-text="'Include code examples and response formats'"
            :rows="10"
          />
          
          <codex-text-area-field
            v-model="documentation.troubleshooting"
            :name="'troubleshooting'"
            :type="'textarea'"
            :label="'Troubleshooting'"
            :placeholder="'Common issues, error messages, solutions...'"
            :layout="'2'"
            :required="false"
            :helper-text="'Help users solve common problems'"
            :rows="10"
          />
        </div>
      </div>
      
      <div class="advanced-sections">
        <codex-text-area-field
          v-model="documentation.examples"
          :name="'examples'"
          :type="'textarea'"
          :label="'Code Examples'"
          :placeholder="'Provide practical code examples and use cases...'"
          :layout="'1'"
          :required="false"
          :helper-text="examplesHelper"
          :rows="12"
          :readonly="false"
        />
        
        <codex-text-area-field
          v-model="documentation.notes"
          :name="'notes'"
          :type="'textarea'"
          :label="'Additional Notes'"
          :placeholder="'Any additional information, warnings, tips...'"
          :layout="'1'"
          :required="false"
          :helper-text="'Important notes for developers'"
          :rows="4"
        />
      </div>
    </div>
    
    <div class="doc-analytics">
      <div class="analytics-cards">
        <div class="analytics-card">
          <h5>Documentation Length</h5>
          <span class="metric">{{ totalWordCount }} words</span>
          <span class="sub-metric">{{ readingTime }} min read</span>
        </div>
        
        <div class="analytics-card">
          <h5>Completion Status</h5>
          <span class="metric">{{ completionPercentage }}%</span>
          <span class="sub-metric">{{ completedSections }}/{{ totalSections }} sections</span>
        </div>
        
        <div class="analytics-card">
          <h5>Code Examples</h5>
          <span class="metric">{{ codeBlockCount }}</span>
          <span class="sub-metric">blocks detected</span>
        </div>
        
        <div class="analytics-card">
          <h5>Documentation Score</h5>
          <span class="metric" :class="documentationScoreClass">{{ documentationScore }}</span>
          <span class="sub-metric">quality rating</span>
        </div>
      </div>
    </div>
    
    <div class="doc-actions">
      <button @click="generateTOC" class="toc-btn">
        Generate Table of Contents
      </button>
      <button @click="previewDocumentation" class="preview-btn">
        Preview Documentation
      </button>
      <button @click="saveDocumentation" :disabled="!canSaveDocumentation" class="save-btn">
        Save Documentation
      </button>
    </div>
  </div>
</template>

<script setup>
const documentation = reactive({
  overview: '',
  requirements: '',
  implementation: '',
  apiReference: '',
  troubleshooting: '',
  examples: '',
  notes: ''
})

const docErrors = ref({
  overview: []
})

const overviewHelper = computed(() => {
  const wordCount = documentation.overview.trim() ? 
    documentation.overview.trim().split(/\s+/).length : 0
  
  if (wordCount === 0) return 'Start with a clear overview'
  if (wordCount < 50) return `${wordCount} words - Add more detail about the purpose and scope`
  if (wordCount > 300) return `${wordCount} words - Consider moving details to other sections`
  return `${wordCount} words - Good overview length`
})

const examplesHelper = computed(() => {
  const codeBlocks = (documentation.examples.match(/```/g) || []).length / 2
  const wordCount = documentation.examples.trim() ? 
    documentation.examples.trim().split(/\s+/).length : 0
  
  if (wordCount === 0) return 'Add practical code examples'
  return `${Math.floor(codeBlocks)} code blocks, ${wordCount} words`
})

const totalWordCount = computed(() => {
  const allText = Object.values(documentation).join(' ')
  return allText.trim() ? allText.trim().split(/\s+/).length : 0
})

const readingTime = computed(() => {
  // Average reading speed: 200 words per minute
  return Math.ceil(totalWordCount.value / 200)
})

const completedSections = computed(() => {
  return Object.values(documentation).filter(value => value.trim().length > 0).length
})

const totalSections = computed(() => {
  return Object.keys(documentation).length
})

const completionPercentage = computed(() => {
  return Math.round((completedSections.value / totalSections.value) * 100)
})

const codeBlockCount = computed(() => {
  const allText = Object.values(documentation).join('\n')
  return Math.floor((allText.match(/```/g) || []).length / 2)
})

const documentationScore = computed(() => {
  let score = 0
  
  // Base completeness score (40%)
  score += (completionPercentage.value * 0.4)
  
  // Word count score (20%)
  if (totalWordCount.value > 500) score += 20
  else if (totalWordCount.value > 200) score += 10
  
  // Code examples score (20%)
  if (codeBlockCount.value > 3) score += 20
  else if (codeBlockCount.value > 0) score += 10
  
  // Required sections score (20%)
  if (documentation.overview && documentation.requirements && documentation.implementation) {
    score += 20
  }
  
  return Math.round(score)
})

const documentationScoreClass = computed(() => {
  if (documentationScore.value >= 80) return 'excellent'
  if (documentationScore.value >= 60) return 'good'
  if (documentationScore.value >= 40) return 'fair'
  return 'needs-work'
})

const canSaveDocumentation = computed(() => {
  return documentation.overview.trim() && 
         documentation.requirements.trim() && 
         documentation.implementation.trim()
})

const generateTOC = () => {
  // Generate table of contents based on sections
  const sections = Object.keys(documentation)
    .filter(key => documentation[key].trim())
    .map(key => key.charAt(0).toUpperCase() + key.slice(1).replace(/([A-Z])/g, ' $1'))
  
  const toc = sections.map((section, index) => `${index + 1}. ${section}`).join('\n')
  
  // You could add this to a specific field or show in a modal
  console.log('Table of Contents:\n', toc)
}

const previewDocumentation = () => {
  // Open documentation preview
  router.push('/documentation/preview')
}

const saveDocumentation = async () => {
  try {
    await saveTechnicalDocumentation(documentation)
    toast.success('Documentation saved successfully')
  } catch (error) {
    if (error.response?.data?.errors) {
      docErrors.value = error.response.data.errors
    }
    toast.error('Failed to save documentation')
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
| `_c-label-container` | Container for label and hint |

## Best Practices

### Textarea Design
- Set appropriate initial height with `rows` attribute
- Allow vertical resizing for user preference
- Use meaningful placeholder text that guides content creation
- Consider maximum length constraints for specific use cases

### Content Management
- Implement auto-save functionality for long-form content
- Provide character/word count feedback for content limits
- Use helper text to guide users on expected content length
- Consider rich text editing for formatted content needs

### User Experience
- Provide visual feedback for content length and quality
- Use progressive enhancement for advanced editing features
- Implement keyboard shortcuts for common actions
- Consider mobile-friendly textarea sizing

### Accessibility
- Ensure proper label association with textarea elements
- Provide clear instructions for required vs optional fields
- Use ARIA labels for additional context when needed
- Test with screen readers and keyboard navigation

### Performance
- Implement debouncing for auto-save and validation
- Use efficient change detection for large text content
- Consider virtual scrolling for very long content
- Cache content locally to prevent data loss

### Validation
- Validate content length and format requirements
- Provide real-time feedback for content quality
- Handle both client-side and server-side validation
- Use meaningful error messages that guide improvement

### Content Quality
- Implement word/character counting for content guidelines
- Provide suggestions for content improvement
- Use progressive validation for content quality metrics
- Consider spell-check and grammar assistance integration

## Component Registration
```javascript
// Global registration
app.component('CodexTextAreaField', TextAreaField)

// Local registration  
import TextAreaField from '@/components/fields/TextAreaField.vue'

export default {
  components: {
    CodexTextAreaField: TextAreaField
  }
}
``` 