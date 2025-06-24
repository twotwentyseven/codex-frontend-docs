# Instructor Component

## Overview
The Instructor component provides a comprehensive instructor profile display with detailed information, image galleries, social links, and upcoming class listings. It features loading states with skeleton animations, error handling, bookmark functionality, and integration with the event listing system. The component supports both prop-based data input and automatic data loading via handle or URL parameters, making it flexible for various use cases.

## Basic Usage
```vue
<template>
  <div class="instructor-profile-example">
    <codex-instructor 
      :instructor="instructorData"
      :show-related="true"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'

const instructorData = {
  id: 1,
  handle: 'sarah-johnson',
  first_name: 'Sarah',
  last_name: 'Johnson',
  photo: '/images/instructors/sarah-johnson.jpg',
  biography: '<p>Sarah is a certified yoga instructor with over 10 years of experience in Hatha and Vinyasa yoga. She believes in creating a supportive and inclusive environment for all students.</p>',
  tags: ['Yoga', 'Meditation', 'Wellness'],
  social_links: [
    { type: 'instagram', label: '@sarahyoga', url: 'https://instagram.com/sarahyoga' },
    { type: 'facebook', label: 'Sarah Johnson Yoga', url: 'https://facebook.com/sarahjohnsonyoga' }
  ],
  images: [
    '/images/instructors/sarah-gallery-1.jpg',
    '/images/instructors/sarah-gallery-2.jpg'
  ]
}
</script>
```

## Key Features
- Comprehensive instructor profile display
- Loading states with skeleton animations
- Error handling with customizable error messages
- Automatic data loading via handle or URL parameters
- Integration with event listing system
- Social media links display
- Image gallery support
- Related instructors section
- Bookmark functionality integration
- Responsive grid layout

## Configuration Props

### Core Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `instructor` | `Object|Number` | `undefined` | Instructor data object or ID |
| `handle` | `String` | `undefined` | Instructor handle for data loading |
| `showRelated` | `Boolean` | `true` | Display related instructors section |

### Common Props
Document which common props from `@/config/common` are used:
| Prop Name | Usage |
|-----------|-------|
| `showPoweredBy` | Controls "Powered by" display across sections |

### Common Functions
Document which utilities from `useCommon` are used:
| Function | Usage |
|----------|-------|
| `handleClick` | Handles click interactions |
| `filteredTags` | Filters and displays instructor tags |
| `loadingItems` | Provides skeleton loading items |

## Slots

### Layout Slots
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ instructor, loading, error, genericErrors }` | Custom header content (instructor details) |
| `content` | `{ instructor, loading, error, genericErrors }` | Custom content area (event listings) |
| `footer` | `{ instructor, loading, error, genericErrors }` | Custom footer content (related instructors) |

### Error Slot
| Slot | Props | Description |
|------|-------|-------------|
| `error-messages` | `{ error, genericErrors }` | Custom error display |

## Instructor Data Structure

The component expects the instructor object to have the following structure:

```javascript
{
  id: Number,                    // Unique instructor ID
  handle: String,               // URL-friendly identifier
  first_name: String,           // Instructor's first name
  last_name: String,            // Instructor's last name
  photo: String,                // Primary photo URL
  biography: String,            // HTML biography content
  tags: Array,                  // Array of tag strings
  social_links: [               // Social media links
    {
      type: String,             // Platform type (instagram, facebook, etc.)
      label: String,            // Display label
      url: String               // Link URL
    }
  ],
  images: Array,                // Additional gallery images
  locations: Array              // Associated locations (optional)
}
```

## Data Loading Behavior

The component automatically loads instructor data when:
1. No `instructor` prop is provided
2. Uses `handle` prop if available
3. Falls back to URL query parameter `instructor`
4. Finally uses the last segment of the URL pathname

## Examples

### Complete Instructor Profile
```vue
<template>
  <div class="instructor-profile-complete">
    <codex-instructor 
      :instructor="instructor"
      :show-related="true"
    >
      <template #header="{ instructor, loading, error }">
        <div v-if="loading" class="loading-state">
          <div class="loading-skeleton">
            <div class="skeleton-avatar"></div>
            <div class="skeleton-content">
              <div class="skeleton-name"></div>
              <div class="skeleton-bio"></div>
              <div class="skeleton-tags"></div>
            </div>
          </div>
        </div>
        
        <div v-else-if="error" class="error-state">
          <div class="error-content">
            <i class="ri-error-warning-line"></i>
            <h3>Unable to load instructor</h3>
            <p>{{ error.message || 'Please try again later.' }}</p>
            <button @click="retryLoad" class="retry-btn">
              <i class="ri-refresh-line"></i>
              Retry
            </button>
          </div>
        </div>
        
        <div v-else-if="instructor" class="instructor-header">
          <div class="instructor-images">
            <div class="primary-image">
              <img 
                v-if="instructor.photo"
                :src="instructor.photo"
                :alt="`${instructor.first_name} ${instructor.last_name}`"
                class="instructor-photo"
              >
              <div v-else class="photo-placeholder">
                <i class="ri-user-line"></i>
              </div>
            </div>
            
            <div v-if="instructor.images?.length" class="gallery-images">
              <img 
                v-for="(image, index) in instructor.images.slice(0, 3)"
                :key="index"
                :src="image"
                :alt="`${instructor.first_name} ${instructor.last_name} - Image ${index + 1}`"
                class="gallery-image"
                @click="openGallery(index)"
              >
              <div 
                v-if="instructor.images.length > 3"
                class="more-images"
                @click="openGallery(3)"
              >
                +{{ instructor.images.length - 3 }}
              </div>
            </div>
          </div>
          
          <div class="instructor-details">
            <div class="instructor-name-section">
              <h1 class="instructor-name">
                {{ instructor.first_name }} {{ instructor.last_name }}
              </h1>
              
              <div class="instructor-actions">
                <button 
                  @click="shareInstructor"
                  class="action-btn share-btn"
                  title="Share instructor"
                >
                  <i class="ri-share-line"></i>
                  Share
                </button>
                
                <codex-bookmark 
                  type="instructor" 
                  :item="instructor.id"
                  class="bookmark-action"
                />
              </div>
            </div>
            
            <div 
              v-if="instructor.biography"
              class="instructor-biography"
              v-html="instructor.biography"
            ></div>
            
            <div v-if="instructor.tags?.length" class="instructor-tags">
              <h3>Specialties</h3>
              <div class="tag-list">
                <span 
                  v-for="tag in instructor.tags"
                  :key="tag"
                  class="instructor-tag"
                >
                  {{ tag }}
                </span>
              </div>
            </div>
            
            <div v-if="instructor.social_links?.length" class="social-links">
              <h3>Connect with {{ instructor.first_name }}</h3>
              <div class="social-list">
                <a 
                  v-for="social in instructor.social_links"
                  :key="social.url"
                  :href="social.url"
                  :title="social.label"
                  class="social-link"
                  target="_blank"
                  rel="noopener noreferrer"
                >
                  <i :class="`ri-${social.type}-fill`"></i>
                  <span>{{ social.label }}</span>
                </a>
              </div>
            </div>
          </div>
        </div>
      </template>
    </codex-instructor>
    
    <!-- Image Gallery Modal -->
    <div v-if="galleryOpen" class="gallery-modal" @click="closeGallery">
      <div class="gallery-content" @click.stop>
        <button @click="closeGallery" class="gallery-close">
          <i class="ri-close-line"></i>
        </button>
        
        <div class="gallery-main">
          <img 
            :src="instructor.images[currentImageIndex]"
            :alt="`${instructor.first_name} ${instructor.last_name} - Gallery Image`"
            class="gallery-main-image"
          >
        </div>
        
        <div class="gallery-controls">
          <button 
            @click="previousImage"
            :disabled="currentImageIndex === 0"
            class="gallery-btn"
          >
            <i class="ri-arrow-left-line"></i>
          </button>
          
          <span class="image-counter">
            {{ currentImageIndex + 1 }} / {{ instructor.images.length }}
          </span>
          
          <button 
            @click="nextImage"
            :disabled="currentImageIndex === instructor.images.length - 1"
            class="gallery-btn"
          >
            <i class="ri-arrow-right-line"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const instructor = ref({
  id: 1,
  handle: 'sarah-johnson',
  first_name: 'Sarah',
  last_name: 'Johnson',
  photo: '/images/instructors/sarah-johnson.jpg',
  biography: `
    <p>Sarah is a certified yoga instructor with over 10 years of experience in Hatha and Vinyasa yoga. She believes in creating a supportive and inclusive environment for all students, regardless of their experience level.</p>
    
    <p>Her teaching philosophy centers around mindfulness, breath awareness, and the connection between mind and body. Sarah has completed her 500-hour RYT certification and continues to study with master teachers around the world.</p>
    
    <p>When she's not teaching, Sarah enjoys hiking, meditation, and spending time with her rescue dog, Luna.</p>
  `,
  tags: ['Hatha Yoga', 'Vinyasa', 'Meditation', 'Breathwork', 'Mindfulness'],
  social_links: [
    {
      type: 'instagram',
      label: '@sarahyoga',
      url: 'https://instagram.com/sarahyoga'
    },
    {
      type: 'facebook',
      label: 'Sarah Johnson Yoga',
      url: 'https://facebook.com/sarahjohnsonyoga'
    },
    {
      type: 'youtube',
      label: 'Sarah\'s Yoga Channel',
      url: 'https://youtube.com/sarahyoga'
    }
  ],
  images: [
    '/images/instructors/sarah-gallery-1.jpg',
    '/images/instructors/sarah-gallery-2.jpg',
    '/images/instructors/sarah-gallery-3.jpg',
    '/images/instructors/sarah-gallery-4.jpg',
    '/images/instructors/sarah-gallery-5.jpg'
  ]
})

const galleryOpen = ref(false)
const currentImageIndex = ref(0)

const openGallery = (index) => {
  currentImageIndex.value = index
  galleryOpen.value = true
  document.body.style.overflow = 'hidden'
}

const closeGallery = () => {
  galleryOpen.value = false
  document.body.style.overflow = 'auto'
}

const nextImage = () => {
  if (currentImageIndex.value < instructor.value.images.length - 1) {
    currentImageIndex.value++
  }
}

const previousImage = () => {
  if (currentImageIndex.value > 0) {
    currentImageIndex.value--
  }
}

const shareInstructor = async () => {
  if (navigator.share) {
    try {
      await navigator.share({
        title: `${instructor.value.first_name} ${instructor.value.last_name} - Instructor`,
        text: 'Check out this amazing instructor!',
        url: window.location.href
      })
    } catch (error) {
      console.log('Share cancelled')
    }
  } else {
    // Fallback to copying URL
    navigator.clipboard.writeText(window.location.href)
    alert('Link copied to clipboard!')
  }
}

const retryLoad = () => {
  window.location.reload()
}
</script>

<style scoped>
.instructor-profile-complete {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.loading-skeleton {
  display: flex;
  gap: 2rem;
  padding: 2rem;
}

.skeleton-avatar {
  width: 200px;
  height: 200px;
  background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
  animation: loading 1.5s infinite;
  border-radius: 8px;
}

.skeleton-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.skeleton-name,
.skeleton-bio,
.skeleton-tags {
  background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
  animation: loading 1.5s infinite;
  border-radius: 4px;
}

.skeleton-name {
  height: 2rem;
  width: 300px;
}

.skeleton-bio {
  height: 4rem;
  width: 100%;
}

.skeleton-tags {
  height: 1.5rem;
  width: 250px;
}

.error-state {
  padding: 4rem 2rem;
  text-align: center;
}

.error-content i {
  font-size: 3rem;
  color: #dc3545;
  margin-bottom: 1rem;
}

.retry-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  margin: 1rem auto 0;
}

.instructor-header {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 3rem;
  padding: 2rem;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.instructor-images {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.primary-image {
  width: 100%;
  aspect-ratio: 1;
  border-radius: 12px;
  overflow: hidden;
}

.instructor-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.photo-placeholder {
  width: 100%;
  height: 100%;
  background: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 4rem;
  color: #ccc;
}

.gallery-images {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.5rem;
}

.gallery-image {
  width: 100%;
  aspect-ratio: 1;
  object-fit: cover;
  border-radius: 6px;
  cursor: pointer;
  transition: transform 0.2s;
}

.gallery-image:hover {
  transform: scale(1.05);
}

.more-images {
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0,0,0,0.7);
  color: white;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
}

.instructor-details {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.instructor-name-section {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.instructor-name {
  margin: 0;
  font-size: 2.5rem;
  color: #333;
  line-height: 1.2;
}

.instructor-actions {
  display: flex;
  gap: 0.75rem;
  align-items: center;
}

.action-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1rem;
  background: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s;
}

.action-btn:hover {
  background: #e9ecef;
}

.instructor-biography {
  color: #555;
  line-height: 1.7;
  font-size: 1.1rem;
}

.instructor-biography p {
  margin-bottom: 1rem;
}

.instructor-tags h3,
.social-links h3 {
  margin: 0 0 1rem 0;
  color: #333;
  font-size: 1.25rem;
}

.tag-list {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
}

.instructor-tag {
  background: linear-gradient(45deg, #667eea, #764ba2);
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-size: 0.875rem;
  font-weight: 500;
}

.social-list {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.social-link {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1.25rem;
  background: #fff;
  border: 2px solid #e1e5e9;
  border-radius: 8px;
  text-decoration: none;
  color: #495057;
  transition: all 0.2s;
}

.social-link:hover {
  border-color: #007bff;
  color: #007bff;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,123,255,0.15);
}

.social-link i {
  font-size: 1.25rem;
}

/* Gallery Modal */
.gallery-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.9);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.gallery-content {
  position: relative;
  max-width: 90vw;
  max-height: 90vh;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.gallery-close {
  position: absolute;
  top: -3rem;
  right: 0;
  background: none;
  border: none;
  color: white;
  font-size: 1.5rem;
  cursor: pointer;
  z-index: 1001;
}

.gallery-main-image {
  max-width: 100%;
  max-height: 80vh;
  object-fit: contain;
  border-radius: 8px;
}

.gallery-controls {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 2rem;
  color: white;
}

.gallery-btn {
  background: rgba(255,255,255,0.2);
  border: 1px solid rgba(255,255,255,0.3);
  color: white;
  padding: 0.75rem;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s;
}

.gallery-btn:hover:not(:disabled) {
  background: rgba(255,255,255,0.3);
}

.gallery-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.image-counter {
  font-weight: 500;
}

@keyframes loading {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

@media (max-width: 768px) {
  .instructor-header {
    grid-template-columns: 1fr;
    gap: 2rem;
    padding: 1.5rem;
  }
  
  .instructor-name {
    font-size: 2rem;
  }
  
  .instructor-name-section {
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
  }
  
  .gallery-images {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .social-list {
    flex-direction: column;
  }
  
  .social-link {
    justify-content: center;
  }
}
</style>
```

### Dynamic Instructor Loading
```vue
<template>
  <div class="dynamic-instructor">
    <div class="instructor-selector">
      <h2>Browse Instructors</h2>
      
      <div class="selector-controls">
        <select v-model="selectedHandle" @change="loadSelectedInstructor">
          <option value="">Select an instructor</option>
          <option 
            v-for="instructor in availableInstructors"
            :key="instructor.handle"
            :value="instructor.handle"
          >
            {{ instructor.first_name }} {{ instructor.last_name }}
          </option>
        </select>
        
        <div class="loading-controls">
          <label>
            <input v-model="simulateLoading" type="checkbox">
            Simulate Loading
          </label>
          <label>
            <input v-model="simulateError" type="checkbox">
            Simulate Error
          </label>
        </div>
      </div>
    </div>
    
    <div class="instructor-display">
      <codex-instructor 
        :handle="selectedHandle"
        :show-related="showRelated"
        :key="instructorKey"
      >
        <template #error-messages="{ error, genericErrors }">
          <div class="custom-error">
            <div class="error-icon">
              <i class="ri-emotion-sad-line"></i>
            </div>
            <h3>Oops! Something went wrong</h3>
            <p>{{ error?.message || 'We couldn\'t load this instructor\'s profile.' }}</p>
            <div class="error-actions">
              <button @click="retryLoad" class="retry-button">
                <i class="ri-refresh-line"></i>
                Try Again
              </button>
              <button @click="goBack" class="back-button">
                <i class="ri-arrow-left-line"></i>
                Go Back
              </button>
            </div>
          </div>
        </template>
        
        <template #content="{ instructor }">
          <div v-if="instructor" class="custom-content">
            <div class="content-header">
              <h2>
                <i class="ri-calendar-event-line"></i>
                Classes with {{ instructor.first_name }}
              </h2>
              
              <div class="view-options">
                <button 
                  @click="viewMode = 'upcoming'"
                  :class="['view-btn', { active: viewMode === 'upcoming' }]"
                >
                  Upcoming
                </button>
                <button 
                  @click="viewMode = 'all'"
                  :class="['view-btn', { active: viewMode === 'all' }]"
                >
                  All Classes
                </button>
              </div>
            </div>
            
            <!-- The event listing would be integrated here -->
            <div class="mock-events">
              <div v-for="event in getMockEvents(instructor)" :key="event.id" class="event-item">
                <div class="event-time">
                  <div class="event-date">{{ event.date }}</div>
                  <div class="event-duration">{{ event.time }}</div>
                </div>
                <div class="event-details">
                  <h4>{{ event.name }}</h4>
                  <p>{{ event.description }}</p>
                  <div class="event-meta">
                    <span class="event-level">{{ event.level }}</span>
                    <span class="event-duration">{{ event.duration }} min</span>
                  </div>
                </div>
                <div class="event-actions">
                  <button class="book-btn">Book Now</button>
                </div>
              </div>
            </div>
          </div>
        </template>
        
        <template #footer="{ instructor }">
          <div v-if="instructor && showRelated" class="custom-footer">
            <h2>More Great Instructors</h2>
            <div class="related-grid">
              <div 
                v-for="related in getRelatedInstructors(instructor)"
                :key="related.handle"
                class="related-instructor"
                @click="selectInstructor(related.handle)"
              >
                <img 
                  v-if="related.photo"
                  :src="related.photo"
                  :alt="related.name"
                  class="related-photo"
                >
                <div v-else class="related-placeholder">
                  <i class="ri-user-line"></i>
                </div>
                <h4>{{ related.name }}</h4>
                <p>{{ related.specialty }}</p>
              </div>
            </div>
          </div>
        </template>
      </codex-instructor>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const selectedHandle = ref('')
const simulateLoading = ref(false)
const simulateError = ref(false)
const showRelated = ref(true)
const viewMode = ref('upcoming')
const instructorKey = ref(0)

const availableInstructors = [
  { handle: 'sarah-johnson', first_name: 'Sarah', last_name: 'Johnson' },
  { handle: 'michael-chen', first_name: 'Michael', last_name: 'Chen' },
  { handle: 'emma-rodriguez', first_name: 'Emma', last_name: 'Rodriguez' },
  { handle: 'david-thompson', first_name: 'David', last_name: 'Thompson' }
]

const loadSelectedInstructor = () => {
  // Force component re-render to simulate new data loading
  instructorKey.value++
}

const getMockEvents = (instructor) => {
  const baseEvents = [
    {
      id: 1,
      name: 'Morning Flow',
      description: 'Energizing vinyasa flow to start your day',
      date: 'Today',
      time: '8:00 AM',
      duration: 60,
      level: 'All Levels'
    },
    {
      id: 2,
      name: 'Deep Stretch',
      description: 'Restorative stretching and relaxation',
      date: 'Tomorrow',
      time: '6:00 PM',
      duration: 45,
      level: 'Beginner'
    },
    {
      id: 3,
      name: 'Power Flow',
      description: 'Dynamic and challenging vinyasa practice',
      date: 'Friday',
      time: '7:00 AM',
      duration: 75,
      level: 'Advanced'
    }
  ]
  
  return baseEvents.map(event => ({
    ...event,
    name: `${event.name} with ${instructor.first_name}`
  }))
}

const getRelatedInstructors = (currentInstructor) => {
  const allInstructors = [
    { handle: 'sarah-johnson', name: 'Sarah Johnson', specialty: 'Yoga & Meditation', photo: '/images/sarah.jpg' },
    { handle: 'michael-chen', name: 'Michael Chen', specialty: 'HIIT & Strength', photo: '/images/michael.jpg' },
    { handle: 'emma-rodriguez', name: 'Emma Rodriguez', specialty: 'Pilates', photo: null },
    { handle: 'david-thompson', name: 'David Thompson', specialty: 'Functional Fitness', photo: '/images/david.jpg' }
  ]
  
  return allInstructors
    .filter(instructor => instructor.handle !== currentInstructor.handle)
    .slice(0, 3)
}

const selectInstructor = (handle) => {
  selectedHandle.value = handle
  loadSelectedInstructor()
}

const retryLoad = () => {
  simulateError.value = false
  loadSelectedInstructor()
}

const goBack = () => {
  selectedHandle.value = ''
}
</script>

<style scoped>
.dynamic-instructor {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.instructor-selector {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 2rem;
}

.selector-controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 1rem;
}

.selector-controls select {
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: white;
  min-width: 250px;
}

.loading-controls {
  display: flex;
  gap: 1rem;
}

.loading-controls label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  cursor: pointer;
}

.custom-error {
  text-align: center;
  padding: 4rem 2rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.error-icon i {
  font-size: 4rem;
  color: #ffc107;
  margin-bottom: 1rem;
}

.error-actions {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-top: 2rem;
}

.retry-button,
.back-button {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
}

.retry-button {
  background: #007bff;
  color: white;
}

.back-button {
  background: #6c757d;
  color: white;
}

.custom-content {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  overflow: hidden;
}

.content-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2rem;
  border-bottom: 1px solid #eee;
}

.content-header h2 {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin: 0;
  color: #333;
}

.view-options {
  display: flex;
  gap: 0.5rem;
}

.view-btn {
  padding: 0.5rem 1rem;
  background: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s;
}

.view-btn.active {
  background: #007bff;
  color: white;
  border-color: #007bff;
}

.mock-events {
  padding: 2rem;
}

.event-item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  gap: 2rem;
  padding: 1.5rem;
  border: 1px solid #eee;
  border-radius: 8px;
  margin-bottom: 1rem;
  transition: all 0.2s;
}

.event-item:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.event-time {
  text-align: center;
  min-width: 100px;
}

.event-date {
  font-weight: 600;
  color: #007bff;
}

.event-details h4 {
  margin: 0 0 0.5rem 0;
  color: #333;
}

.event-details p {
  margin: 0 0 1rem 0;
  color: #666;
}

.event-meta {
  display: flex;
  gap: 1rem;
}

.event-meta span {
  background: #f8f9fa;
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.875rem;
  color: #495057;
}

.book-btn {
  padding: 0.75rem 1.5rem;
  background: #28a745;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  white-space: nowrap;
}

.custom-footer {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.custom-footer h2 {
  text-align: center;
  margin-bottom: 2rem;
  color: #333;
}

.related-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.5rem;
}

.related-instructor {
  text-align: center;
  padding: 1.5rem;
  border: 1px solid #eee;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
}

.related-instructor:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 25px rgba(0,0,0,0.1);
}

.related-photo {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
  margin: 0 auto 1rem;
}

.related-placeholder {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 1rem;
  font-size: 2rem;
  color: #ccc;
}

.related-instructor h4 {
  margin: 0 0 0.5rem 0;
  color: #333;
}

.related-instructor p {
  margin: 0;
  color: #666;
  font-size: 0.875rem;
}

@media (max-width: 768px) {
  .selector-controls {
    flex-direction: column;
    gap: 1rem;
    align-items: stretch;
  }
  
  .content-header {
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
  }
  
  .event-item {
    grid-template-columns: 1fr;
    gap: 1rem;
    text-align: center;
  }
  
  .error-actions {
    flex-direction: column;
    align-items: center;
  }
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-instructor` | Main instructor container |
| `_c-card` | Card layout styling |
| `_c-header` | Header section styling |
| `_c-content` | Content section styling |
| `_c-footer` | Footer section styling |
| `_c-flex` | Flexbox utility |
| `_c-gap-lg` | Large gap spacing |
| `_c-instructor-images` | Image container styling |
| `_c-w-full` | Full width utility |
| `_c-square` | Square aspect ratio |
| `_c-instructor-info` | Info section styling |
| `_c-grid` | Grid layout |
| `_c-two-up` | Two-column grid |
| `_c-image-container` | Image wrapper |
| `_c-square-image` | Square image container |
| `_c-image` | Image element styling |
| `_c-additional-images-wrapper` | Gallery images wrapper |
| `_c-p-0` | Zero padding utility |
| `_c-display` | Display text styling |
| `_c-instructor-first-name` | First name styling |
| `_c-instructor-last-name` | Last name styling |
| `_c-instructor-desc` | Description styling |
| `_c-desc` | Generic description styling |
| `_c-leading-lg` | Large line height |
| `_c-tag-container` | Tag container |
| `_c-instructor-tags` | Instructor tags |
| `_c-tag` | Individual tag styling |
| `_c-tag-lg` | Large tag variant |
| `_c-social-links` | Social links container |
| `_c-social-link` | Individual social link |
| `_c-social-link-label` | Social link label |
| `_c-text-bold` | Bold text utility |
| `_c-instructor-grid` | Instructor grid layout |
| `_c-instructor-related` | Related instructors section |

## Translation Support

The Instructor component supports comprehensive internationalization through Vue i18n integration. The component displays instructor profiles with localized content and integrates with event listings and related instructor systems.

### Translation Keys

#### Profile Section Headers
- `instructor.upcoming_classes` - Upcoming classes section title
- `instructor.view_timetable` - View timetable link text
- `instructor.related_instructors` - Related instructors section title

#### Profile Information
- `instructor.biography` - Biography section label
- `instructor.specialties` - Specialties section label
- `instructor.experience` - Experience level label
- `instructor.social_links` - Social links section label
- `instructor.locations` - Locations section label

#### Class Information
- `instructor.class_count` - Total classes taught
- `instructor.student_count` - Total students taught
- `instructor.rating` - Average rating label
- `instructor.years_teaching` - Years of teaching experience

#### Content States
- `instructor.no_upcoming_classes` - No upcoming classes message
- `instructor.no_related_instructors` - No related instructors message
- `instructor.biography_unavailable` - Biography not available message

### Template Usage Examples

#### Complete Instructor Profile
```vue
<template>
  <codex-instructor 
    :instructor="instructor"
    :show-related="true"
  >
    <template #header="{ instructor }">
      <div class="custom-header">
        <div class="instructor-hero">
          <div class="photo-section">
            <img 
              v-if="instructor.photo" 
              :src="instructor.photo"
              :alt="$t('instructor.photo_alt', { name: instructor.first_name })"
            />
          </div>
          
          <div class="info-section">
            <h1>{{ instructor.first_name }} {{ instructor.last_name }}</h1>
            
            <div v-if="instructor.title" class="instructor-title">
              {{ $t(`instructor.titles.${instructor.title}`) }}
            </div>
            
            <div v-if="instructor.experience_years" class="experience">
              {{ $t('instructor.years_teaching', { years: instructor.experience_years }) }}
            </div>
            
            <div v-if="instructor.rating" class="rating">
              {{ $t('instructor.rating') }}: {{ instructor.rating }}/5
            </div>
          </div>
        </div>
      </div>
    </template>
    
    <template #content="{ instructor }">
      <div class="instructor-content">
        <!-- Biography Section -->
        <section class="biography-section">
          <h2>{{ $t('instructor.biography') }}</h2>
          <div 
            v-if="instructor.biography"
            v-html="instructor.biography"
            class="biography-content"
          />
          <p v-else class="no-biography">
            {{ $t('instructor.biography_unavailable') }}
          </p>
        </section>
        
        <!-- Specialties Section -->
        <section v-if="instructor.specialties?.length" class="specialties-section">
          <h3>{{ $t('instructor.specialties') }}</h3>
          <div class="specialty-tags">
            <span 
              v-for="specialty in instructor.specialties"
              :key="specialty"
              class="specialty-tag"
            >
              {{ $t(`specialties.${specialty}`) }}
            </span>
          </div>
        </section>
        
        <!-- Social Links Section -->
        <section v-if="instructor.social_links?.length" class="social-section">
          <h3>{{ $t('instructor.social_links') }}</h3>
          <div class="social-links">
            <a 
              v-for="link in instructor.social_links"
              :key="link.type"
              :href="link.url"
              target="_blank"
              :aria-label="$t('instructor.social_link_aria', { platform: link.type, name: instructor.first_name })"
            >
              <i :class="`ri-${link.type}-fill`"></i>
              {{ link.label }}
            </a>
          </div>
        </section>
        
        <!-- Upcoming Classes Section -->
        <section class="classes-section">
          <div class="section-header">
            <h2>{{ $t('instructor.upcoming_classes') }}</h2>
            <a :href="timetableUrl" class="view-all-link">
              {{ $t('instructor.view_timetable') }}
            </a>
          </div>
          
          <codex-event-listing 
            :instructor-handle="instructor.handle"
            :show-filters="false"
          >
            <template #empty>
              <div class="no-classes">
                {{ $t('instructor.no_upcoming_classes') }}
              </div>
            </template>
          </codex-event-listing>
        </section>
      </div>
    </template>
  </codex-instructor>
</template>
```

#### Instructor Profile with Statistics
```vue
<template>
  <codex-instructor :instructor="instructor">
    <template #header="{ instructor }">
      <div class="instructor-header">
        <div class="basic-info">
          <h1>{{ instructor.first_name }} {{ instructor.last_name }}</h1>
        </div>
        
        <div class="instructor-stats">
          <div v-if="instructor.total_classes" class="stat-item">
            <span class="stat-number">{{ instructor.total_classes }}</span>
            <span class="stat-label">{{ $t('instructor.class_count') }}</span>
          </div>
          
          <div v-if="instructor.total_students" class="stat-item">
            <span class="stat-number">{{ instructor.total_students }}</span>
            <span class="stat-label">{{ $t('instructor.student_count') }}</span>
          </div>
          
          <div v-if="instructor.years_teaching" class="stat-item">
            <span class="stat-number">{{ instructor.years_teaching }}</span>
            <span class="stat-label">{{ $t('instructor.years_experience') }}</span>
          </div>
          
          <div v-if="instructor.rating" class="stat-item">
            <span class="stat-number">{{ instructor.rating }}/5</span>
            <span class="stat-label">{{ $t('instructor.average_rating') }}</span>
          </div>
        </div>
      </div>
    </template>
  </codex-instructor>
</template>
```

#### Mobile-Optimized Instructor Profile
```vue
<template>
  <codex-instructor 
    :instructor="instructor"
    :class="{ 'mobile-layout': isMobile }"
  >
    <template #header="{ instructor }">
      <div class="mobile-header">
        <div class="instructor-photo">
          <img 
            v-if="instructor.photo"
            :src="instructor.photo"
            :alt="$t('instructor.photo_alt', { name: instructor.first_name })"
          />
        </div>
        
        <div class="instructor-basic">
          <h1>{{ instructor.first_name }} {{ instructor.last_name }}</h1>
          
          <div v-if="instructor.short_bio" class="mobile-bio">
            {{ instructor.short_bio }}
          </div>
          
          <div class="mobile-actions">
            <button @click="bookClass" class="book-btn">
              {{ $t('button.book_class') }}
            </button>
            
            <button @click="viewSchedule" class="schedule-btn">
              {{ $t('instructor.view_schedule') }}
            </button>
          </div>
        </div>
      </div>
    </template>
    
    <template #footer="{ instructor }">
      <div class="mobile-footer">
        <h3>{{ $t('instructor.related_instructors') }}</h3>
        
        <div v-if="!relatedInstructors.length" class="no-related">
          {{ $t('instructor.no_related_instructors') }}
        </div>
        
        <div v-else class="related-instructors-mobile">
          <codex-instructor-card
            v-for="related in relatedInstructors.slice(0, 3)"
            :key="related.id"
            :instructor="related"
            compact
          />
        </div>
      </div>
    </template>
  </codex-instructor>
</template>
```

#### Error State Instructor Profile
```vue
<template>
  <codex-instructor 
    :instructor="instructor"
    :loading="loading"
    :error="error"
  >
    <template #error-messages="{ error }">
      <div class="instructor-error">
        <div class="error-icon">
          <i class="ri-user-unfollow-line"></i>
        </div>
        
        <h2>{{ $t('instructor.not_found_title') }}</h2>
        <p>{{ $t('instructor.not_found_message') }}</p>
        
        <div class="error-actions">
          <button @click="goToInstructors" class="primary-action">
            {{ $t('instructor.browse_all_instructors') }}
          </button>
          
          <button @click="tryAgain" class="secondary-action">
            {{ $t('button.try_again') }}
          </button>
        </div>
      </div>
    </template>
  </codex-instructor>
</template>
```

### Integration with useI18n

```javascript
import { useI18n } from 'vue-i18n'

const { t, locale } = useI18n()

// Dynamic content handling
const instructorBiography = computed(() => {
  if (!instructor.value?.biography) return ''
  
  // Handle multi-language biographies
  if (typeof instructor.value.biography === 'object') {
    return instructor.value.biography[locale.value] || instructor.value.biography.en
  }
  
  return instructor.value.biography
})

// Specialties localization
const localizedSpecialties = computed(() => 
  instructor.value?.specialties?.map(specialty => 
    t(`specialties.${specialty}`, specialty)
  ) || []
)

// Experience level translation
const experienceLevel = computed(() => 
  instructor.value?.experience_level 
    ? t(`instructor.levels.${instructor.value.experience_level}`)
    : null
)
```

### Event Integration

When integrating with events and timetables:

```vue
<template>
  <codex-filter-context 
    group="instructor" 
    :fixed-values="{ 'instructor.handle': instructor.handle }"
  >
    <codex-event-listing 
      group="instructor"
      :show-filters="false"
      :days-to-show="3"
    >
      <template #header>
        <div class="events-header">
          <h2>{{ $t('instructor.upcoming_classes') }}</h2>
          <a :href="timetableUrl">
            <i class="ri-calendar-line"></i>
            {{ $t('instructor.view_timetable') }}
          </a>
        </div>
      </template>
      
      <template #empty>
        <div class="no-events">
          <i class="ri-calendar-line"></i>
          <p>{{ $t('instructor.no_upcoming_classes') }}</p>
          <a :href="timetableUrl">
            {{ $t('instructor.view_full_schedule') }}
          </a>
        </div>
      </template>
    </codex-event-listing>
  </codex-filter-context>
</template>
```

### Related Instructors Integration

```vue
<template>
  <div class="related-instructors">
    <h2>{{ $t('instructor.related_instructors') }}</h2>
    
    <div v-if="!relatedInstructors?.length" class="no-related">
      {{ $t('instructor.no_related_instructors') }}
    </div>
    
    <div v-else class="related-grid">
      <codex-instructor-card
        v-for="related in relatedInstructors"
        :key="related.id"
        :instructor="related"
        :show-description="false"
        @view="viewRelatedInstructor"
      />
    </div>
  </div>
</template>
```

### Accessibility Integration

```vue
<template>
  <article 
    class="instructor-profile"
    :aria-label="$t('instructor.profile_aria', { name: instructor.first_name + ' ' + instructor.last_name })"
  >
    <header>
      <h1 id="instructor-name">
        {{ instructor.first_name }} {{ instructor.last_name }}
      </h1>
    </header>
    
    <main>
      <section aria-labelledby="biography-heading">
        <h2 id="biography-heading">{{ $t('instructor.biography') }}</h2>
        <div v-html="instructor.biography" />
      </section>
      
      <section aria-labelledby="classes-heading">
        <h2 id="classes-heading">{{ $t('instructor.upcoming_classes') }}</h2>
        <!-- Event listing content -->
      </section>
    </main>
  </article>
</template>
```

The Instructor component's comprehensive translation system ensures that instructor profiles are fully localized with proper semantic structure and accessibility considerations.

## Best Practices

### Data Loading
- Handle both prop-based and dynamic data loading
- Implement proper loading states
- Provide meaningful error messages
- Cache instructor data when appropriate
- Handle URL parameter fallbacks

### Content Display
- Format biography content safely (HTML)
- Handle missing photos gracefully
- Display social links appropriately
- Show tags in a user-friendly manner
- Provide image gallery functionality

### Performance
- Lazy load related instructors
- Optimize image loading
- Implement proper component lifecycle
- Monitor data fetching performance
- Use skeleton loading for better UX

### Accessibility
- Provide proper alt text for images
- Ensure keyboard navigation works
- Use semantic HTML structure
- Support screen readers
- Handle focus management

### Integration
- Sync with event listing system
- Handle bookmark functionality
- Support deep linking
- Test router integration
- Validate data structures

### Error Handling
- Provide fallback for missing data
- Handle network errors gracefully
- Show user-friendly error messages
- Implement retry mechanisms
- Log errors for debugging

## Component Registration
```javascript
// Global registration
app.component('CodexInstructor', Instructor)

// Local registration  
import Instructor from '@/components/instructors/Instructor.vue'

export default {
  components: {
    CodexInstructor: Instructor
  }
}
```

## Internationalization

The component uses the following translation keys:

### Core Instructor Interface
| Key | Usage |
|-----|-------|
| `instructor.upcoming_classes` | Upcoming classes section title |
| `instructor.view_timetable` | View timetable button text |
| `instructor.related_instructors` | Related instructors section title |

### Event Listing Integration
| Key | Usage |
|-----|-------|
| `event.no_upcoming_events` | No upcoming events message |
| `event.view_all_classes` | View all classes link text |
| `timetable.book_now` | Book now button text |
| `timetable.view_schedule` | View schedule link text |

### Navigation and Actions
| Key | Usage |
|-----|-------|
| `button.view_profile` | View profile button text |
| `button.book_class` | Book class action button |
| `navigation.back_to_instructors` | Back to instructors list |

### Error Messages
| Key | Usage |
|-----|-------|
| `error.instructor_not_found` | Instructor not found error |
| `error.loading_failed` | Failed to load instructor data |
| `error.no_classes_available` | No classes available message |

### Translation Usage Examples
```vue
<!-- Upcoming classes section -->
<codex-event-listing group="instructor" :show-filters="false">
  <template #header="{ instructor }">
    <div class="section-header">
      <codex-title :content="t('instructor.upcoming_classes')" />
      <codex-anchor 
        anchor-class="btn primary-btn"
        :default-text="t('instructor.view_timetable')"
        :href="window.codex.urls.booking_url"
      >
        <template #before>
          <i class="ri-calendar-line"></i>
        </template>
      </codex-anchor>
    </div>
  </template>
</codex-event-listing>

<!-- Related instructors section -->
<div v-if="showRelated" class="related-instructors">
  <codex-title 
    class-name="section-title"
    :content="t('instructor.related_instructors')"
  />
  <codex-instructor-card
    v-for="instructor in relatedInstructors"
    :key="instructor.handle"
    :instructor="instructor"
    :loading="loading"
  />
</div>

<!-- Error handling -->
<template v-if="error">
  <div class="error-state">
    <h3>{{ $t('error.instructor_not_found') }}</h3>
    <p>{{ $t('error.loading_failed') }}</p>
    <button @click="retryLoad" class="retry-button">
      {{ $t('button.retry') }}
    </button>
  </div>
</template>
```

### Component Integration Translation Notes

#### Event Listing Integration
- The component uses `codex-event-listing` which has its own translation requirements
- Event listing translations are handled by the child component
- The instructor component provides translated header content for the event listing

#### Filter Context Integration
- Uses `codex-filter-context` for event filtering by instructor
- Filter-related translations are managed by the filter components
- The instructor handle is passed as a fixed filter value

#### Social Links
- Social link labels come from the instructor data, not translation keys
- Social link types (instagram, facebook, etc.) could be translated if needed
- The component displays social links as provided in the data structure

#### Related Instructors
- Uses the same `codex-instructor-card` components for related instructors
- Card-level translations are handled by the individual card components
- Loading states for related instructors use the same skeleton patterns