# Milestones Component

## Overview
The Milestones component provides a gamified progress tracking interface with visual milestone cards, circular progress charts, and achievement modals. It features customer statistics tracking, customizable milestone steps, progress visualization, achievement notifications, and bookmark integration for milestone completion tracking.

## Basic Usage
```vue
<codex-milestones
  :type="'bookings_attended'"
  :title="'Your Progress'"
  :title-tag="'h2'"
/>
```

## Key Features
- Visual milestone progress tracking
- Circular progress chart visualization
- Customizable milestone steps and goals
- Achievement modal notifications
- Customer statistics integration
- Bookmark system for milestone tracking
- Progress-based milestone unlocking
- Completion status management
- Responsive milestone cards
- Configurable chart styling

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| type | String | No | 'bookings_attended' | Type of milestone tracking ('bookings_attended', 'videos_watched') |
| chartType | String | No | 'circle' | Chart visualization type ('circle', 'progress_bar') |
| steps | Object | No | `{bronze: 25, silver: 50, gold: 75, platinum: 100}` | Milestone steps and goal values |
| completedMsg | String\|Boolean | No | false | Custom message for all milestones completed |
| circleData | Object | No | `{diameter: 50, strokeWidth: 20}` | Circle chart configuration |
| title | String | No | undefined | Title for the milestones section |
| titleTag | String | No | 'h1' | HTML tag for the title element |

### Common Props
All common props from `@/config/common` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| milestone-achieved | `milestone: String, value: Number` | Emitted when a milestone is completed |
| milestone-modal-closed | `milestone: String` | Emitted when achievement modal is closed |

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content -->
</template>
```

### Milestone Modal Slot
```vue
<template #milestone-modal="{ key, value }">
  <!-- Custom achievement modal content -->
</template>
```

## Milestone Types
The component supports different tracking types:

### Bookings Attended
- **Selector**: `total_unique_bookings_attended`
- **Description**: Tracks unique class bookings attended
- **Default Message**: "You've attended {value} class{es}!"

### Videos Watched
- **Selector**: `total_videos_completed`
- **Description**: Tracks completed video content
- **Default Message**: "You've watched {value} video{s}!"

## Progress Visualization
The component uses circular progress charts:
- **SVG Implementation**: Scalable vector graphics for crisp display
- **Animated Progress**: Visual progress indication
- **Customizable Styling**: Configurable diameter and stroke width
- **Accessibility**: Screen reader friendly with proper titles

## Milestone System
Milestone progression follows these rules:
- **Sequential Unlocking**: Milestones unlock in order
- **Progress Tracking**: Current progress compared to goals
- **Completion Detection**: Automatic achievement detection
- **Modal Notifications**: Achievement modals for new completions
- **Bookmark Integration**: Prevents duplicate notifications

## Chart Configuration
Circle chart properties can be customized:
```javascript
{
  diameter: 50,        // Chart diameter in pixels
  strokeWidth: 20      // Stroke width for progress ring
}
```

## Default Milestone Steps
The component includes default progression:
```javascript
{
  bronze: 25,          // Bronze milestone at 25
  silver: 50,          // Silver milestone at 50
  gold: 75,            // Gold milestone at 75
  platinum: 100        // Platinum milestone at 100
}
```

## Internationalization

The `Milestones` component uses translation keys for milestone progress tracking and completion messaging:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `milestone.milestone_progress` | Accessibility title for progress chart | Screen reader support for SVG chart |
| `milestone.out_of` | Progress separator in accessibility title | "X out of Y" milestone progress |
| `milestones.all_milestones_completed` | Completion message when all milestones reached | Maximum milestone achievement |

### Implementation Examples

```vue
<!-- Accessibility title for milestone chart -->
<svg :viewBox="`0 0 ${circle.viewbox} ${circle.viewbox}`">
    <title>
        {{ $t('milestone.milestone_progress') }} {{ Math.min(currentProgress, value) }} 
        {{ $t('milestone.out_of') }} {{ value }}
    </title>
    <!-- Chart elements -->
</svg>

<!-- All milestones completed message -->
<div v-else class="cdx_milestone-completed">
    {{ completedMsg || $t('milestones.all_milestones_completed') }}
</div>
```

### Dynamic Milestone Messages

The component includes hardcoded English messages that should be converted to translation keys:

```javascript
// Current hardcoded implementation - needs translation conversion
function generateMilestoneMessage(value) {
    switch (props.type) {
        case 'bookings_attended':
            return `You've attended ${value} class${value === 1 ? '!' : "es!"}`;
        case 'videos_watched':
            return `You've watched ${value} video${value === 1 ? '!' : "s!"}`;
        default:
            return props.type;
    }
}
```

### Recommended Translation Keys for Dynamic Messages

```javascript
// Recommended translation structure
function generateMilestoneMessage(value) {
    switch (props.type) {
        case 'bookings_attended':
            return t('milestone.classes_attended', { count: value });
        case 'videos_watched':
            return t('milestone.videos_watched', { count: value });
        default:
            return t(`milestone.${props.type}`, { count: value });
    }
}
```

### Required Translation Keys for Full Internationalization

| Translation Key | Usage | Parameters |
|----------------|-------|------------|
| `milestone.classes_attended` | Bookings milestone completion message | `count` (number of classes) |
| `milestone.videos_watched` | Videos milestone completion message | `count` (number of videos) |

### Milestone Configuration

The component supports configurable milestone steps:

```javascript
steps: {
    type: Object,
    default: function () {
        return {
            bronze: 25,
            silver: 50,
            gold: 75,
            platinum: 100
        };
    }
}
```

### Accessibility Features

- SVG chart includes translated accessibility titles
- Milestone names (bronze, silver, gold, platinum) are displayed as provided in configuration
- Progress values are numeric and don't require translation

### Notes
- Milestone names in the `steps` object could be translation keys for full internationalization
- Component includes modal system for milestone celebrations (slots can provide translated content)
- Progress calculations and chart rendering are numeric and language-agnostic
- Custom completion messages can be provided through the `completedMsg` prop

## Examples

### Basic Implementation
```vue
<codex-milestones />
```

### Custom Milestone Type
```vue
<codex-milestones
  :type="'videos_watched'"
  :title="'Video Progress'"
/>
```

### Custom Steps Configuration
```vue
<codex-milestones
  :type="'bookings_attended'"
  :steps="{
    beginner: 10,
    intermediate: 25,
    advanced: 50,
    expert: 100,
    master: 200
  }"
/>
```

### Custom Chart Styling
```vue
<codex-milestones
  :circle-data="{
    diameter: 80,
    strokeWidth: 12
  }"
  :chart-type="'circle'"
/>
```

### Custom Header
```vue
<codex-milestones>
  <template #header>
    <div class="milestones-header">
      <h2>Achievement Progress</h2>
      <p>Track your fitness journey with our milestone system</p>
      <div class="progress-summary">
        Current Level: {{ currentLevel }}
      </div>
    </div>
  </template>
</codex-milestones>
```

### Custom Achievement Modal
```vue
<codex-milestones>
  <template #milestone-modal="{ key, value }">
    <div class="custom-achievement-modal">
      <div class="achievement-animation">
        <i class="trophy-icon"></i>
      </div>
      <h3>Congratulations!</h3>
      <p>You've reached the {{ key }} milestone!</p>
      <p>{{ generateCustomMessage(key, value) }}</p>
      <div class="achievement-rewards">
        <p>You've earned:</p>
        <ul>
          <li>{{ getRewardForMilestone(key) }}</li>
          <li>Special badge</li>
          <li>Bragging rights!</li>
        </ul>
      </div>
      <button @click="shareAchievement(key, value)">Share Achievement</button>
      <button @click="closeModal(key)">Continue</button>
    </div>
  </template>
</codex-milestones>
```

### With Event Tracking
```vue
<codex-milestones
  @milestone-achieved="trackMilestoneAchievement"
  @milestone-modal-closed="trackModalClosure"
/>

<script setup>
const trackMilestoneAchievement = (milestone, value) => {
  analytics.track('milestone_achieved', {
    milestone_type: milestone,
    milestone_value: value,
    customer_id: customer.value?.id
  })
}

const trackModalClosure = (milestone) => {
  analytics.track('achievement_modal_closed', {
    milestone_type: milestone,
    customer_id: customer.value?.id
  })
}
</script>
```

### Progress Dashboard Integration
```vue
<div class="progress-dashboard">
  <div class="stats-overview">
    <div class="stat-card">
      <h3>Classes Attended</h3>
      <span class="stat-value">{{ customer.stats.total_unique_bookings_attended }}</span>
    </div>
    <div class="stat-card">
      <h3>Videos Watched</h3>
      <span class="stat-value">{{ customer.stats.total_videos_completed }}</span>
    </div>
  </div>
  
  <div class="milestone-sections">
    <div class="milestone-group">
      <h3>Class Milestones</h3>
      <codex-milestones 
        :type="'bookings_attended'"
        :title="false"
      />
    </div>
    
    <div class="milestone-group">
      <h3>Video Milestones</h3>
      <codex-milestones 
        :type="'videos_watched'"
        :title="false"
      />
    </div>
  </div>
</div>
```

### Gamification Integration
```