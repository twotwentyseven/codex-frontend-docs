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
- `instructor.social_links` - Social links section label
- `instructor.locations` - Locations section label

#### Content States
- `instructor.no_upcoming_classes` - No upcoming classes message
- `instructor.no_related_instructors` - No related instructors message

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