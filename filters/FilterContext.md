# FilterContext Component

## Overview
The FilterContext component provides a centralized state management system for filter functionality. It manages filter values, combines fixed and dynamic filters, handles URL synchronization, tracks filter loading states, and provides dependency injection for child filter components. This component serves as the foundation for complex filtering systems and enables seamless coordination between multiple filter components.

## Basic Usage
```vue
<template>
  <div class="filterable-content">
    <codex-filter-context 
      :group="'products'" 
      :default-values="defaultFilters"
      :fixed-values="fixedFilters"
    >
      <template #default="{ filters, combinedFilters }">
        <div class="filter-interface">
          <!-- Filter components automatically connect to this context -->
          <codex-contextual-filter
            :group="'products'"
            :definition="'categories'"
            :filter-key="'category_id'"
            :label="'Category'"
          />
          
          <!-- Content that uses the filters -->
          <product-list :filters="combinedFilters" />
        </div>
      </template>
    </codex-filter-context>
  </div>
</template>

<script setup>
const defaultFilters = {
  category_id: [],
  search: '',
  price_range: ''
}

const fixedFilters = {
  status: 'active'
}
</script>
```

## Key Features
- Centralized filter state management with reactive updates
- URL synchronization for shareable filter states (configurable)
- Fixed values support for persistent filters
- Default values initialization for common filter states
- Filter loading state tracking for async operations
- Dependency injection system for child components
- Combined filter computation for API integration
- Filter registration system for component coordination
- Pagination state management integration

## Configuration Props

### Context Configuration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `group` | `String` | `'default'` | Unique identifier for this filter context |
| `config` | `String` | `undefined` | Configuration key reference |

### Filter Value Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `fixedValues` | `Object` | `{}` | Static filter values that cannot be changed |
| `defaultValues` | `Object` | `{}` | Initial filter values |

### URL Integration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `acceptUrlFilters` | `String\|Array` | `[]` | Filter keys to sync with URL parameters |

### Pagination Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `paginate` | `Boolean` | `false` | Whether to include pagination state management |

## Slot Props

| Slot | Props | Description |
|------|-------|-------------|
| `default` | `{ filters, fixedValues, combinedFilters }` | Provides access to all filter states |

### Slot Prop Details

#### `filters`
Reactive object containing user-controlled filter values:
```javascript
{
  category_id: [1, 3, 5],
  search: 'laptop',
  price_range: '100-500',
  rating: 4
}
```

#### `fixedValues`
Static filter values that remain constant:
```javascript
{
  status: 'published',
  type: 'product'
}
```

#### `combinedFilters`
Computed combination of filters and fixedValues:
```javascript
{
  category_id: [1, 3, 5],
  search: 'laptop',
  price_range: '100-500',
  rating: 4,
  status: 'published',
  type: 'product'
}
```

## Provided Dependencies

The component provides the following dependencies via Vue's inject system:

| Provided Key | Type | Description |
|--------------|------|-------------|
| `fixedValues:{group}` | `Object` | Reactive fixed filter values |
| `filters:{group}` | `Object` | Reactive user filter values |
| `filterStates:{group}` | `Object` | Filter loading states |
| `readyState:{group}` | `ComputedRef` | Whether all filters are loaded |
| `registerContextualFilter:{group}` | `Function` | Register filter components |
| `updateState:{group}` | `Function` | Update filter loading state |
| `combinedFilters:{group}` | `ComputedRef` | Combined filter values |
| `pagination:{group}` | `Object` | Pagination state management |
| `updatePagination:{group}` | `Function` | Update pagination state |

## URL Synchronization

The component supports URL synchronization for filter persistence:

### JSON Format (Default)
```javascript
// URL: /products?products={"category_id":[1,2],"search":"laptop"}
const filters = {
  category_id: [1, 2],
  search: 'laptop'
}
```

### CSV Format (Alternative)
```javascript
// URL: /products?products=category_id:1,2;search:laptop
const filters = {
  category_id: [1, 2],
  search: 'laptop'
}
```

## Examples

### Advanced E-commerce Filter System
```vue
<template>
  <div class="advanced-product-search">
    <h2>Advanced Product Search</h2>
    
    <codex-filter-context 
      :group="'advanced-products'"
      :default-values="productDefaults"
      :fixed-values="productFixed"
      :accept-url-filters="['category_id', 'search', 'brand_id']"
      :paginate="true"
    >
      <template #default="{ filters, combinedFilters, fixedValues }">
        <div class="search-interface">
          <!-- Filter Loading Indicator -->
          <filter-loading-state :group="'advanced-products'" />
          
          <!-- Primary Filter Interface -->
          <codex-filter-wrapper :results-count="productCount" :group="'advanced-products'">
            <template #primaryFilters>
              <div class="primary-filters-grid">
                <codex-contextual-filter
                  :group="'advanced-products'"
                  :definition="'categories.hierarchy'"
                  :filter-key="'category_id'"
                  :label="'Categories'"
                  :filter-type="'checkbox-group'"
                  :primary-filter="true"
                />
                
                <codex-contextual-filter
                  :group="'advanced-products'"
                  :definition="searchDefinition"
                  :filter-key="'search'"
                  :label="'Search Products'"
                  :filter-type="'text'"
                />
                
                <codex-contextual-filter
                  :group="'advanced-products'"
                  :definition="'price_ranges.dynamic'"
                  :filter-key="'price_range'"
                  :label="'Price Range'"
                  :filter-type="'select'"
                />
              </div>
            </template>
            
            <template #secondaryFilters>
              <div class="advanced-filters">
                <div class="filter-section">
                  <h4>Brand & Specifications</h4>
                  
                  <codex-contextual-filter
                    :group="'advanced-products'"
                    :definition="'brands.verified'"
                    :filter-key="'brand_id'"
                    :label="'Brand'"
                    :filter-type="'multi-select'"
                  />
                  
                  <codex-contextual-filter
                    :group="'advanced-products'"
                    :definition="'specifications'"
                    :filter-key="'spec_ids'"
                    :label="'Specifications'"
                    :filter-type="'checkbox-group'"
                  />
                </div>
                
                <div class="filter-section">
                  <h4>Availability & Shipping</h4>
                  
                  <codex-contextual-filter
                    :group="'advanced-products'"
                    :definition="'availability'"
                    :filter-key="'in_stock'"
                    :label="'In Stock'"
                    :filter-type="'checkbox'"
                  />
                  
                  <codex-contextual-filter
                    :group="'advanced-products'"
                    :definition="'shipping_options'"
                    :filter-key="'shipping_type'"
                    :label="'Shipping Options'"
                    :filter-type="'checkbox-group'"
                  />
                </div>
                
                <div class="filter-section">
                  <h4>Customer Reviews</h4>
                  
                  <codex-contextual-filter
                    :group="'advanced-products'"
                    :definition="'ratings.ranges'"
                    :filter-key="'min_rating'"
                    :label="'Minimum Rating'"
                    :filter-type="'radio'"
                  />
                  
                  <codex-contextual-filter
                    :group="'advanced-products'"
                    :definition="reviewCountDefinition"
                    :filter-key="'min_review_count'"
                    :label="'Minimum Reviews'"
                    :filter-type="'number'"
                  />
                </div>
              </div>
            </template>
          </codex-filter-wrapper>
          
          <!-- Filter State Debug Panel -->
          <div v-if="showDebug" class="debug-panel">
            <h4>Filter Debug Information</h4>
            
            <div class="debug-section">
              <h5>User Filters</h5>
              <pre>{{ JSON.stringify(filters, null, 2) }}</pre>
            </div>
            
            <div class="debug-section">
              <h5>Fixed Values</h5>
              <pre>{{ JSON.stringify(fixedValues, null, 2) }}</pre>
            </div>
            
            <div class="debug-section">
              <h5>Combined Filters (API Query)</h5>
              <pre>{{ JSON.stringify(combinedFilters, null, 2) }}</pre>
            </div>
            
            <div class="debug-section">
              <h5>Active Filter Summary</h5>
              <div class="filter-summary">
                <div class="summary-item">
                  <span class="label">Total Active Filters:</span>
                  <span class="value">{{ getActiveFilterCount(filters) }}</span>
                </div>
                <div class="summary-item">
                  <span class="label">Search Query:</span>
                  <span class="value">{{ filters.search || 'None' }}</span>
                </div>
                <div class="summary-item">
                  <span class="label">Selected Categories:</span>
                  <span class="value">{{ filters.category_id?.length || 0 }}</span>
                </div>
                <div class="summary-item">
                  <span class="label">URL Sync:</span>
                  <span class="value">{{ hasUrlSync ? 'Enabled' : 'Disabled' }}</span>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Product Results -->
          <div class="product-results-section">
            <div class="results-header">
              <div class="results-count">
                <h3>{{ productCount }} Products Found</h3>
                <p v-if="hasActiveFilters(filters)">
                  Showing filtered results
                  <button @click="clearAllFilters" class="clear-link">
                    Clear all filters
                  </button>
                </p>
              </div>
              
              <div class="results-actions">
                <button @click="saveFilterPreset" class="save-preset-btn">
                  Save Filter Preset
                </button>
                <button @click="shareFilteredResults" class="share-btn">
                  Share Results
                </button>
                <button @click="exportResults" class="export-btn">
                  Export Results
                </button>
              </div>
            </div>
            
            <enhanced-product-grid 
              :filters="combinedFilters"
              :group="'advanced-products'"
              @products-loaded="handleProductsLoaded"
            />
          </div>
        </div>
      </template>
    </codex-filter-context>
    
    <!-- Debug Toggle -->
    <div class="debug-controls">
      <button @click="toggleDebug" class="debug-toggle">
        {{ showDebug ? 'Hide' : 'Show' }} Debug Panel
      </button>
    </div>
  </div>
</template>

<script setup>
const showDebug = ref(false)
const productCount = ref(0)
const hasUrlSync = ref(true)

const productDefaults = {
  category_id: [],
  search: '',
  price_range: '',
  brand_id: [],
  in_stock: false,
  min_rating: '',
  shipping_type: []
}

const productFixed = {
  status: 'active',
  visibility: 'public',
  store_id: 'current'
}

const searchDefinition = {
  type: 'text',
  source: 'static'
}

const reviewCountDefinition = {
  type: 'number',
  source: 'static'
}

const getActiveFilterCount = (filters) => {
  return Object.entries(filters).filter(([key, value]) => {
    if (Array.isArray(value)) return value.length > 0
    if (typeof value === 'string') return value.trim() !== ''
    if (typeof value === 'boolean') return value === true
    if (typeof value === 'number') return value > 0
    return value !== null && value !== undefined
  }).length
}

const hasActiveFilters = (filters) => {
  return getActiveFilterCount(filters) > 0
}

const clearAllFilters = () => {
  // Implementation would reset filters through context
  console.log('Clearing all filters')
}

const saveFilterPreset = () => {
  // Implementation would save current filter state
  console.log('Saving filter preset')
}

const shareFilteredResults = () => {
  // Implementation would generate shareable URL
  const url = window.location.href
  navigator.clipboard.writeText(url)
  toast.success('Filter URL copied to clipboard!')
}

const exportResults = () => {
  // Implementation would export filtered data
  console.log('Exporting filtered results')
}

const toggleDebug = () => {
  showDebug.value = !showDebug.value
}

const handleProductsLoaded = (count) => {
  productCount.value = count
}
</script>
```

### Multi-Context Dashboard Filters
```vue
<template>
  <div class="analytics-dashboard">
    <h2>Analytics Dashboard</h2>
    
    <div class="dashboard-contexts">
      <!-- Sales Analytics Context -->
      <div class="analytics-section">
        <h3>Sales Analytics</h3>
        
        <codex-filter-context 
          :group="'sales'"
          :default-values="salesDefaults"
          :fixed-values="{ department: 'sales' }"
          :accept-url-filters="['date_range', 'region', 'product_category']"
        >
          <template #default="{ filters: salesFilters, combinedFilters: salesCombined }">
            <div class="sales-filters">
              <codex-contextual-filter
                :group="'sales'"
                :definition="dateRangeDefinition"
                :filter-key="'date_range'"
                :label="'Date Range'"
                :filter-type="'select'"
              />
              
              <codex-contextual-filter
                :group="'sales'"
                :definition="'regions'"
                :filter-key="'region'"
                :label="'Region'"
                :filter-type="'multi-select'"
              />
              
              <codex-contextual-filter
                :group="'sales'"
                :definition="'product_categories'"
                :filter-key="'product_category'"
                :label="'Product Category'"
                :filter-type="'checkbox-group'"
              />
            </div>
            
            <sales-analytics-dashboard :filters="salesCombined" />
          </template>
        </codex-filter-context>
      </div>
      
      <!-- Customer Analytics Context -->
      <div class="analytics-section">
        <h3>Customer Analytics</h3>
        
        <codex-filter-context 
          :group="'customers'"
          :default-values="customerDefaults"
          :fixed-values="{ status: 'active' }"
          :accept-url-filters="['segment', 'acquisition_channel']"
        >
          <template #default="{ filters: customerFilters, combinedFilters: customerCombined }">
            <div class="customer-filters">
              <codex-contextual-filter
                :group="'customers'"
                :definition="'customer_segments'"
                :filter-key="'segment'"
                :label="'Customer Segment'"
                :filter-type="'select'"
              />
              
              <codex-contextual-filter
                :group="'customers'"
                :definition="'acquisition_channels'"
                :filter-key="'acquisition_channel'"
                :label="'Acquisition Channel'"
                :filter-type="'checkbox-group'"
              />
              
              <codex-contextual-filter
                :group="'customers'"
                :definition="lifetimeValueDefinition"
                :filter-key="'min_lifetime_value'"
                :label="'Min Lifetime Value'"
                :filter-type="'number'"
              />
            </div>
            
            <customer-analytics-dashboard :filters="customerCombined" />
          </template>
        </codex-filter-context>
      </div>
      
      <!-- Marketing Analytics Context -->
      <div class="analytics-section">
        <h3>Marketing Analytics</h3>
        
        <codex-filter-context 
          :group="'marketing'"
          :default-values="marketingDefaults"
          :fixed-values="{ status: 'active', type: 'campaign' }"
          :paginate="true"
        >
          <template #default="{ filters: marketingFilters, combinedFilters: marketingCombined }">
            <div class="marketing-filters">
              <codex-contextual-filter
                :group="'marketing'"
                :definition="'campaign_types'"
                :filter-key="'campaign_type'"
                :label="'Campaign Type'"
                :filter-type="'radio'"
              />
              
              <codex-contextual-filter
                :group="'marketing'"
                :definition="'marketing_channels'"
                :filter-key="'channel'"
                :label="'Marketing Channel'"
                :filter-type="'multi-select'"
              />
              
              <codex-contextual-filter
                :group="'marketing'"
                :definition="budgetDefinition"
                :filter-key="'min_budget'"
                :label="'Minimum Budget'"
                :filter-type="'number'"
              />
            </div>
            
            <marketing-analytics-dashboard :filters="marketingCombined" />
          </template>
        </codex-filter-context>
      </div>
    </div>
    
    <!-- Cross-Context Analytics -->
    <div class="cross-context-analytics">
      <h3>Cross-Department Insights</h3>
      <p>Analytics combining data from all departments with their respective filters.</p>
      
      <!-- This would use a custom component that listens to multiple contexts -->
      <cross-department-insights />
    </div>
  </div>
</template>

<script setup>
// Sales filter defaults
const salesDefaults = {
  date_range: 'this_month',
  region: [],
  product_category: []
}

// Customer filter defaults
const customerDefaults = {
  segment: '',
  acquisition_channel: [],
  min_lifetime_value: ''
}

// Marketing filter defaults
const marketingDefaults = {
  campaign_type: '',
  channel: [],
  min_budget: ''
}

// Custom filter definitions
const dateRangeDefinition = {
  type: 'select',
  source: {
    'today': 'Today',
    'yesterday': 'Yesterday',
    'this_week': 'This Week',
    'last_week': 'Last Week',
    'this_month': 'This Month',
    'last_month': 'Last Month',
    'this_quarter': 'This Quarter',
    'last_quarter': 'Last Quarter',
    'this_year': 'This Year',
    'last_year': 'Last Year',
    'custom': 'Custom Range'
  }
}

const lifetimeValueDefinition = {
  type: 'number',
  source: 'static'
}

const budgetDefinition = {
  type: 'number',
  source: 'static'
}
</script>
```

## Best Practices

### Context Organization
- Use descriptive group names that clearly identify the context purpose
- Keep related filters within the same context group
- Avoid deeply nested filter contexts when possible
- Plan filter context architecture before implementation
- Consider context isolation for independent filter systems

### State Management
- Initialize filters with sensible default values
- Use fixed values for filters that should never change
- Validate filter combinations both client and server side
- Handle filter state persistence appropriately
- Implement proper error handling for invalid filter states

### URL Synchronization
- Only sync essential filters to avoid long URLs
- Use meaningful parameter names in URL synchronization
- Consider URL length limitations for complex filter states
- Implement proper URL encoding/decoding for special characters
- Test URL sharing across different environments

### Performance
- Use reactive filter updates efficiently to minimize re-renders
- Implement proper filter caching strategies
- Consider debouncing for frequently changing filters
- Optimize combined filter computation for large datasets
- Monitor filter context performance in production

### Integration
- Design filter contexts to work well with pagination systems
- Ensure filter contexts integrate properly with API endpoints
- Handle loading states for asynchronous filter operations
- Implement proper error boundaries for filter failures
- Test filter contexts with real-world data volumes

### Development
- Use the slot props pattern for clean component architecture
- Implement proper TypeScript types for filter values
- Create reusable filter definition configurations
- Document filter context APIs for team development
- Use consistent naming conventions across filter contexts

## Component Registration
```javascript
// Global registration
app.component('CodexFilterContext', FilterContext)

// Local registration  
import FilterContext from '@/components/filters/FilterContext.vue'

export default {
  components: {
    CodexFilterContext: FilterContext
  }
}
``` 