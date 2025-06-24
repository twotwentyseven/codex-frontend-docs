# ContextualFilter Component

## Overview
The ContextualFilter component provides a flexible filtering interface that can render different types of filter inputs (checkbox, radio, select, text, number) based on configuration. It integrates with the filter context system to manage filter state and automatically fetches options from APIs or static sources. The component supports dynamic filter definitions and provides a standardized way to implement filtering across different data models.

## Basic Usage
```vue
<template>
  <div class="filter-interface">
    <codex-contextual-filter
      :group="'products'"
      :definition="'categories.default'"
      :filter-key="'category_id'"
      :label="'Product Category'"
      :filter-type="'checkbox-group'"
    />
  </div>
</template>

<script setup>
// Filter context should be provided at a higher level
</script>
```

## Key Features
- Multiple filter input types (checkbox, radio, select, text, number)
- Dynamic option loading from API endpoints or static sources
- Integration with filter context for state management
- Support for filter definitions via configuration files
- Automatic registration with parent filter context
- Internationalization support for placeholders and labels
- Flexible layout configurations
- Button variant support for checkbox groups
- Primary/secondary filter styling options
- Slot support for custom filter implementations

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `definition` | `String\|Object` | `required` | Filter definition string (model.key) or object |
| `filterKey` | `String` | `required` | Unique key for this filter instance |

### Filter Configuration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `group` | `String` | `'default'` | Filter group identifier for context |
| `filterType` | `String` | `undefined` | Override filter type from definition |
| `filters` | `Object` | `{}` | Current filter values object |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Display label for the filter |
| `primaryFilter` | `Boolean` | `false` | Whether this is a primary filter (affects styling) |

### Common Props
| Prop | Type | Description |
|------|------|-------------|
| `...commonProps` | `Various` | Inherits common props from config |

## Filter Types

The component supports the following filter types:

### Checkbox Group
```vue
<codex-contextual-filter
  :filter-type="'checkbox-group'"
  :definition="'categories'"
  :filter-key="'category_ids'"
  :label="'Categories'"
/>
```

### Single Checkbox
```vue
<codex-contextual-filter
  :filter-type="'checkbox'"
  :definition="'featured'"
  :filter-key="'is_featured'"
  :label="'Featured Only'"
/>
```

### Radio Buttons
```vue
<codex-contextual-filter
  :filter-type="'radio'"
  :definition="'status'"
  :filter-key="'status'"
  :label="'Status'"
/>
```

### Select Dropdown
```vue
<codex-contextual-filter
  :filter-type="'select'"
  :definition="'brands'"
  :filter-key="'brand_id'"
  :label="'Brand'"
/>
```

### Multi-Select
```vue
<codex-contextual-filter
  :filter-type="'multi-select'"
  :definition="'tags'"
  :filter-key="'tag_ids'"
  :label="'Tags'"
/>
```

### Text Input
```vue
<codex-contextual-filter
  :filter-type="'text'"
  :definition="'search'"
  :filter-key="'search_query'"
  :label="'Search'"
/>
```

### Number Input
```vue
<codex-contextual-filter
  :filter-type="'number'"
  :definition="'price'"
  :filter-key="'min_price'"
  :label="'Minimum Price'"
/>
```

## Filter Definitions

Filter definitions can be strings referencing configuration objects or direct objects:

### String Definition
```javascript
// References filterDefinitions['products']['categories']
definition="products.categories"

// References filterDefinitions['products']['default']
definition="products"
```

### Object Definition
```javascript
const customDefinition = {
  type: 'select',
  source: 'api',
  model: CategoryModel,
  labelField: 'name',
  key: 'id',
  orderBy: 'name'
}
```

## Slot Props

| Slot | Props | Description |
|------|-------|-------------|
| `default` | `{ filters }` | Access to current filter state |

## Examples

### E-commerce Product Filters
```vue
<template>
  <div class="product-filters">
    <h3>Filter Products</h3>
    
    <codex-filter-context :group="'products'" :default-values="defaultFilters">
      <template #default="{ filters, combinedFilters }">
        <div class="filter-sections">
          <!-- Primary Filters (Always Visible) -->
          <div class="primary-filters">
            <codex-contextual-filter
              :group="'products'"
              :definition="'categories.default'"
              :filter-key="'category_id'"
              :label="'Category'"
              :filter-type="'checkbox-group'"
              :primary-filter="true"
            />
            
            <codex-contextual-filter
              :group="'products'"
              :definition="'brands.popular'"
              :filter-key="'brand_id'"
              :label="'Popular Brands'"
              :filter-type="'checkbox-group'"
              :primary-filter="true"
            />
            
            <codex-contextual-filter
              :group="'products'"
              :definition="'price_ranges'"
              :filter-key="'price_range'"
              :label="'Price Range'"
              :filter-type="'radio'"
              :primary-filter="true"
            />
          </div>
          
          <!-- Search and Text Filters -->
          <div class="search-filters">
            <codex-contextual-filter
              :group="'products'"
              :definition="searchDefinition"
              :filter-key="'search'"
              :label="'Search Products'"
              :filter-type="'text'"
            />
            
            <codex-contextual-filter
              :group="'products'"
              :definition="'featured'"
              :filter-key="'featured_only'"
              :label="'Featured Products Only'"
              :filter-type="'checkbox'"
            />
          </div>
          
          <!-- Advanced Filters (Secondary) -->
          <div class="advanced-filters">
            <h4>Advanced Filters</h4>
            
            <codex-contextual-filter
              :group="'products'"
              :definition="'materials'"
              :filter-key="'material_ids'"
              :label="'Materials'"
              :filter-type="'multi-select'"
            />
            
            <codex-contextual-filter
              :group="'products'"
              :definition="'sizes'"
              :filter-key="'size_ids'"
              :label="'Available Sizes'"
              :filter-type="'checkbox-group'"
            />
            
            <codex-contextual-filter
              :group="'products'"
              :definition="'colors'"
              :filter-key="'color_ids'"
              :label="'Colors'"
              :filter-type="'checkbox-group'"
            />
            
            <codex-contextual-filter
              :group="'products'"
              :definition="'ratings'"
              :filter-key="'min_rating'"
              :label="'Minimum Rating'"
              :filter-type="'select'"
            />
            
            <codex-contextual-filter
              :group="'products'"
              :definition="priceDefinition"
              :filter-key="'min_price'"
              :label="'Minimum Price'"
              :filter-type="'number'"
            />
            
            <codex-contextual-filter
              :group="'products'"
              :definition="priceDefinition"
              :filter-key="'max_price'"
              :label="'Maximum Price'"
              :filter-type="'number'"
            />
          </div>
        </div>
        
        <!-- Filter Results Summary -->
        <div class="filter-summary" v-if="hasActiveFilters(filters)">
          <h4>Active Filters</h4>
          <div class="active-filters">
            <div v-for="(value, key) in getActiveFilters(filters)" :key="key" class="filter-tag">
              <span class="filter-label">{{ getFilterLabel(key) }}:</span>
              <span class="filter-value">{{ formatFilterValue(key, value) }}</span>
              <button @click="clearFilter(key)" class="remove-filter">
                <i class="ri-close-line"></i>
              </button>
            </div>
          </div>
          
          <div class="filter-actions">
            <button @click="clearAllFilters" class="clear-all-btn">
              Clear All Filters
            </button>
            <button @click="applyFilters" class="apply-filters-btn">
              Apply Filters ({{ getFilteredProductCount(combinedFilters) }} products)
            </button>
          </div>
        </div>
        
        <!-- Product Results -->
        <div class="product-results">
          <product-list :filters="combinedFilters" />
        </div>
      </template>
    </codex-filter-context>
  </div>
</template>

<script setup>
const defaultFilters = {
  category_id: [],
  featured_only: false,
  min_rating: '',
  search: ''
}

const searchDefinition = {
  type: 'text',
  source: 'static'
}

const priceDefinition = {
  type: 'number',
  source: 'static'
}

const hasActiveFilters = (filters) => {
  return Object.values(filters).some(value => {
    if (Array.isArray(value)) return value.length > 0
    if (typeof value === 'string') return value !== ''
    if (typeof value === 'boolean') return value === true
    return value !== null && value !== undefined
  })
}

const getActiveFilters = (filters) => {
  const active = {}
  Object.entries(filters).forEach(([key, value]) => {
    if (Array.isArray(value) && value.length > 0) active[key] = value
    else if (typeof value === 'string' && value !== '') active[key] = value
    else if (typeof value === 'boolean' && value === true) active[key] = value
    else if (typeof value === 'number' && value > 0) active[key] = value
  })
  return active
}

const getFilterLabel = (key) => {
  const labels = {
    category_id: 'Category',
    brand_id: 'Brand',
    material_ids: 'Materials',
    size_ids: 'Sizes',
    color_ids: 'Colors',
    min_rating: 'Rating',
    featured_only: 'Featured',
    search: 'Search',
    min_price: 'Min Price',
    max_price: 'Max Price'
  }
  return labels[key] || key
}

const formatFilterValue = (key, value) => {
  if (Array.isArray(value)) {
    return value.length > 3 ? `${value.length} selected` : value.join(', ')
  }
  if (typeof value === 'boolean') {
    return value ? 'Yes' : 'No'
  }
  return value.toString()
}

const clearFilter = (key) => {
  // Implementation would update filter context
  console.log(`Clearing filter: ${key}`)
}

const clearAllFilters = () => {
  // Implementation would reset all filters
  console.log('Clearing all filters')
}

const applyFilters = () => {
  // Implementation would trigger product search
  console.log('Applying filters')
}

const getFilteredProductCount = (filters) => {
  // Implementation would return actual count
  return Math.floor(Math.random() * 500) + 50
}
</script>
```

### Content Management Filters
```vue
<template>
  <div class="content-filters">
    <h3>Content Library Filters</h3>
    
    <codex-filter-context :group="'content'" :fixed-values="{ status: 'published' }">
      <template #default="{ filters, combinedFilters }">
        <div class="content-filter-interface">
          <!-- Content Type Filters -->
          <div class="filter-section">
            <h4>Content Type</h4>
            
            <codex-contextual-filter
              :group="'content'"
              :definition="'content_types'"
              :filter-key="'content_type'"
              :label="'Type'"
              :filter-type="'radio'"
              :primary-filter="true"
            />
          </div>
          
          <!-- Author and Category Filters -->
          <div class="filter-section">
            <h4>Organization</h4>
            
            <codex-contextual-filter
              :group="'content'"
              :definition="'authors.active'"
              :filter-key="'author_id'"
              :label="'Author'"
              :filter-type="'select'"
            />
            
            <codex-contextual-filter
              :group="'content'"
              :definition="'categories.content'"
              :filter-key="'category_ids'"
              :label="'Categories'"
              :filter-type="'checkbox-group'"
            />
            
            <codex-contextual-filter
              :group="'content'"
              :definition="'tags.popular'"
              :filter-key="'tag_ids'"
              :label="'Tags'"
              :filter-type="'multi-select'"
            />
          </div>
          
          <!-- Date and Metrics Filters -->
          <div class="filter-section">
            <h4>Publishing & Performance</h4>
            
            <codex-contextual-filter
              :group="'content'"
              :definition="dateRangeDefinition"
              :filter-key="'date_range'"
              :label="'Published Date Range'"
              :filter-type="'select'"
            />
            
            <codex-contextual-filter
              :group="'content'"
              :definition="viewsDefinition"
              :filter-key="'min_views'"
              :label="'Minimum Views'"
              :filter-type="'number'"
            />
            
            <codex-contextual-filter
              :group="'content'"
              :definition="'featured_content'"
              :filter-key="'is_featured'"
              :label="'Featured Content'"
              :filter-type="'checkbox'"
            />
            
            <codex-contextual-filter
              :group="'content'"
              :definition="'trending_content'"
              :filter-key="'is_trending'"
              :label="'Trending'"
              :filter-type="'checkbox'"
            />
          </div>
          
          <!-- Search and Advanced Filters -->
          <div class="filter-section">
            <h4>Search & Advanced</h4>
            
            <codex-contextual-filter
              :group="'content'"
              :definition="searchDefinition"
              :filter-key="'search'"
              :label="'Search Content'"
              :filter-type="'text'"
            />
            
            <codex-contextual-filter
              :group="'content'"
              :definition="'reading_time'"
              :filter-key="'reading_time_range'"
              :label="'Reading Time'"
              :filter-type="'select'"
            />
            
            <codex-contextual-filter
              :group="'content'"
              :definition="'difficulty_level'"
              :filter-key="'difficulty'"
              :label="'Difficulty Level'"
              :filter-type="'radio'"
            />
          </div>
        </div>
        
        <!-- Filter Status and Results -->
        <div class="filter-status">
          <div class="status-summary">
            <div class="results-count">
              <span class="count">{{ getContentCount(combinedFilters) }}</span>
              <span class="label">articles found</span>
            </div>
            
            <div class="filter-stats">
              <div class="stat-item">
                <span class="stat-value">{{ getActiveFilterCount(filters) }}</span>
                <span class="stat-label">active filters</span>
              </div>
              <div class="stat-item">
                <span class="stat-value">{{ getAverageReadingTime(combinedFilters) }}min</span>
                <span class="stat-label">avg reading time</span>
              </div>
            </div>
          </div>
          
          <div class="quick-actions">
            <button @click="saveFilterPreset" class="save-preset-btn">
              Save Filter Preset
            </button>
            <button @click="exportResults" class="export-btn">
              Export Results
            </button>
            <button @click="resetToDefaults" class="reset-btn">
              Reset to Defaults
            </button>
          </div>
        </div>
        
        <!-- Content Results -->
        <div class="content-results">
          <content-list :filters="combinedFilters" />
        </div>
      </template>
    </codex-filter-context>
  </div>
</template>

<script setup>
const dateRangeDefinition = {
  type: 'select',
  source: {
    'last_week': 'Last Week',
    'last_month': 'Last Month',
    'last_quarter': 'Last Quarter',
    'last_year': 'Last Year',
    'custom': 'Custom Range'
  }
}

const viewsDefinition = {
  type: 'number',
  source: 'static'
}

const searchDefinition = {
  type: 'text',
  source: 'static'
}

const getContentCount = (filters) => {
  // Implementation would return actual count
  return Math.floor(Math.random() * 1000) + 100
}

const getActiveFilterCount = (filters) => {
  return Object.values(filters).filter(value => {
    if (Array.isArray(value)) return value.length > 0
    if (typeof value === 'boolean') return value === true
    if (typeof value === 'string') return value !== ''
    return value !== null && value !== undefined
  }).length
}

const getAverageReadingTime = (filters) => {
  // Implementation would calculate based on filtered content
  return Math.floor(Math.random() * 10) + 3
}

const saveFilterPreset = () => {
  console.log('Saving filter preset')
}

const exportResults = () => {
  console.log('Exporting results')
}

const resetToDefaults = () => {
  console.log('Resetting to defaults')
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-contextual-filter` | Main container class |
| `_c-flex` | Flexbox layout |
| `_c-items-center` | Center align items |
| `_c-w-full` | Full width |
| `_c-gap-md` | Medium gap spacing |
| `_c-wrap` | Flex wrap |
| `_c-order-0` | Order 0 for layout |
| `_c-desc` | Description text styling |
| `_c-subtitle` | Subtitle text styling |
| `_c-divider` | Border divider styling |

## Internationalization

The component uses the following translation keys:

| Key | Default | Usage |
|-----|---------|-------|
| `filter.select_placeholder` | `"Select an option"` | Select field placeholder |
| `filter.multi_select_placeholder` | `"Select multiple options"` | Multi-select placeholder |

## Best Practices

### Filter Configuration
- Use consistent filter definition structures across your application
- Implement proper API caching for filter options to improve performance
- Consider using lazy loading for filter options with large datasets
- Group related filters together for better user experience
- Provide sensible default values for common filters

### Performance
- Implement debouncing for text-based filters to reduce API calls
- Cache filter option data when possible
- Use efficient data structures for filter state management
- Consider virtual scrolling for large option lists
- Optimize filter API endpoints with proper indexing

### User Experience
- Provide clear visual feedback when filters are loading
- Show filter result counts to help users understand impact
- Allow users to save and restore filter presets
- Implement filter history for easy navigation
- Group primary and secondary filters appropriately

### Accessibility
- Ensure proper ARIA labels for all filter inputs
- Support keyboard navigation through filter options
- Test with screen readers to ensure proper announcement
- Provide clear instructions for complex filter interactions
- Use sufficient color contrast for filter states

### State Management
- Use consistent filter group naming across related components
- Implement proper filter state persistence (URL, localStorage)
- Handle filter state synchronization across multiple components
- Provide clear filter reset mechanisms
- Validate filter values both client and server side

### Integration
- Register filters properly with the filter context system
- Handle loading states during filter option fetching
- Implement proper error handling for failed API requests
- Ensure filters work well with pagination systems
- Test filter combinations thoroughly for edge cases

## Component Registration
```javascript
// Global registration
app.component('CodexContextualFilter', ContextualFilter)

// Local registration  
import ContextualFilter from '@/components/filters/ContextualFilter.vue'

export default {
  components: {
    CodexContextualFilter: ContextualFilter
  }
}
``` 