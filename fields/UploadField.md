# UploadField Component

## Overview
The UploadField component provides a file upload interface with drag-and-drop functionality, file type validation, and progress tracking. It wraps the codex-file-uploader component with form field capabilities including labels, hints, error handling, and validation. The component supports various file types, size restrictions, and provides a seamless file upload experience with visual feedback and accessibility features.

## Basic Usage
```vue
<template>
  <div class="upload-form">
    <codex-upload-field
      v-model="uploadedFile"
      :name="'document'"
      :label="'Upload Document'"
      :type="'file'"
      :file-type="'pdf'"
      :required="true"
    />
  </div>
</template>

<script setup>
const uploadedFile = ref(null)
</script>
```

## Key Features
- Drag-and-drop file upload interface
- File type validation and restrictions
- File size validation with customizable limits
- Progress tracking during upload
- Preview functionality for supported file types
- Multiple file upload support
- Form integration with validation and error handling
- Label and hint system with tooltip support
- Helper text and error messaging
- Accessibility features with proper ARIA labels
- Testing support with Dusk attributes
- Readonly mode for display-only scenarios

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `name` | `String` | `required` | Field name for form submission |
| `type` | `String` | `required` | Input type identifier |

### File Configuration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `fileType` | `String` | `undefined` | Allowed file type (pdf, image, video, etc.) |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the upload area |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |
| `placeholder` | `String` | `''` | Placeholder text for upload area |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'1'` | Layout size (auto, 4, 3, 2, 1) |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `required` | `Boolean` | `true` | Whether file upload is required |
| `readonly` | `Boolean` | `false` | Whether field is read-only |
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

The component uses `defineModel()` for file handling:

```vue
<template>
  <codex-upload-field v-model="uploadedFile" />
</template>

<script setup>
const uploadedFile = ref(null)
// uploadedFile contains the uploaded file object or file data
// Can be File object, base64 string, or file metadata
</script>
```

## File Type Support

The component supports various file types through the `fileType` prop:

```javascript
// Common file types
const fileTypes = {
  'pdf': 'PDF documents',
  'image': 'Images (jpg, png, gif, etc.)',
  'video': 'Video files',
  'audio': 'Audio files',
  'document': 'Office documents',
  'csv': 'CSV files',
  'excel': 'Excel spreadsheets'
}
```

## Examples

### Document Upload Form
```vue
<template>
  <div class="document-upload">
    <h3>Document Upload Portal</h3>
    
    <div class="upload-sections">
      <div class="upload-section">
        <h4>Required Documents</h4>
        
        <codex-upload-field
          v-model="documents.identification"
          :name="'identification'"
          :label="'Government-issued ID'"
          :type="'file'"
          :file-type="'image'"
          :layout="'2'"
          :required="true"
          :errors="documentErrors.identification"
          :helper-text="'Upload a clear photo of your driver\'s license or passport'"
        />
        
        <codex-upload-field
          v-model="documents.proofOfAddress"
          :name="'proof_of_address'"
          :label="'Proof of Address'"
          :type="'file'"
          :file-type="'pdf'"
          :layout="'2'"
          :required="true"
          :errors="documentErrors.proofOfAddress"
          :helper-text="'Utility bill or bank statement from the last 3 months'"
        />
        
        <codex-upload-field
          v-model="documents.resume"
          :name="'resume'"
          :label="'Resume/CV'"
          :type="'file'"
          :file-type="'pdf'"
          :layout="'1'"
          :required="true"
          :errors="documentErrors.resume"
          :helper-text="'PDF format preferred, maximum 5MB'"
        />
      </div>
      
      <div class="upload-section">
        <h4>Optional Documents</h4>
        
        <codex-upload-field
          v-model="documents.coverLetter"
          :name="'cover_letter'"
          :label="'Cover Letter'"
          :type="'file'"
          :file-type="'pdf'"
          :layout="'2'"
          :required="false"
          :helper-text="'Optional but recommended'"
        />
        
        <codex-upload-field
          v-model="documents.portfolio"
          :name="'portfolio'"
          :label="'Portfolio'"
          :type="'file'"
          :file-type="'pdf'"
          :layout="'2'"
          :required="false"
          :helper-text="'Work samples or portfolio PDF'"
        />
        
        <codex-upload-field
          v-model="documents.certifications"
          :name="'certifications'"
          :label="'Certifications'"
          :type="'file'"
          :file-type="'image'"
          :layout="'1'"
          :required="false"
          :helper-text="'Professional certifications or training certificates'"
        />
      </div>
    </div>
    
    <div class="upload-summary" v-if="hasUploadedDocuments">
      <h4>Upload Summary</h4>
      
      <div class="document-status">
        <div class="status-section">
          <h5>Required Documents</h5>
          <div class="document-list">
            <div v-for="doc in requiredDocuments" :key="doc.key" class="document-item" :class="{ uploaded: documents[doc.key] }">
              <div class="document-info">
                <i :class="documents[doc.key] ? 'ri-check-circle-line' : 'ri-time-line'"></i>
                <span class="document-name">{{ doc.name }}</span>
                <span v-if="documents[doc.key]" class="file-info">
                  {{ getFileInfo(documents[doc.key]) }}
                </span>
              </div>
              <div class="document-actions">
                <button v-if="documents[doc.key]" @click="previewDocument(doc.key)" class="preview-btn">
                  <i class="ri-eye-line"></i> Preview
                </button>
                <button v-if="documents[doc.key]" @click="removeDocument(doc.key)" class="remove-btn">
                  <i class="ri-delete-bin-line"></i> Remove
                </button>
              </div>
            </div>
          </div>
        </div>
        
        <div class="status-section" v-if="hasOptionalDocuments">
          <h5>Optional Documents</h5>
          <div class="document-list">
            <div v-for="doc in optionalDocuments" :key="doc.key" class="document-item" :class="{ uploaded: documents[doc.key] }">
              <div class="document-info">
                <i :class="documents[doc.key] ? 'ri-check-circle-line' : 'ri-circle-line'"></i>
                <span class="document-name">{{ doc.name }}</span>
                <span v-if="documents[doc.key]" class="file-info">
                  {{ getFileInfo(documents[doc.key]) }}
                </span>
              </div>
              <div class="document-actions">
                <button v-if="documents[doc.key]" @click="previewDocument(doc.key)" class="preview-btn">
                  <i class="ri-eye-line"></i> Preview
                </button>
                <button v-if="documents[doc.key]" @click="removeDocument(doc.key)" class="remove-btn">
                  <i class="ri-delete-bin-line"></i> Remove
                </button>
              </div>
            </div>
          </div>
        </div>
        
        <div class="upload-statistics">
          <div class="stat-item">
            <span class="stat-value">{{ totalUploadedCount }}</span>
            <span class="stat-label">Total Documents</span>
          </div>
          <div class="stat-item">
            <span class="stat-value">{{ requiredCompletionPercentage }}%</span>
            <span class="stat-label">Required Complete</span>
          </div>
          <div class="stat-item">
            <span class="stat-value">{{ formatFileSize(totalUploadSize) }}</span>
            <span class="stat-label">Total Size</span>
          </div>
        </div>
      </div>
    </div>
    
    <div class="upload-actions">
      <button @click="clearAllDocuments" class="clear-btn">
        Clear All Documents
      </button>
      <button @click="uploadAllDocuments" :disabled="!canUploadDocuments" class="upload-btn">
        Submit Documents
      </button>
    </div>
  </div>
</template>

<script setup>
const documents = reactive({
  identification: null,
  proofOfAddress: null,
  resume: null,
  coverLetter: null,
  portfolio: null,
  certifications: null
})

const documentErrors = ref({
  identification: [],
  proofOfAddress: [],
  resume: []
})

const requiredDocuments = [
  { key: 'identification', name: 'Government-issued ID' },
  { key: 'proofOfAddress', name: 'Proof of Address' },
  { key: 'resume', name: 'Resume/CV' }
]

const optionalDocuments = [
  { key: 'coverLetter', name: 'Cover Letter' },
  { key: 'portfolio', name: 'Portfolio' },
  { key: 'certifications', name: 'Certifications' }
]

const hasUploadedDocuments = computed(() => {
  return Object.values(documents).some(doc => doc !== null)
})

const hasOptionalDocuments = computed(() => {
  return optionalDocuments.some(doc => documents[doc.key] !== null)
})

const totalUploadedCount = computed(() => {
  return Object.values(documents).filter(doc => doc !== null).length
})

const requiredCompletionPercentage = computed(() => {
  const completedRequired = requiredDocuments.filter(doc => documents[doc.key] !== null).length
  return Math.round((completedRequired / requiredDocuments.length) * 100)
})

const totalUploadSize = computed(() => {
  return Object.values(documents)
    .filter(doc => doc !== null)
    .reduce((total, doc) => total + (doc.size || 0), 0)
})

const canUploadDocuments = computed(() => {
  return requiredDocuments.every(doc => documents[doc.key] !== null)
})

const getFileInfo = (file) => {
  if (!file) return ''
  return `${file.name} (${formatFileSize(file.size)})`
}

const formatFileSize = (bytes) => {
  if (bytes === 0) return '0 B'
  const k = 1024
  const sizes = ['B', 'KB', 'MB', 'GB']
  const i = Math.floor(Math.log(bytes) / Math.log(k))
  return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i]
}

const previewDocument = (documentKey) => {
  const file = documents[documentKey]
  if (file) {
    // Open file preview in modal or new tab
    const url = URL.createObjectURL(file)
    window.open(url, '_blank')
  }
}

const removeDocument = (documentKey) => {
  documents[documentKey] = null
  toast.info(`${requiredDocuments.find(d => d.key === documentKey)?.name || optionalDocuments.find(d => d.key === documentKey)?.name} removed`)
}

const clearAllDocuments = () => {
  Object.keys(documents).forEach(key => {
    documents[key] = null
  })
  toast.info('All documents cleared')
}

const uploadAllDocuments = async () => {
  try {
    const formData = new FormData()
    
    Object.keys(documents).forEach(key => {
      if (documents[key]) {
        formData.append(key, documents[key])
      }
    })
    
    await submitDocuments(formData)
    toast.success('Documents uploaded successfully!')
    router.push('/application/review')
  } catch (error) {
    if (error.response?.data?.errors) {
      documentErrors.value = error.response.data.errors
    }
    toast.error('Failed to upload documents')
  }
}
</script>
```

### Media Upload Gallery
```vue
<template>
  <div class="media-upload">
    <h3>Media Upload Gallery</h3>
    
    <div class="upload-categories">
      <div class="upload-category">
        <h4>Product Images</h4>
        <p>High-quality images of your products</p>
        
        <codex-upload-field
          v-model="media.productImages"
          :name="'product_images'"
          :label="'Product Photos'"
          :type="'file'"
          :file-type="'image'"
          :layout="'1'"
          :required="true"
          :errors="mediaErrors.productImages"
          :helper-text="'Upload clear, well-lit photos of your products. Multiple images allowed.'"
        />
      </div>
      
      <div class="upload-category">
        <h4>Marketing Materials</h4>
        <p>Promotional content and brand assets</p>
        
        <codex-upload-field
          v-model="media.logo"
          :name="'logo'"
          :label="'Company Logo'"
          :type="'file'"
          :file-type="'image'"
          :layout="'2'"
          :required="true"
          :errors="mediaErrors.logo"
          :helper-text="'PNG or SVG format preferred for best quality'"
        />
        
        <codex-upload-field
          v-model="media.bannerImage"
          :name="'banner_image'"
          :label="'Banner Image'"
          :type="'file'"
          :file-type="'image'"
          :layout="'2'"
          :required="false"
          :helper-text="'Header image for your profile page'"
        />
        
        <codex-upload-field
          v-model="media.brochure"
          :name="'brochure'"
          :label="'Marketing Brochure'"
          :type="'file'"
          :file-type="'pdf'"
          :layout="'1'"
          :required="false"
          :helper-text="'PDF brochure or catalog'"
        />
      </div>
      
      <div class="upload-category">
        <h4>Video Content</h4>
        <p>Product demonstrations and promotional videos</p>
        
        <codex-upload-field
          v-model="media.productVideo"
          :name="'product_video'"
          :label="'Product Demo Video'"
          :type="'file'"
          :file-type="'video'"
          :layout="'1'"
          :required="false"
          :helper-text="'Show your product in action. Max 100MB.'"
        />
        
        <codex-upload-field
          v-model="media.testimonialVideo"
          :name="'testimonial_video'"
          :label="'Customer Testimonial'"
          :type="'file'"
          :file-type="'video'"
          :layout="'2'"
          :required="false"
          :helper-text="'Customer reviews and testimonials'"
        />
      </div>
    </div>
    
    <div class="media-gallery" v-if="hasUploadedMedia">
      <h4>Uploaded Media</h4>
      
      <div class="gallery-grid">
        <div v-for="(file, key) in uploadedFiles" :key="key" class="gallery-item">
          <div class="gallery-preview">
            <div v-if="isImage(file)" class="image-preview">
              <img :src="getFilePreview(file)" :alt="getFileName(key)" />
            </div>
            <div v-else-if="isVideo(file)" class="video-preview">
              <video controls>
                <source :src="getFilePreview(file)" :type="file.type" />
              </video>
            </div>
            <div v-else class="file-preview">
              <i class="ri-file-line"></i>
              <span>{{ getFileName(key) }}</span>
            </div>
          </div>
          
          <div class="gallery-info">
            <h6>{{ getFileDisplayName(key) }}</h6>
            <div class="file-details">
              <span class="file-size">{{ formatFileSize(file.size) }}</span>
              <span class="file-type">{{ file.type }}</span>
            </div>
            <div class="gallery-actions">
              <button @click="downloadFile(key, file)" class="download-btn">
                <i class="ri-download-line"></i>
              </button>
              <button @click="replaceFile(key)" class="replace-btn">
                <i class="ri-refresh-line"></i>
              </button>
              <button @click="removeFile(key)" class="remove-btn">
                <i class="ri-delete-bin-line"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
      
      <div class="gallery-stats">
        <div class="stat-group">
          <div class="stat-item">
            <span class="stat-value">{{ totalFilesCount }}</span>
            <span class="stat-label">Total Files</span>
          </div>
          <div class="stat-item">
            <span class="stat-value">{{ imageFilesCount }}</span>
            <span class="stat-label">Images</span>
          </div>
          <div class="stat-item">
            <span class="stat-value">{{ videoFilesCount }}</span>
            <span class="stat-label">Videos</span>
          </div>
          <div class="stat-item">
            <span class="stat-value">{{ formatFileSize(totalMediaSize) }}</span>
            <span class="stat-label">Total Size</span>
          </div>
        </div>
        
        <div class="upload-progress" v-if="uploadProgress > 0 && uploadProgress < 100">
          <div class="progress-bar">
            <div class="progress-fill" :style="{ width: uploadProgress + '%' }"></div>
          </div>
          <span class="progress-text">{{ uploadProgress }}% uploaded</span>
        </div>
      </div>
    </div>
    
    <div class="media-actions">
      <button @click="clearAllMedia" class="clear-btn">
        Clear All Media
      </button>
      <button @click="optimizeImages" :disabled="!hasImages" class="optimize-btn">
        Optimize Images
      </button>
      <button @click="uploadMedia" :disabled="!canUploadMedia" class="upload-btn">
        Upload Media
      </button>
    </div>
  </div>
</template>

<script setup>
const media = reactive({
  productImages: null,
  logo: null,
  bannerImage: null,
  brochure: null,
  productVideo: null,
  testimonialVideo: null
})

const mediaErrors = ref({
  productImages: [],
  logo: []
})

const uploadProgress = ref(0)

const hasUploadedMedia = computed(() => {
  return Object.values(media).some(file => file !== null)
})

const uploadedFiles = computed(() => {
  const files = {}
  Object.keys(media).forEach(key => {
    if (media[key]) {
      files[key] = media[key]
    }
  })
  return files
})

const totalFilesCount = computed(() => {
  return Object.values(uploadedFiles.value).length
})

const imageFilesCount = computed(() => {
  return Object.values(uploadedFiles.value).filter(file => isImage(file)).length
})

const videoFilesCount = computed(() => {
  return Object.values(uploadedFiles.value).filter(file => isVideo(file)).length
})

const totalMediaSize = computed(() => {
  return Object.values(uploadedFiles.value).reduce((total, file) => total + file.size, 0)
})

const hasImages = computed(() => {
  return imageFilesCount.value > 0
})

const canUploadMedia = computed(() => {
  return media.productImages !== null && media.logo !== null
})

const isImage = (file) => {
  return file && file.type.startsWith('image/')
}

const isVideo = (file) => {
  return file && file.type.startsWith('video/')
}

const getFilePreview = (file) => {
  return URL.createObjectURL(file)
}

const getFileName = (key) => {
  return media[key]?.name || ''
}

const getFileDisplayName = (key) => {
  const displayNames = {
    productImages: 'Product Images',
    logo: 'Company Logo',
    bannerImage: 'Banner Image',
    brochure: 'Marketing Brochure',
    productVideo: 'Product Demo Video',
    testimonialVideo: 'Customer Testimonial'
  }
  return displayNames[key] || key
}

const formatFileSize = (bytes) => {
  if (bytes === 0) return '0 B'
  const k = 1024
  const sizes = ['B', 'KB', 'MB', 'GB']
  const i = Math.floor(Math.log(bytes) / Math.log(k))
  return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i]
}

const downloadFile = (key, file) => {
  const url = URL.createObjectURL(file)
  const a = document.createElement('a')
  a.href = url
  a.download = file.name
  a.click()
  URL.revokeObjectURL(url)
}

const replaceFile = (key) => {
  // Trigger file input for replacement
  // This would typically open the file picker for that specific field
  toast.info(`Click the upload area to replace ${getFileDisplayName(key)}`)
}

const removeFile = (key) => {
  media[key] = null
  toast.info(`${getFileDisplayName(key)} removed`)
}

const clearAllMedia = () => {
  Object.keys(media).forEach(key => {
    media[key] = null
  })
  uploadProgress.value = 0
  toast.info('All media cleared')
}

const optimizeImages = async () => {
  try {
    toast.info('Optimizing images...')
    // Image optimization logic would go here
    await new Promise(resolve => setTimeout(resolve, 2000))
    toast.success('Images optimized successfully!')
  } catch (error) {
    toast.error('Failed to optimize images')
  }
}

const uploadMedia = async () => {
  try {
    uploadProgress.value = 0
    const formData = new FormData()
    
    Object.keys(media).forEach(key => {
      if (media[key]) {
        formData.append(key, media[key])
      }
    })
    
    // Simulate upload progress
    const uploadInterval = setInterval(() => {
      uploadProgress.value += 10
      if (uploadProgress.value >= 100) {
        clearInterval(uploadInterval)
      }
    }, 200)
    
    await submitMediaFiles(formData)
    uploadProgress.value = 100
    
    toast.success('Media uploaded successfully!')
    router.push('/media/gallery')
  } catch (error) {
    uploadProgress.value = 0
    if (error.response?.data?.errors) {
      mediaErrors.value = error.response.data.errors
    }
    toast.error('Failed to upload media')
  }
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input-container` | Main container for the upload field |
| `_c-form-field--auto` | Auto-sized layout |
| `_c-form-field--quarter` | Quarter width layout (25%) |
| `_c-form-field--third` | Third width layout (33%) |
| `_c-form-field--half` | Half width layout (50%) |
| `_c-form-field--full` | Full width layout (100%) |
| `_c-label-container` | Container for label and hint |

## Best Practices

### File Validation
- Implement client-side file type validation before upload
- Set appropriate file size limits for different file types
- Validate file extensions and MIME types for security
- Provide clear error messages for validation failures
- Consider server-side validation as the primary security measure

### User Experience
- Provide drag-and-drop functionality for intuitive uploads
- Show upload progress for large files
- Display file previews when possible
- Allow file replacement and removal
- Provide clear instructions for file requirements

### Security
- Never trust client-side validation alone
- Scan uploaded files for malware
- Store uploaded files outside web-accessible directories
- Use secure file naming conventions
- Implement proper access controls for uploaded files

### Performance
- Implement file compression for images when appropriate
- Use lazy loading for file previews
- Consider chunked uploads for large files
- Optimize file storage and delivery
- Implement proper caching strategies

### Accessibility
- Provide proper ARIA labels for screen readers
- Support keyboard navigation for upload interactions
- Offer alternative upload methods for users with disabilities
- Ensure sufficient color contrast for upload states
- Provide clear instructions and feedback

### Error Handling
- Handle network errors gracefully during upload
- Provide retry mechanisms for failed uploads
- Show clear error messages with actionable guidance
- Implement proper timeout handling
- Log upload errors for debugging

### Mobile Optimization
- Ensure upload interface works well on touch devices
- Optimize for slower mobile connections
- Consider mobile-specific file size limits
- Provide mobile-friendly file preview options
- Test upload functionality across different mobile browsers

## Component Registration
```javascript
// Global registration
app.component('CodexUploadField', UploadField)

// Local registration  
import UploadField from '@/components/fields/UploadField.vue'

export default {
  components: {
    CodexUploadField: UploadField
  }
}
``` 