# InstructorCard Component

## Overview
The InstructorCard component displays individual instructor information in a visually appealing card format with support for loading states, bookmarking functionality, and hover interactions. It provides a flexible card layout with customizable slots for header, content, and footer sections, integration with the bookmark system, and tag filtering capabilities. The component handles instructor photos with fallback SVG graphics and includes interactive elements for viewing instructor profiles.

## Basic Usage
```vue
<template>
  <div class="instructor-card-example">
    <codex-instructor-card 
      :instructor="instructor"
      :loading="isLoading"
      :show-description="true"
      :truncate-length="150"
      @view="handleViewInstructor"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'

const isLoading = ref(false)

const instructor = {
  id: 1,
  first_name: 'Sarah',
  last_name: 'Johnson',
  photo: '/images/instructors/sarah-johnson.jpg',
  description: 'Sarah is a certified yoga instructor with over 10 years of experience in Hatha and Vinyasa yoga.',
  tags: ['Yoga', 'Meditation', 'Wellness'],
  metafields: {
    description: 'Backup description from metafields'
  }
}

const handleViewInstructor = (instructor) => {
  console.log('Viewing instructor:', instructor.first_name, instructor.last_name)
}
</script>
```

## Key Features
- Square card layout with hover interactions
- Loading state with skeleton animation
- Instructor photo display with SVG fallback
- Bookmark functionality integration
- Tag filtering and display
- Customizable description truncation
- Slot-based content customization
- Click-to-view functionality
- Responsive design support

## Configuration Props

### Core Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `instructor` | `Object|Number` | `required` | Instructor data object or ID |
| `loading` | `Boolean` | `false` | Show loading skeleton state |

### Display Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showTags` | `Array` | `[]` | Array of tag names to display |
| `showDescription` | `Boolean` | `false` | Display instructor description |
| `truncateLength` | `Number|Boolean` | `false` | Description truncation length |

### Common Props
Document which common props from `@/config/common` are used:
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Controls card border display |

### Common Functions
Document which utilities from `useCommon` are used:
| Function | Usage |
|----------|-------|
| `truncateString` | Truncates instructor description text |
| `filteredTags` | Filters and displays instructor tags |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `view` | `instructor` | Emitted when instructor card is clicked |

## Slots

### Layout Slots
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ instructor }` | Custom header content (tags and bookmark) |
| `content` | `{ instructor }` | Custom content area (photo section) |
| `footer` | `{ instructor }` | Custom footer content (name and description) |

## Instructor Data Structure

The component expects the instructor object to have the following structure:

```javascript
{
  id: Number,                    // Unique instructor ID
  first_name: String,           // Instructor's first name
  last_name: String,            // Instructor's last name
  photo: String,                // URL to instructor photo
  description: String,          // Instructor description
  tags: Array,                  // Array of tag strings
  metafields: {
    description: String         // Fallback description
  }
}
```

## Examples

### Basic Instructor Grid
```vue
<template>
  <div class="instructor-grid-example">
    <h2>Meet Our Instructors</h2>
    
    <div class="instructor-grid">
      <codex-instructor-card 
        v-for="instructor in instructors"
        :key="instructor.id"
        :instructor="instructor"
        :loading="isLoading"
        :show-description="true"
        :truncate-length="120"
        :show-tags="['Yoga', 'Pilates', 'Fitness']"
        @view="handleViewInstructor"
      />
    </div>
    
    <div v-if="isLoading" class="loading-grid">
      <codex-instructor-card 
        v-for="i in 6"
        :key="i"
        :instructor="{}"
        :loading="true"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const isLoading = ref(true)
const instructors = ref([])

onMounted(async () => {
  // Simulate loading instructors
  setTimeout(() => {
    instructors.value = [
      {
        id: 1,
        first_name: 'Sarah',
        last_name: 'Johnson',
        photo: '/images/instructors/sarah-johnson.jpg',
        description: 'Sarah is a certified yoga instructor with over 10 years of experience in Hatha and Vinyasa yoga. She believes in creating a supportive and inclusive environment for all students.',
        tags: ['Yoga', 'Meditation', 'Hatha', 'Vinyasa']
      },
      {
        id: 2,
        first_name: 'Michael',
        last_name: 'Chen',
        photo: '/images/instructors/michael-chen.jpg',
        description: 'Michael specializes in high-intensity interval training and strength building. His energetic classes will push you to achieve your fitness goals.',
        tags: ['HIIT', 'Strength', 'Fitness']
      },
      {
        id: 3,
        first_name: 'Emma',
        last_name: 'Rodriguez',
        photo: '/images/instructors/emma-rodriguez.jpg',
        description: 'Emma brings grace and precision to Pilates instruction. Her classes focus on core strength, flexibility, and body awareness.',
        tags: ['Pilates', 'Core', 'Flexibility']
      },
      {
        id: 4,
        first_name: 'David',
        last_name: 'Thompson',
        photo: null, // Will show SVG fallback
        description: 'David is passionate about functional movement and injury prevention. His classes combine mobility work with strength training.',
        tags: ['Functional', 'Mobility', 'Strength']
      }
    ]
    isLoading.value = false
  }, 1500)
})

const handleViewInstructor = (instructor) => {
  console.log('Navigating to instructor profile:', instructor)
  // In a real app, this would navigate to instructor detail page
  window.location.href = `/instructors/${instructor.id}`
}
</script>

<style scoped>
.instructor-grid-example {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.instructor-grid,
.loading-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}

h2 {
  text-align: center;
  color: #333;
  margin-bottom: 1rem;
}

@media (max-width: 768px) {
  .instructor-grid,
  .loading-grid {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 1rem;
  }
}
</style>
```

### Custom Instructor Card with Slots
```vue
<template>
  <div class="custom-instructor-cards">
    <h2>Featured Instructors</h2>
    
    <div class="featured-grid">
      <codex-instructor-card 
        v-for="instructor in featuredInstructors"
        :key="instructor.id"
        :instructor="instructor"
        :show-description="true"
        :truncate-length="100"
        @view="handleViewInstructor"
      >
        <template #header="{ instructor }">
          <div class="custom-header">
            <div class="instructor-badges">
              <span v-if="instructor.featured" class="featured-badge">
                <i class="ri-star-fill"></i>
                Featured
              </span>
              <span v-if="instructor.new" class="new-badge">
                <i class="ri-fire-fill"></i>
                New
              </span>
            </div>
            
            <div class="instructor-rating" v-if="instructor.rating">
              <div class="stars">
                <i 
                  v-for="star in 5"
                  :key="star"
                  :class="[
                    'ri-star-fill',
                    { active: star <= instructor.rating }
                  ]"
                ></i>
              </div>
              <span class="rating-text">{{ instructor.rating }}/5</span>
            </div>
          </div>
        </template>
        
        <template #content="{ instructor }">
          <div class="custom-content">
            <div class="instructor-photo-container">
              <img 
                v-if="instructor.photo"
                :src="instructor.photo"
                :alt="`${instructor.first_name} ${instructor.last_name}`"
                class="instructor-photo"
              >
              <div v-else class="photo-placeholder">
                <i class="ri-user-line"></i>
              </div>
              
              <div class="photo-overlay">
                <div class="class-count" v-if="instructor.class_count">
                  <i class="ri-calendar-line"></i>
                  {{ instructor.class_count }} classes
                </div>
              </div>
            </div>
          </div>
        </template>
        
        <template #footer="{ instructor }">
          <div class="custom-footer">
            <div class="instructor-name">
              <h3>{{ instructor.first_name }} {{ instructor.last_name }}</h3>
              <p class="instructor-title">{{ instructor.title }}</p>
            </div>
            
            <div class="instructor-specialties" v-if="instructor.specialties">
              <div class="specialty-tags">
                <span 
                  v-for="specialty in instructor.specialties.slice(0, 3)"
                  :key="specialty"
                  class="specialty-tag"
                >
                  {{ specialty }}
                </span>
              </div>
            </div>
            
            <div class="instructor-description" v-if="instructor.description">
              <p>{{ truncateText(instructor.description, 80) }}</p>
            </div>
            
            <div class="instructor-actions">
              <button 
                @click.stop="viewProfile(instructor)"
                class="btn-primary"
              >
                View Profile
              </button>
              <button 
                @click.stop="viewSchedule(instructor)"
                class="btn-secondary"
              >
                <i class="ri-calendar-line"></i>
                Schedule
              </button>
            </div>
          </div>
        </template>
      </codex-instructor-card>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const featuredInstructors = ref([
  {
    id: 1,
    first_name: 'Sarah',
    last_name: 'Johnson',
    title: 'Senior Yoga Instructor',
    photo: '/images/instructors/sarah-johnson.jpg',
    description: 'Sarah brings over 10 years of yoga expertise to every class, specializing in Hatha and Vinyasa styles with a focus on mindfulness and breath work.',
    rating: 5,
    class_count: 142,
    specialties: ['Hatha Yoga', 'Vinyasa', 'Meditation', 'Breathwork'],
    featured: true,
    new: false
  },
  {
    id: 2,
    first_name: 'Michael',
    last_name: 'Chen',
    title: 'HIIT & Strength Coach',
    photo: '/images/instructors/michael-chen.jpg',
    description: 'Michael\'s high-energy classes combine strength training with cardio for maximum results. His motivational style helps students push past their limits.',
    rating: 4,
    class_count: 89,
    specialties: ['HIIT', 'Strength Training', 'Cardio', 'CrossFit'],
    featured: false,
    new: true
  },
  {
    id: 3,
    first_name: 'Emma',
    last_name: 'Rodriguez',
    title: 'Pilates Master Trainer',
    photo: '/images/instructors/emma-rodriguez.jpg',
    description: 'Emma\'s precise Pilates instruction focuses on core strength, posture, and body alignment. Her classes are perfect for all fitness levels.',
    rating: 5,
    class_count: 203,
    specialties: ['Pilates', 'Core Strength', 'Posture', 'Rehabilitation'],
    featured: true,
    new: false
  }
])

const truncateText = (text, length) => {
  return text.length > length ? text.substring(0, length) + '...' : text
}

const handleViewInstructor = (instructor) => {
  viewProfile(instructor)
}

const viewProfile = (instructor) => {
  console.log('Viewing instructor profile:', instructor)
}

const viewSchedule = (instructor) => {
  console.log('Viewing instructor schedule:', instructor)
}
</script>

<style scoped>
.custom-instructor-cards {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.featured-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}

.custom-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1rem;
}

.instructor-badges {
  display: flex;
  gap: 0.5rem;
}

.featured-badge,
.new-badge {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 600;
}

.featured-badge {
  background: linear-gradient(45deg, #ffd700, #ffed4e);
  color: #333;
}

.new-badge {
  background: linear-gradient(45deg, #ff6b6b, #ee5a24);
  color: white;
}

.instructor-rating {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 0.25rem;
}

.stars {
  display: flex;
  gap: 0.125rem;
}

.stars i {
  color: #ddd;
  font-size: 0.875rem;
}

.stars i.active {
  color: #ffd700;
}

.rating-text {
  font-size: 0.75rem;
  color: #666;
}

.custom-content {
  position: relative;
  margin-bottom: 1rem;
}

.instructor-photo-container {
  position: relative;
  width: 100%;
  aspect-ratio: 1;
  border-radius: 8px;
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
  font-size: 3rem;
  color: #ccc;
}

.photo-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(transparent, rgba(0,0,0,0.7));
  color: white;
  padding: 1rem;
  transform: translateY(100%);
  transition: transform 0.3s ease;
}

.instructor-photo-container:hover .photo-overlay {
  transform: translateY(0);
}

.class-count {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
}

.custom-footer {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.instructor-name h3 {
  margin: 0;
  color: #333;
  font-size: 1.25rem;
}

.instructor-title {
  margin: 0.25rem 0 0 0;
  color: #666;
  font-size: 0.875rem;
  font-weight: 500;
}

.specialty-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.specialty-tag {
  background: #e3f2fd;
  color: #1976d2;
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 500;
}

.instructor-description p {
  margin: 0;
  color: #666;
  font-size: 0.875rem;
  line-height: 1.4;
}

.instructor-actions {
  display: flex;
  gap: 0.75rem;
}

.btn-primary,
.btn-secondary {
  flex: 1;
  padding: 0.75rem 1rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  transition: all 0.2s;
}

.btn-primary {
  background: #1976d2;
  color: white;
}

.btn-primary:hover {
  background: #1565c0;
}

.btn-secondary {
  background: #f5f5f5;
  color: #333;
  border: 1px solid #ddd;
}

.btn-secondary:hover {
  background: #eeeeee;
}

@media (max-width: 768px) {
  .featured-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
  
  .instructor-actions {
    flex-direction: column;
  }
}
</style>
```

### Instructor Card with Filter Integration
```vue
<template>
  <div class="filtered-instructors">
    <div class="filter-section">
      <h2>Find Your Perfect Instructor</h2>
      
      <div class="filter-controls">
        <div class="filter-group">
          <label>Specialties:</label>
          <div class="checkbox-group">
            <label v-for="specialty in allSpecialties" :key="specialty">
              <input 
                v-model="selectedSpecialties"
                :value="specialty"
                type="checkbox"
              >
              {{ specialty }}
            </label>
          </div>
        </div>
        
        <div class="filter-group">
          <label>Experience Level:</label>
          <select v-model="selectedExperience">
            <option value="">Any Level</option>
            <option value="beginner">Beginner Friendly</option>
            <option value="intermediate">Intermediate</option>
            <option value="advanced">Advanced</option>
          </select>
        </div>
        
        <div class="filter-group">
          <label>
            <input v-model="showFavoritesOnly" type="checkbox">
            Show Favorites Only
          </label>
        </div>
        
        <button @click="clearFilters" class="clear-btn">
          Clear Filters
        </button>
      </div>
    </div>
    
    <div class="results-section">
      <div class="results-header">
        <h3>{{ filteredInstructors.length }} Instructors Found</h3>
        <div class="view-options">
          <button 
            @click="viewMode = 'grid'"
            :class="['view-btn', { active: viewMode === 'grid' }]"
          >
            <i class="ri-grid-line"></i>
            Grid
          </button>
          <button 
            @click="viewMode = 'list'"
            :class="['view-btn', { active: viewMode === 'list' }]"
          >
            <i class="ri-list-line"></i>
            List
          </button>
        </div>
      </div>
      
      <div :class="['instructor-container', viewMode]">
        <codex-instructor-card 
          v-for="instructor in filteredInstructors"
          :key="instructor.id"
          :instructor="instructor"
          :show-description="true"
          :truncate-length="viewMode === 'list' ? 200 : 100"
          :show-tags="selectedSpecialties"
          @view="handleViewInstructor"
        />
      </div>
      
      <div v-if="!filteredInstructors.length" class="no-results">
        <i class="ri-search-line"></i>
        <h3>No instructors found</h3>
        <p>Try adjusting your filters to see more results.</p>
        <button @click="clearFilters" class="btn-primary">
          Clear All Filters
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const viewMode = ref('grid')
const selectedSpecialties = ref([])
const selectedExperience = ref('')
const showFavoritesOnly = ref(false)

// Mock favorites (would come from bookmark system in real app)
const favoriteInstructors = ref([1, 3])

const allSpecialties = [
  'Yoga', 'Pilates', 'HIIT', 'Strength Training', 
  'Cardio', 'Meditation', 'Flexibility', 'Dance'
]

const allInstructors = ref([
  {
    id: 1,
    first_name: 'Sarah',
    last_name: 'Johnson',
    photo: '/images/instructors/sarah-johnson.jpg',
    description: 'Sarah brings mindfulness and precision to every yoga class, creating a supportive environment for students of all levels.',
    tags: ['Yoga', 'Meditation', 'Flexibility'],
    experience_level: 'beginner',
    specialties: ['Yoga', 'Meditation']
  },
  {
    id: 2,
    first_name: 'Michael',
    last_name: 'Chen',
    photo: '/images/instructors/michael-chen.jpg',
    description: 'High-energy instructor specializing in HIIT and strength training for maximum fitness results.',
    tags: ['HIIT', 'Strength Training', 'Cardio'],
    experience_level: 'intermediate',
    specialties: ['HIIT', 'Strength Training']
  },
  {
    id: 3,
    first_name: 'Emma',
    last_name: 'Rodriguez',
    photo: '/images/instructors/emma-rodriguez.jpg',
    description: 'Pilates expert focusing on core strength, posture improvement, and body awareness.',
    tags: ['Pilates', 'Core', 'Flexibility'],
    experience_level: 'beginner',
    specialties: ['Pilates', 'Flexibility']
  },
  {
    id: 4,
    first_name: 'David',
    last_name: 'Thompson',
    photo: null,
    description: 'Advanced strength and conditioning coach for serious athletes and fitness enthusiasts.',
    tags: ['Strength Training', 'Cardio', 'HIIT'],
    experience_level: 'advanced',
    specialties: ['Strength Training', 'HIIT']
  },
  {
    id: 5,
    first_name: 'Lisa',
    last_name: 'Park',
    photo: '/images/instructors/lisa-park.jpg',
    description: 'Dance fitness instructor bringing joy and energy to every workout session.',
    tags: ['Dance', 'Cardio', 'Fun'],
    experience_level: 'beginner',
    specialties: ['Dance', 'Cardio']
  }
])

const filteredInstructors = computed(() => {
  return allInstructors.value.filter(instructor => {
    // Filter by specialties
    if (selectedSpecialties.value.length > 0) {
      const hasSpecialty = selectedSpecialties.value.some(specialty =>
        instructor.specialties.includes(specialty)
      )
      if (!hasSpecialty) return false
    }
    
    // Filter by experience level
    if (selectedExperience.value && instructor.experience_level !== selectedExperience.value) {
      return false
    }
    
    // Filter by favorites
    if (showFavoritesOnly.value && !favoriteInstructors.value.includes(instructor.id)) {
      return false
    }
    
    return true
  })
})

const clearFilters = () => {
  selectedSpecialties.value = []
  selectedExperience.value = ''
  showFavoritesOnly.value = false
}

const handleViewInstructor = (instructor) => {
  console.log('Viewing instructor:', instructor)
}
</script>

<style scoped>
.filtered-instructors {
  max-width: 1400px;
  margin: 0 auto;
  padding: 2rem;
}

.filter-section {
  background: white;
  border-radius: 8px;
  padding: 2rem;
  margin-bottom: 2rem;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.filter-controls {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  margin-top: 1.5rem;
}

.filter-group {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.filter-group label {
  font-weight: 600;
  color: #333;
}

.checkbox-group {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 0.5rem;
}

.checkbox-group label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-weight: normal;
  cursor: pointer;
}

.filter-group select {
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: white;
}

.clear-btn {
  padding: 0.75rem 1.5rem;
  background: #dc3545;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  align-self: start;
}

.results-section {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  overflow: hidden;
}

.results-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem;
  border-bottom: 1px solid #eee;
}

.view-options {
  display: flex;
  gap: 0.5rem;
}

.view-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  background: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s;
}

.view-btn.active {
  background: #1976d2;
  color: white;
  border-color: #1976d2;
}

.instructor-container.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 2rem;
  padding: 2rem;
}

.instructor-container.list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 2rem;
}

.no-results {
  text-align: center;
  padding: 4rem 2rem;
  color: #666;
}

.no-results i {
  font-size: 3rem;
  margin-bottom: 1rem;
  color: #ccc;
}

.btn-primary {
  padding: 0.75rem 1.5rem;
  background: #1976d2;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 1rem;
}

@media (max-width: 768px) {
  .filter-controls {
    grid-template-columns: 1fr;
    gap: 1rem;
  }
  
  .results-header {
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
  }
  
  .instructor-container.grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
    padding: 1rem;
  }
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-card` | Base card styling |
| `_c-card-instructor` | Instructor-specific card styling |
| `_c-square` | Square aspect ratio container |
| `_c-hover-parent` | Enables hover interactions |
| `_c-border` | Card border styling |
| `_c-header` | Card header section |
| `_c-content` | Card content section |
| `_c-footer` | Card footer section |
| `_c-tag-container` | Tag container styling |
| `_c-instructor-tags` | Instructor tag styling |
| `_c-tag` | Individual tag styling |
| `_c-photo-wrapper` | Photo container wrapper |
| `_c-square-image` | Square image container |
| `_c-photo` | Photo element styling |
| `_c-photo--primary` | Primary photo variant |
| `_c-title` | Title text styling |
| `_c-instructor-first-name` | First name styling |
| `_c-instructor-last-name` | Last name styling |
| `_c-hover-content` | Content shown on hover |
| `_c-overflow-hidden` | Hidden overflow container |
| `_c-description` | Description text styling |
| `_c-btn-container` | Button container styling |
| `_c-btn` | Button styling |
| `_c-w-auto` | Auto width utility |
| `_c-secondary-btn` | Secondary button variant |

## Internationalization

The component uses the following translation keys:

### Core Action Buttons
| Key | Usage |
|-----|-------|
| `button.view_profile` | View profile button text in hover state |

### Translation Usage Examples
```vue
<!-- View profile button in hover overlay -->
<div class="hover-content">
  <div class="instructor-description" v-if="showDescription">
    {{ truncateString(instructor.description || instructor.metafields.description) }}
  </div>
  <div class="button-container">
    <div class="btn primary-btn" @click.prevent="$emit('view', instructor)">
      {{ $t("button.view_profile") }}
    </div>
  </div>
</div>
```

### Translation Notes

#### Minimal Translation Requirements
The InstructorCard component has minimal translation requirements because:
- Instructor names are displayed as provided in the data (first_name, last_name)
- Instructor descriptions come from the data and are not translated keys
- Tags are displayed as provided in the instructor data
- The main translatable element is the "View Profile" button

#### Data-Driven Content
Most of the card content is data-driven rather than requiring translation:
- **Instructor Names**: Displayed directly from `instructor.first_name` and `instructor.last_name`
- **Descriptions**: Come from `instructor.description` or `instructor.metafields.description`
- **Tags**: Displayed as provided in the `instructor.tags` array
- **Photos**: Image URLs from `instructor.photo`

#### Integration with Parent Components
- The card is typically used within instructor listing components
- Parent components handle bulk translation needs (headers, sections, pagination)
- The card focuses on individual instructor presentation

#### Hover Interactions
- The primary translation requirement is for the hover state "View Profile" button
- Hover descriptions are truncated using the `truncateString` utility
- Click interactions emit events rather than navigate directly, allowing parent components to handle routing

#### Accessibility Considerations
- The `button.view_profile` text also serves as accessible button content
- Alt text for images would use instructor names from the data
- Card interactions are keyboard accessible through standard button elements

## Component Registration
```javascript
// Global registration
app.component('CodexInstructorCard', InstructorCard)

// Local registration  
import InstructorCard from '@/components/instructors/InstructorCard.vue'

export default {
  components: {
    CodexInstructorCard: InstructorCard
  }
}
``` 