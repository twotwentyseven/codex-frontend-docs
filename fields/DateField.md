# DateField Component

## Overview
The DateField component provides a comprehensive date input with extensive calendar functionality, validation, and internationalization. It supports both date-only and date-time selection, custom date constraints, multiple display formats, timezone handling, and accessibility features. The component integrates with VueJS Datepicker for rich calendar interactions while maintaining seamless form integration.

## Basic Usage
```vue
<template>
  <div class="date-form">
    <codex-date-field
      v-model="selectedDate"
      :name="'event_date'"
      :label="'Event Date'"
      :placeholder="'Select a date'"
      :required="true"
    />
  </div>
</template>

<script setup>
const selectedDate = ref('')
</script>
```

## Key Features
- Full calendar picker with VueJS Datepicker integration
- Date-only and date-time selection modes
- Timezone support and conversion
- Custom date range constraints (min/max dates)
- Disabled dates and day-of-week restrictions
- Multiple date format options
- Internationalization with locale support
- Accessibility features with keyboard navigation
- Comprehensive validation with custom rules

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `name` | `String` | `required` | Field name for form submission |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
| `placeholder` | `String` | `''` | Input placeholder text |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the input |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |

### Date Configuration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `enableTimePicker` | `Boolean` | `false` | Enable time selection |
| `timePicker` | `Boolean` | `false` | Show only time picker |
| `range` | `Boolean` | `false` | Enable date range selection |
| `multiCalendars` | `Boolean` | `false` | Show multiple calendar months |
| `inline` | `Boolean` | `false` | Display calendar inline |

### Date Constraints Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `minDate` | `String\|Date` | `null` | Minimum selectable date |
| `maxDate` | `String\|Date` | `null` | Maximum selectable date |
| `disabledDates` | `Array` | `[]` | Array of disabled dates |
| `disabledWeekDays` | `Array` | `[]` | Disabled days of week (0-6) |
| `allowedDates` | `Array` | `[]` | Only these dates are selectable |

### Display Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `format` | `String` | `'dd/MM/yyyy'` | Date display format |
| `previewFormat` | `String` | `''` | Format for preview display |
| `locale` | `String` | `'en'` | Locale for date display |
| `weekStart` | `Number` | `1` | First day of week (0=Sunday, 1=Monday) |

### Timezone Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `timezone` | `String` | `'UTC'` | Timezone for date handling |
| `utc` | `Boolean` | `false` | Use UTC timezone |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'1'` | Layout size (auto, 4, 3, 2, 1) |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `required` | `Boolean` | `true` | Whether field is required |
| `readonly` | `Boolean` | `false` | Whether field is readonly |
| `disabled` | `Boolean` | `false` | Whether field is disabled |
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
| `update:modelValue` | `date` | Emitted when date value changes |
| `date-select` | `date` | Emitted when date is selected from calendar |
| `calendar-open` | `none` | Emitted when calendar opens |
| `calendar-close` | `none` | Emitted when calendar closes |

## Model Value

The component uses `defineModel()` with date handling:

```vue
<template>
  <!-- Date string -->
  <codex-date-field v-model="dateString" />
  
  <!-- Date object -->
  <codex-date-field v-model="dateObject" />
  
  <!-- Date range -->
  <codex-date-field 
    v-model="dateRange"
    :range="true"
  />
</template>

<script setup>
const dateString = ref('2024-03-15')
const dateObject = ref(new Date())
const dateRange = ref([new Date(), new Date()])
</script>
```

## Examples

### Event Booking Form
```vue
<template>
  <div class="event-booking">
    <h3>Book Your Event</h3>
    
    <div class="booking-form">
      <codex-date-field
        v-model="booking.eventDate"
        :name="'event_date'"
        :label="'Event Date'"
        :placeholder="'Select event date'"
        :layout="'2'"
        :required="true"
        :minDate="minBookingDate"
        :maxDate="maxBookingDate"
        :disabledWeekDays="[0]"
        :disabledDates="blackoutDates"
        :format="'DD MMMM YYYY'"
        :errors="bookingErrors.eventDate"
        :helper-text="'Events available Tuesday - Sunday'"
      />
      
      <codex-date-field
        v-model="booking.eventTime"
        :name="'event_time'"
        :label="'Event Time'"
        :placeholder="'Select time'"
        :layout="'2'"
        :required="true"
        :enableTimePicker="true"
        :timePicker="true"
        :format="'HH:mm'"
        :errors="bookingErrors.eventTime"
        :helper-text="'Available times: 9:00 AM - 10:00 PM'"
      />
      
      <codex-date-field
        v-model="booking.setupTime"
        :name="'setup_time'"
        :label="'Setup Start Time'"
        :placeholder="'Optional setup time'"
        :layout="'2'"
        :required="false"
        :enableTimePicker="true"
        :format="'DD/MM/YYYY HH:mm'"
        :minDate="booking.eventDate"
        :maxDate="booking.eventDate"
        :helper-text="'Same day as event, before event time'"
      />
      
      <div class="availability-info" v-if="availabilityInfo">
        <div class="info-card" :class="availabilityInfo.status">
          <i :class="availabilityInfo.icon"></i>
          <div>
            <h4>{{ availabilityInfo.title }}</h4>
            <p>{{ availabilityInfo.message }}</p>
          </div>
        </div>
      </div>
    </div>
    
    <div class="booking-summary" v-if="isBookingValid">
      <h4>Booking Summary</h4>
      <div class="summary-details">
        <p><strong>Date:</strong> {{ formatDate(booking.eventDate) }}</p>
        <p><strong>Time:</strong> {{ booking.eventTime }}</p>
        <p v-if="booking.setupTime"><strong>Setup:</strong> {{ formatDateTime(booking.setupTime) }}</p>
        <p><strong>Duration:</strong> {{ estimatedDuration }} hours</p>
      </div>
    </div>
  </div>
</template>

<script setup>
const booking = reactive({
  eventDate: '',
  eventTime: '',
  setupTime: ''
})

const bookingErrors = ref({
  eventDate: [],
  eventTime: []
})

// Set minimum date to tomorrow
const minBookingDate = computed(() => {
  const tomorrow = new Date()
  tomorrow.setDate(tomorrow.getDate() + 1)
  return tomorrow
})

// Set maximum date to 6 months from now
const maxBookingDate = computed(() => {
  const maxDate = new Date()
  maxDate.setMonth(maxDate.getMonth() + 6)
  return maxDate
})

// Example blackout dates (holidays, maintenance)
const blackoutDates = [
  '2024-12-25', // Christmas
  '2024-01-01', // New Year
  '2024-07-04', // Independence Day
  '2024-03-20', // Maintenance day
]

const availabilityInfo = computed(() => {
  if (!booking.eventDate) return null
  
  const selectedDate = new Date(booking.eventDate)
  const dayOfWeek = selectedDate.getDay()
  
  if (dayOfWeek === 6) { // Saturday
    return {
      status: 'warning',
      icon: 'ri-alert-line',
      title: 'Weekend Premium',
      message: 'Saturday bookings include a 25% premium charge'
    }
  } else if (dayOfWeek === 5) { // Friday
    return {
      status: 'info',
      icon: 'ri-information-line',
      title: 'Popular Day',
      message: 'Friday slots fill up quickly. Book early!'
    }
  } else {
    return {
      status: 'success',
      icon: 'ri-check-line',
      title: 'Available',
      message: 'Standard rates apply for weekday bookings'
    }
  }
})

const isBookingValid = computed(() => {
  return booking.eventDate && booking.eventTime
})

const estimatedDuration = computed(() => {
  // Calculate based on event type, default 3 hours
  return 3
})

const formatDate = (dateStr) => {
  if (!dateStr) return ''
  return new Date(dateStr).toLocaleDateString('en-US', {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}

const formatDateTime = (dateTimeStr) => {
  if (!dateTimeStr) return ''
  return new Date(dateTimeStr).toLocaleString('en-US')
}

// Validate booking times
watch([() => booking.eventDate, () => booking.eventTime, () => booking.setupTime], () => {
  validateBookingTimes()
})

const validateBookingTimes = () => {
  bookingErrors.value = { eventDate: [], eventTime: [] }
  
  if (booking.eventTime) {
    const [hours] = booking.eventTime.split(':').map(Number)
    if (hours < 9 || hours > 22) {
      bookingErrors.value.eventTime = ['Event time must be between 9:00 AM and 10:00 PM']
    }
  }
  
  if (booking.setupTime && booking.eventTime) {
    const setupTime = new Date(booking.setupTime)
    const eventDateTime = new Date(`${booking.eventDate} ${booking.eventTime}`)
    
    if (setupTime >= eventDateTime) {
      bookingErrors.value.eventTime = ['Setup time must be before event time']
    }
  }
}
</script>
```

### Employee Schedule Manager
```vue
<template>
  <div class="schedule-manager">
    <h3>Employee Schedule</h3>
    
    <div class="schedule-controls">
      <codex-date-field
        v-model="schedule.dateRange"
        :name="'schedule_range'"
        :label="'Schedule Period'"
        :placeholder="'Select date range'"
        :range="true"
        :multiCalendars="true"
        :layout="'2'"
        :required="true"
        :minDate="new Date()"
        :format="'DD/MM/YYYY'"
        :helper-text="'Select up to 4 weeks'"
        @date-select="handleDateRangeChange"
      />
      
      <codex-date-field
        v-model="schedule.workingDays"
        :name="'working_days'"
        :label="'Available Work Days'"
        :placeholder="'Select available days'"
        :layout="'2'"
        :required="true"
        :allowedDates="availableWorkDays"
        :format="'DD/MM'"
        :helper-text="'Choose your available days'"
      />
    </div>
    
    <div class="shift-times">
      <h4>Shift Times</h4>
      
      <div class="time-grid">
        <codex-date-field
          v-model="schedule.shiftStart"
          :name="'shift_start'"
          :label="'Shift Start'"
          :placeholder="'Start time'"
          :layout="'4'"
          :enableTimePicker="true"
          :timePicker="true"
          :format="'HH:mm'"
          :required="true"
        />
        
        <codex-date-field
          v-model="schedule.shiftEnd"
          :name="'shift_end'"
          :label="'Shift End'"
          :placeholder="'End time'"
          :layout="'4'"
          :enableTimePicker="true"
          :timePicker="true"
          :format="'HH:mm'"
          :required="true"
        />
        
        <codex-date-field
          v-model="schedule.breakTime"
          :name="'break_time'"
          :label="'Break Time'"
          :placeholder="'Break start'"
          :layout="'4'"
          :enableTimePicker="true"
          :timePicker="true"
          :format="'HH:mm'"
          :required="false"
          :helper-text="'Optional 30-minute break'"
        />
        
        <div class="shift-duration">
          <strong>Total Hours: {{ calculateShiftHours() }}</strong>
        </div>
      </div>
    </div>
    
    <div class="schedule-preview" v-if="isScheduleComplete">
      <h4>Schedule Preview</h4>
      <div class="preview-calendar">
        <div v-for="day in previewDays" :key="day.date" class="preview-day">
          <div class="day-header">
            <strong>{{ formatPreviewDate(day.date) }}</strong>
          </div>
          <div class="day-schedule">
            <span class="shift-time">{{ schedule.shiftStart }} - {{ schedule.shiftEnd }}</span>
            <span v-if="schedule.breakTime" class="break-time">Break: {{ schedule.breakTime }}</span>
          </div>
        </div>
      </div>
    </div>
    
    <div class="schedule-actions">
      <button @click="saveSchedule" :disabled="!isScheduleComplete" class="save-btn">
        Save Schedule
      </button>
      <button @click="requestTimeOff" class="time-off-btn">
        Request Time Off
      </button>
    </div>
  </div>
</template>

<script setup>
const schedule = reactive({
  dateRange: [],
  workingDays: [],
  shiftStart: '09:00',
  shiftEnd: '17:00',
  breakTime: '12:00'
})

const availableWorkDays = computed(() => {
  if (!schedule.dateRange || schedule.dateRange.length !== 2) return []
  
  const startDate = new Date(schedule.dateRange[0])
  const endDate = new Date(schedule.dateRange[1])
  const workDays = []
  
  const currentDate = new Date(startDate)
  while (currentDate <= endDate) {
    // Exclude weekends (Saturday = 6, Sunday = 0)
    if (currentDate.getDay() !== 0 && currentDate.getDay() !== 6) {
      workDays.push(new Date(currentDate))
    }
    currentDate.setDate(currentDate.getDate() + 1)
  }
  
  return workDays
})

const isScheduleComplete = computed(() => {
  return schedule.dateRange.length === 2 && 
         schedule.workingDays.length > 0 && 
         schedule.shiftStart && 
         schedule.shiftEnd
})

const previewDays = computed(() => {
  if (!schedule.workingDays.length) return []
  
  return schedule.workingDays.slice(0, 7).map(date => ({
    date: date,
    isWorkDay: true
  }))
})

const calculateShiftHours = () => {
  if (!schedule.shiftStart || !schedule.shiftEnd) return '0'
  
  const [startHours, startMinutes] = schedule.shiftStart.split(':').map(Number)
  const [endHours, endMinutes] = schedule.shiftEnd.split(':').map(Number)
  
  const startTime = startHours + startMinutes / 60
  const endTime = endHours + endMinutes / 60
  
  const totalHours = endTime - startTime
  const breakHours = schedule.breakTime ? 0.5 : 0
  
  return Math.max(0, totalHours - breakHours).toFixed(1)
}

const formatPreviewDate = (date) => {
  return new Date(date).toLocaleDateString('en-US', {
    weekday: 'short',
    month: 'short',
    day: 'numeric'
  })
}

const handleDateRangeChange = () => {
  // Reset working days when date range changes
  schedule.workingDays = []
}

const saveSchedule = async () => {
  try {
    await saveEmployeeSchedule(schedule)
    toast.success('Schedule saved successfully')
  } catch (error) {
    toast.error('Failed to save schedule')
  }
}

const requestTimeOff = () => {
  // Navigate to time-off request form
  router.push('/time-off-request')
}

// Validate shift times
watch([() => schedule.shiftStart, () => schedule.shiftEnd], () => {
  if (schedule.shiftStart && schedule.shiftEnd) {
    const startTime = schedule.shiftStart.replace(':', '')
    const endTime = schedule.shiftEnd.replace(':', '')
    
    if (parseInt(endTime) <= parseInt(startTime)) {
      toast.warning('End time must be after start time')
    }
  }
})
</script>
```

### Project Deadline Tracker
```vue
<template>
  <div class="deadline-tracker">
    <h3>Project Deadlines</h3>
    
    <div class="deadline-form">
      <codex-date-field
        v-model="project.startDate"
        :name="'start_date'"
        :label="'Project Start Date'"
        :placeholder="'When does the project start?'"
        :layout="'2'"
        :required="true"
        :minDate="new Date()"
        :format="'DD MMM YYYY'"
        :locale="'en-GB'"
        :errors="projectErrors.startDate"
      />
      
      <codex-date-field
        v-model="project.deadline"
        :name="'deadline'"
        :label="'Project Deadline'"
        :placeholder="'Final deadline'"
        :layout="'2'"
        :required="true"
        :minDate="project.startDate || new Date()"
        :format="'DD MMM YYYY'"
        :locale="'en-GB'"
        :errors="projectErrors.deadline"
        :helper-text="deadlineHelperText"
      />
      
      <codex-date-field
        v-model="project.milestones"
        :name="'milestones'"
        :label="'Key Milestones'"
        :placeholder="'Select milestone dates'"
        :layout="'1'"
        :required="false"
        :allowedDates="availableMilestoneDates"
        :format="'DD MMM'"
        :helper-text="'Choose important milestone dates within project timeline'"
      />
      
      <codex-date-field
        v-model="project.reviewDates"
        :name="'review_dates'"
        :label="'Review Schedule'"
        :placeholder="'Select review dates'"
        :layout="'1'"
        :required="true"
        :disabledWeekDays="[0, 6]"
        :format="'DD MMM YYYY'"
        :helper-text="'Weekly reviews on weekdays only'"
      />
    </div>
    
    <div class="timeline-visualization" v-if="isProjectTimelineValid">
      <h4>Project Timeline</h4>
      <div class="timeline">
        <div class="timeline-item start">
          <div class="timeline-marker"></div>
          <div class="timeline-content">
            <strong>Project Start</strong>
            <span>{{ formatTimelineDate(project.startDate) }}</span>
          </div>
        </div>
        
        <div v-for="milestone in sortedMilestones" :key="milestone" class="timeline-item milestone">
          <div class="timeline-marker"></div>
          <div class="timeline-content">
            <strong>Milestone</strong>
            <span>{{ formatTimelineDate(milestone) }}</span>
          </div>
        </div>
        
        <div class="timeline-item deadline">
          <div class="timeline-marker"></div>
          <div class="timeline-content">
            <strong>Final Deadline</strong>
            <span>{{ formatTimelineDate(project.deadline) }}</span>
            <span class="days-remaining">{{ daysUntilDeadline }} days</span>
          </div>
        </div>
      </div>
    </div>
    
    <div class="project-analytics" v-if="isProjectTimelineValid">
      <div class="analytics-grid">
        <div class="metric-card">
          <h5>Project Duration</h5>
          <span class="metric-value">{{ projectDurationDays }} days</span>
        </div>
        
        <div class="metric-card">
          <h5>Time Remaining</h5>
          <span class="metric-value" :class="timeRemainingClass">{{ daysUntilDeadline }} days</span>
        </div>
        
        <div class="metric-card">
          <h5>Progress Checkpoints</h5>
          <span class="metric-value">{{ project.milestones.length + project.reviewDates.length }}</span>
        </div>
        
        <div class="metric-card">
          <h5>Working Days</h5>
          <span class="metric-value">{{ calculateWorkingDays() }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const project = reactive({
  startDate: '',
  deadline: '',
  milestones: [],
  reviewDates: []
})

const projectErrors = ref({
  startDate: [],
  deadline: []
})

const availableMilestoneDates = computed(() => {
  if (!project.startDate || !project.deadline) return []
  
  const startDate = new Date(project.startDate)
  const endDate = new Date(project.deadline)
  const milestoneDates = []
  
  const currentDate = new Date(startDate)
  currentDate.setDate(currentDate.getDate() + 7) // Start from week 1
  
  while (currentDate < endDate) {
    milestoneDates.push(new Date(currentDate))
    currentDate.setDate(currentDate.getDate() + 7) // Weekly intervals
  }
  
  return milestoneDates
})

const deadlineHelperText = computed(() => {
  if (!project.startDate) return 'Select start date first'
  if (!project.deadline) return 'Choose your project deadline'
  
  const duration = projectDurationDays.value
  if (duration < 7) return 'Very short project timeline'
  if (duration < 30) return 'Short-term project'
  if (duration < 90) return 'Medium-term project'
  return 'Long-term project'
})

const isProjectTimelineValid = computed(() => {
  return project.startDate && project.deadline
})

const projectDurationDays = computed(() => {
  if (!project.startDate || !project.deadline) return 0
  
  const start = new Date(project.startDate)
  const end = new Date(project.deadline)
  const diffTime = Math.abs(end - start)
  
  return Math.ceil(diffTime / (1000 * 60 * 60 * 24))
})

const daysUntilDeadline = computed(() => {
  if (!project.deadline) return 0
  
  const today = new Date()
  const deadline = new Date(project.deadline)
  const diffTime = deadline - today
  
  return Math.ceil(diffTime / (1000 * 60 * 60 * 24))
})

const timeRemainingClass = computed(() => {
  const days = daysUntilDeadline.value
  if (days < 7) return 'urgent'
  if (days < 30) return 'warning'
  return 'normal'
})

const sortedMilestones = computed(() => {
  return [...project.milestones].sort((a, b) => new Date(a) - new Date(b))
})

const formatTimelineDate = (dateStr) => {
  if (!dateStr) return ''
  return new Date(dateStr).toLocaleDateString('en-GB', {
    day: 'numeric',
    month: 'short',
    year: 'numeric'
  })
}

const calculateWorkingDays = () => {
  if (!project.startDate || !project.deadline) return 0
  
  const startDate = new Date(project.startDate)
  const endDate = new Date(project.deadline)
  let workingDays = 0
  
  const currentDate = new Date(startDate)
  while (currentDate <= endDate) {
    const dayOfWeek = currentDate.getDay()
    if (dayOfWeek !== 0 && dayOfWeek !== 6) { // Exclude weekends
      workingDays++
    }
    currentDate.setDate(currentDate.getDate() + 1)
  }
  
  return workingDays
}

// Validation watchers
watch(() => project.deadline, (newDeadline) => {
  if (newDeadline && project.startDate) {
    const startDate = new Date(project.startDate)
    const endDate = new Date(newDeadline)
    
    if (endDate <= startDate) {
      projectErrors.value.deadline = ['Deadline must be after start date']
    } else {
      projectErrors.value.deadline = []
    }
  }
})

watch(() => project.startDate, (newStartDate) => {
  if (newStartDate && project.deadline) {
    const startDate = new Date(newStartDate)
    const endDate = new Date(project.deadline)
    
    if (startDate >= endDate) {
      projectErrors.value.startDate = ['Start date must be before deadline']
    } else {
      projectErrors.value.startDate = []
    }
  }
  
  // Reset milestones when start date changes
  project.milestones = []
})
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
| `_c-placeholder` | Placeholder state styling when no date selected |

## Best Practices

### Date Selection Design
- Use appropriate date constraints to guide user selection
- Provide clear visual feedback for disabled/unavailable dates
- Consider business rules when setting min/max dates
- Use meaningful placeholder text that indicates expected format

### Timezone Handling
- Be explicit about timezone handling in your application
- Use UTC for data storage and convert for display
- Clearly communicate which timezone is being used
- Consider user's local timezone for date display

### Validation
- Validate date ranges and business logic constraints
- Provide immediate feedback for invalid date selections
- Handle edge cases like leap years and month boundaries
- Consider cultural differences in date interpretation

### Accessibility
- Ensure calendar navigation works with keyboard
- Provide clear labels and ARIA attributes
- Test with screen readers and keyboard navigation
- Use semantic HTML for date inputs

### Performance
- Limit date range calculations for very large ranges
- Cache formatted date strings when possible
- Use computed properties for dependent date calculations
- Debounce validation for complex date rules

### Internationalization
- Use appropriate locale settings for date formatting
- Consider different calendar systems (Gregorian, Islamic, etc.)
- Handle right-to-left languages properly
- Test with different date format preferences

### User Experience
- Show calendar inline for frequent date selection
- Use date ranges for booking and scheduling scenarios
- Provide helpful date shortcuts (today, tomorrow, next week)
- Consider mobile-friendly date picker interactions

## Component Registration
```javascript
// Global registration
app.component('CodexDateField', DateField)

// Local registration  
import DateField from '@/components/fields/DateField.vue'

export default {
  components: {
    CodexDateField: DateField
  }
}
``` 