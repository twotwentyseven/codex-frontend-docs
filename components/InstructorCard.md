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