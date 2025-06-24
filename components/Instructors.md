# Instructors Component

## Overview
The Instructors component provides a comprehensive instructor listing interface with filtering capabilities, pagination, and bookmark functionality. It features loading states, error handling, filter integration with contextual filtering, and responsive grid layouts. The component supports both filtered and unfiltered views, bookmark toggling, and integration with the filter context system for advanced search and filtering capabilities.

## Basic Usage
```vue
<template>
  <div class="instructors-listing-example">
    <codex-instructors 
      :show-filters="true"
      :show-description="true"
      :enable-bookmark-toggling="true"
      :per-page="12"
    />
  </div>
</template>
```

## Key Features
- Comprehensive instructor grid display
- Advanced filtering with contextual filters
- Bookmark functionality with toggle controls
- Loading states with skeleton animations
- Pagination integration
- Error handling with user-friendly messages
- Responsive grid layout
- Tag filtering and display
- No results state handling
- Filter wrapper integration

## Configuration Props

### Display Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showTags` | `Array` | `[]` | Array of tag names to display |
| `showDescription` | `Boolean` | `false` | Display instructor descriptions |
| `showFilters` | `Boolean` | `true` | Show filter controls |
| `truncateLength` | `Number|Boolean` | `false` | Description truncation length |

### Filtering Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `onlyShowBookmarked` | `Boolean` | `false` | Show only bookmarked instructors |
| `enableBookmarkToggling` | `Boolean` | `false` | Enable bookmark filter toggle |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `contentWrapperType` | `String` | `''` | Content wrapper component type |
| `contentWrapperSettings` | `Object` | `{}` | Wrapper component settings |
| `perPage` | `Number` | `27` | Number of instructors per page |
| `group` | `String` | `'default'` | Filter group identifier |

### Common Props
Document which common props from `@/config/common` are used:
| Prop Name | Usage |
|-----------|-------|
| `enableBorder` | Controls instructor card borders |
| `showPoweredBy` | Controls "Powered by" display |

### Common Functions
Document which utilities from `useCommon` are used:
| Function | Usage |
|----------|-------|
| `truncateString` | Truncates instructor descriptions |
| `filteredTags` | Filters displayed tags |
| `loadingItems` | Provides skeleton loading items |

## Slots

### Layout Slots
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ instructors, loading, error, genericErrors, loadingItems }` | Custom header content |
| `content` | `{ instructors, loading, error, genericErrors, loadingItems }` | Custom content area |
| `footer` | `{ instructors, loading, error, genericErrors, loadingItems }` | Custom footer content |
| `filters` | N/A | Custom filter controls |
| `primary-filters` | `{ group }` | Primary filter components |

## Filter Integration

The component integrates with the filter context system:

```javascript
// Filter context usage
const hasFilterContext = useFilterContext(props.group, loadInstructors)
```

### Primary Filters
- Instructor selection filter (checkbox group)
- Bookmark toggle (when enabled)
- Custom contextual filters

## Examples

### Basic Instructor Listing
```vue
<template>
  <div class="instructor-listing-basic">
    <codex-instructors 
      :show-filters="true"
      :show-description="true"
      :per-page="9"
      group="studio-instructors"
    />
  </div>
</template>
```

### Advanced Instructor Directory
```vue
<template>
  <div class="instructor-directory">
    <codex-instructors 
      :show-filters="true"
      :show-description="true"
      :enable-bookmark-toggling="true"
      :per-page="12"
      group="directory"
    >
      <template #header="{ instructors, loading }">
        <div class="directory-header">
          <h1>Meet Our Instructors</h1>
          <p>Discover amazing instructors and find your perfect match</p>
          
          <div class="stats" v-if="!loading">
            <div class="stat-item">
              <span class="stat-number">{{ instructors?.length || 0 }}</span>
              <span class="stat-label">Instructors</span>
            </div>
            <div class="stat-item">
              <span class="stat-number">{{ getSpecialtyCount() }}</span>
              <span class="stat-label">Specialties</span>
            </div>
          </div>
        </div>
      </template>
      
      <template #primary-filters="{ group }">
        <div class="custom-filters">
          <codex-contextual-filter 
            label="Instructor Type"
            :group="group"
            definition="instructors.Type"
            filter-key="instructor_type"
            filter-type="checkbox-group"
            :enable-border="false"
            :primary-filter="true"
          />
          
          <codex-contextual-filter 
            label="Experience Level"
            :group="group"
            definition="instructors.ExperienceLevel"
            filter-key="experience"
            filter-type="radio-group"
            :enable-border="false"
            :primary-filter="true"
          />
          
          <codex-contextual-filter 
            label="Specialties"
            :group="group"
            definition="instructors.Specialty"
            filter-key="specialties"
            filter-type="checkbox-group"
            :enable-border="false"
            :primary-filter="true"
          />
        </div>
      </template>
    </codex-instructors>
  </div>
</template>

<script setup>
const getSpecialtyCount = () => {
  // Would calculate unique specialties from instructor data
  return 15
}
</script>

<style scoped>
.instructor-directory {
  max-width: 1400px;
  margin: 0 auto;
  padding: 2rem;
}

.directory-header {
  text-align: center;
  margin-bottom: 3rem;
}

.directory-header h1 {
  font-size: 3rem;
  color: #333;
  margin-bottom: 1rem;
}

.directory-header p {
  font-size: 1.25rem;
  color: #666;
  margin-bottom: 2rem;
}

.stats {
  display: flex;
  justify-content: center;
  gap: 3rem;
}

.stat-item {
  text-align: center;
}

.stat-number {
  display: block;
  font-size: 2.5rem;
  font-weight: bold;
  color: #007bff;
}

.stat-label {
  font-size: 1rem;
  color: #666;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.custom-filters {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 2rem;
}

@media (max-width: 768px) {
  .directory-header h1 {
    font-size: 2rem;
  }
  
  .stats {
    flex-direction: column;
    gap: 1rem;
  }
  
  .custom-filters {
    grid-template-columns: 1fr;
  }
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-instructors` | Main instructors container |
| `_c-card` | Card layout styling |
| `_c-header` | Header section |
| `_c-content` | Content section |
| `_c-footer` | Footer section |
| `_c-grid` | Grid layout |
| `_c-instructor-grid` | Instructor-specific grid |
| `_c-no-results` | No results message styling |

## Internationalization

The component uses the following translation keys:

### Core Interface Messages
| Key | Usage |
|-----|-------|
| `instructor.no_results` | No instructors found message |
| `filter.instructor_default_label` | Default instructor filter label |
| `filter.show_favorites` | Bookmark filter toggle label |

### Translation Usage Examples
```vue
<!-- No results message -->
<div v-if="instructors && !instructors.length && !loading" class="_c-no-results">
  {{ $t('instructor.no_results') }}
</div>

<!-- Filter labels -->
<codex-contextual-filter 
  :label="t('filter.instructor_default_label')" 
  :group="props.group" 
  definition="instructors.FullNameHandle" 
  filter-key="handle" 
  filter-type="text"
/>

<!-- Bookmark toggle -->
<codex-switch-toggle 
  :label="t('filter.show_favorites')" 
  v-model="toggleBookmarked" 
  id="show-favorites"
/>
```

### Translation Notes

#### Minimal Translation Requirements
The Instructors component has minimal direct translation needs as it primarily orchestrates other components. Most translation is handled by:
- InstructorCard components for individual instructor display
- Filter components for search and filtering interface
- Pagination component for navigation
- Common error and loading messages

#### Integration with Child Components
The component passes translation context to child components that handle their own internationalization:
- `codex-instructor-card` components handle instructor-specific translations
- `codex-filter-wrapper` and contextual filters handle filter-related translations
- `codex-pagination` handles pagination navigation translations

## Best Practices

### Data Management
- Use proper filter context integration
- Handle loading states appropriately
- Implement bookmark functionality correctly
- Cache instructor data when possible
- Validate data structures

### User Experience
- Provide clear filtering options
- Show loading skeletons during fetch
- Handle empty states gracefully
- Ensure responsive design
- Implement proper pagination

### Performance
- Optimize instructor card rendering
- Implement lazy loading for large datasets
- Use proper component keys
- Monitor filter performance
- Cache filter results

### Accessibility
- Ensure keyboard navigation works
- Provide proper labeling
- Support screen readers
- Handle focus management
- Test with assistive technologies

### Integration
- Sync with bookmark system
- Handle filter context properly
- Test pagination integration
- Validate filter definitions
- Monitor API performance

## Component Registration
```javascript
// Global registration
app.component('CodexInstructors', Instructors)

// Local registration  
import Instructors from '@/components/instructors/Instructors.vue'

export default {
  components: {
    CodexInstructors: Instructors
  }
}
``` 