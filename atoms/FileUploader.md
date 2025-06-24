# FileUploader Component

## Overview
The FileUploader component provides a comprehensive file upload interface built on FilePond with support for both image and PDF file types. It features drag-and-drop functionality, file validation, preview capabilities, instant upload processing, progress tracking, and server-side integration with automatic file processing and storage management.

## Basic Usage
```vue
<!-- Must be used within a provider that injects file upload configuration -->
<template>
  <div>
    <!-- Provider component that injects file upload configuration -->
    <file-provider
      :placeholder="'Drag and drop your file here'"
      :name="'document_upload'"
      :id="'file-uploader'"
      :required="true"
      :readonly="false"
      :has-error="false"
    >
      <codex-file-uploader 
        v-model="uploadedFileId"
        :name="'document'"
        :type="'pdf'"
      />
    </file-provider>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const uploadedFileId = ref('')
</script>
```

## Key Features
- Drag-and-drop file upload interface
- Support for image and PDF file types
- Real-time file validation and type checking
- File preview capabilities (images and PDFs)
- Instant upload with progress indicators
- Server-side processing integration
- Maximum file size enforcement (5MB)
- Single file upload limitation
- Error handling and validation feedback
- Dependency injection for form integration

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `modelValue` | `Array` | `[]` | Two-way binding for uploaded files |
| `name` | `String` | `required` | Form field name for the upload |
| `type` | `String` | `'image'` | File type filter ('image' or 'pdf') |

## Common Props (Injected)

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `placeholder` | `String` | `''` | Upload area placeholder text |
| `name` | `String` | `''` | Form field name attribute |
| `id` | `String` | `''` | Unique identifier for the uploader |
| `required` | `Boolean` | `false` | Whether file upload is required |
| `readonly` | `Boolean` | `false` | Whether uploader is read-only |
| `hasError` | `Boolean` | `false` | External error state from parent |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `update:modelValue` | `string` | Emitted with server file ID after successful upload |
| `updatefiles` | `Array` | Emitted when file list changes (internal) |
| `processfile` | `Object` | Emitted when file processing completes |

## Slots
This component does not provide any slots.

## States

### Default State (Image Upload)
```vue
<codex-file-uploader 
  v-model="imageFile"
  :name="'profile_image'"
  :type="'image'"
/>
```

### PDF Upload State
```vue
<codex-file-uploader 
  v-model="documentFile"
  :name="'document'"
  :type="'pdf'"
/>
```

### Error State
```vue
<file-provider :has-error="true">
  <codex-file-uploader 
    v-model="errorFile"
    :name="'failed_upload'"
  />
</file-provider>
```

### Required State
```vue
<file-provider :required="true">
  <codex-file-uploader 
    v-model="requiredFile"
    :name="'required_document'"
  />
</file-provider>
```

## File Upload Configuration

### Server Endpoint
The component automatically uploads to: `${window.codex.api}/api/v1/customer/uploads/process`

### File Constraints
- **Maximum file size**: 5MB (5,000,000 bytes)
- **Maximum files**: 1 file at a time
- **Accepted file types**: 
  - Images: `image/*` (when type="image")
  - PDFs: `application/pdf` (when type="pdf")

## Examples

### Basic Image Upload
```vue
<template>
  <div class="profile-section">
    <h3>Profile Picture</h3>
    <file-provider 
      :placeholder="'Upload your profile picture'"
      :name="'profile_image'"
      :required="true"
    >
      <codex-file-uploader 
        v-model="profileImage"
        :name="'profile_image'"
        :type="'image'"
      />
    </file-provider>
    
    <p v-if="profileImage" class="success">
      Image uploaded successfully!
    </p>
  </div>
</template>

<script setup>
const profileImage = ref('')
</script>
```

### Document Upload with Validation
```vue
<template>
  <div class="document-upload">
    <h3>Upload Document</h3>
    <file-provider 
      :placeholder="'Upload PDF document (max 5MB)'"
      :name="'legal_document'"
      :has-error="uploadError"
      :required="true"
    >
      <codex-file-uploader 
        v-model="documentId"
        :name="'legal_document'"
        :type="'pdf'"
        @update:modelValue="handleUploadSuccess"
      />
    </file-provider>
    
    <div v-if="uploadError" class="error-message">
      <p>Upload failed. Please try again.</p>
      <button @click="resetUpload">Reset</button>
    </div>
  </div>
</template>

<script setup>
const documentId = ref('')
const uploadError = ref(false)

const handleUploadSuccess = (fileId) => {
  uploadError.value = false
  console.log('Document uploaded:', fileId)
}

const resetUpload = () => {
  documentId.value = ''
  uploadError.value = false
}
</script>
```

### Multi-Document Upload Form
```vue
<template>
  <div class="document-form">
    <h2>Document Upload Form</h2>
    
    <div class="upload-section">
      <label>ID Document</label>
      <file-provider 
        :placeholder="'Upload ID (image or PDF)'"
        :name="'id_document'"
        :required="true"
      >
        <codex-file-uploader 
          v-model="idDocument"
          :name="'id_document'"
          :type="'image'"
        />
      </file-provider>
    </div>
    
    <div class="upload-section">
      <label>Proof of Address</label>
      <file-provider 
        :placeholder="'Upload proof of address (PDF only)'"
        :name="'proof_address'"
        :required="true"
      >
        <codex-file-uploader 
          v-model="proofAddress"
          :name="'proof_address'"
          :type="'pdf'"
        />
      </file-provider>
    </div>
    
    <div class="upload-section">
      <label>Additional Documents (Optional)</label>
      <file-provider 
        :placeholder="'Upload additional documents'"
        :name="'additional_docs'"
        :required="false"
      >
        <codex-file-uploader 
          v-model="additionalDocs"
          :name="'additional_docs'"
          :type="'pdf'"
        />
      </file-provider>
    </div>
    
    <button 
      @click="submitForm" 
      :disabled="!canSubmit"
      class="submit-button"
    >
      Submit Documents
    </button>
  </div>
</template>

<script setup>
const idDocument = ref('')
const proofAddress = ref('')
const additionalDocs = ref('')

const canSubmit = computed(() => {
  return idDocument.value && proofAddress.value
})

const submitForm = async () => {
  try {
    const formData = {
      id_document: idDocument.value,
      proof_address: proofAddress.value,
      additional_docs: additionalDocs.value
    }
    
    // Submit to your API
    await submitDocuments(formData)
    
    // Handle success
    console.log('Documents submitted successfully')
  } catch (error) {
    console.error('Submission failed:', error)
  }
}
</script>
```

### Avatar Upload with Preview
```vue
<template>
  <div class="avatar-upload">
    <div class="current-avatar" v-if="currentAvatar">
      <img :src="currentAvatar" alt="Current avatar" />
      <p>Current Avatar</p>
    </div>
    
    <file-provider 
      :placeholder="'Upload new avatar image'"
      :name="'avatar'"
      :has-error="avatarError"
    >
      <codex-file-uploader 
        v-model="newAvatar"
        :name="'avatar'"
        :type="'image'"
        @update:modelValue="handleAvatarUpload"
      />
    </file-provider>
    
    <div class="upload-actions">
      <button @click="saveAvatar" :disabled="!newAvatar">
        Save New Avatar
      </button>
      <button @click="cancelUpload" v-if="newAvatar">
        Cancel
      </button>
    </div>
  </div>
</template>

<script setup>
const currentAvatar = ref('/path/to/current/avatar.jpg')
const newAvatar = ref('')
const avatarError = ref(false)

const handleAvatarUpload = (fileId) => {
  avatarError.value = false
  console.log('New avatar uploaded:', fileId)
}

const saveAvatar = async () => {
  try {
    await updateUserAvatar(newAvatar.value)
    currentAvatar.value = `/api/files/${newAvatar.value}`
    newAvatar.value = ''
  } catch (error) {
    avatarError.value = true
    console.error('Failed to save avatar:', error)
  }
}

const cancelUpload = () => {
  newAvatar.value = ''
  avatarError.value = false
}
</script>
```

### File Upload with Progress
```vue
<template>
  <div class="upload-with-progress">
    <file-provider 
      :placeholder="'Upload your file'"
      :name="'file_upload'"
    >
      <codex-file-uploader 
        v-model="uploadedFile"
        :name="'file_upload'"
        :type="'pdf'"
        ref="fileUploader"
      />
    </file-provider>
    
    <div v-if="uploadProgress.show" class="upload-status">
      <div class="progress-bar">
        <div 
          class="progress-fill" 
          :style="{ width: uploadProgress.percent + '%' }"
        ></div>
      </div>
      <p>{{ uploadProgress.message }}</p>
    </div>
  </div>
</template>

<script setup>
const uploadedFile = ref('')
const fileUploader = ref(null)
const uploadProgress = ref({
  show: false,
  percent: 0,
  message: ''
})

// Monitor upload progress through FilePond events
onMounted(() => {
  if (fileUploader.value) {
    const pond = fileUploader.value.$refs.pond
    
    pond.onaddfile = () => {
      uploadProgress.value = {
        show: true,
        percent: 0,
        message: 'Starting upload...'
      }
    }
    
    pond.onprocessfileprogress = (file, progress) => {
      uploadProgress.value = {
        show: true,
        percent: Math.round(progress * 100),
        message: `Uploading... ${Math.round(progress * 100)}%`
      }
    }
    
    pond.onprocessfile = () => {
      uploadProgress.value = {
        show: true,
        percent: 100,
        message: 'Upload complete!'
      }
      
      setTimeout(() => {
        uploadProgress.value.show = false
      }, 2000)
    }
  }
})
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input` | Base input styling applied to FilePond wrapper |
| `_c-error` | Error state styling |
| `filepond--wrapper` | FilePond wrapper styling |
| `filepond--drop-label` | Drop area label styling |

## Best Practices

### Accessibility
- Provide clear upload instructions in placeholder text
- Use semantic labeling for screen readers
- Ensure keyboard navigation support
- Test with assistive technologies
- Provide alternative upload methods if needed

### File Validation
- Validate file types on both client and server side
- Enforce reasonable file size limits
- Provide clear error messages for validation failures
- Handle network errors gracefully
- Implement retry mechanisms for failed uploads

### User Experience
- Show upload progress indicators
- Provide file preview when possible
- Allow users to cancel uploads
- Give clear feedback on success/failure
- Use appropriate file type icons

### Security
- Validate file types server-side
- Scan uploaded files for malware
- Implement file size restrictions
- Use secure file storage solutions
- Sanitize file names and metadata

### Performance
- Implement file compression when appropriate
- Use chunked uploads for large files
- Optimize image uploads with resizing
- Cache uploaded files appropriately
- Monitor upload bandwidth usage

### Error Handling
- Handle network connectivity issues
- Provide retry options for failed uploads
- Clear error states when resolved
- Log upload errors for debugging
- Implement fallback upload methods

### Form Integration
- Integrate with form validation systems
- Handle form submission with file uploads
- Validate required file uploads
- Clear uploaded files on form reset
- Maintain upload state across form interactions

## Component Registration
```javascript
// Global registration
app.component('CodexFileUploader', FileUploader)

// Local registration  
import FileUploader from '@/components/atoms/FileUploader.vue'

export default {
  components: {
    CodexFileUploader: FileUploader
  }
}
``` 