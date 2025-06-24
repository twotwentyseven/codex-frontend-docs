# FilterWrapper Component

## Overview
The FilterWrapper component provides a layout container for organizing primary and secondary filters with a modal popout system. It manages the display of primary filters that are always visible and secondary filters that appear in a modal overlay. The component includes result count display, filter button interface, and responsive layout management for optimal filtering experiences across different screen sizes.

## Basic Usage
```vue
<template>
  <div class="product-listing">
    <codex-filter-wrapper :results-count="productCount" :group="'products'">
      <template #primaryFilters>
        <codex-contextual-filter
          :group="'products'"
          :definition="'categories'"
          :filter-key="'category_id'"
          :label="'Category'"
          :filter-type="'checkbox-group'"
          :primary-filter="true"
        />
      </template>
      
      <template #secondaryFilters>
        <codex-contextual-filter
          :group="'products'"
          :definition="'brands'"
          :filter-key="'brand_id'"
          :label="'Brand'"
          :filter-type="'select'"
        />
      </template>
    </codex-filter-wrapper>
  </div>
</template>

<script setup>
const productCount = ref(245)
</script>
```

## Key Features
- Primary and secondary filter organization
- Modal popout system for secondary filters
- Responsive layout with mobile-first design
- Results count display in filter modal
- Automatic filter button visibility based on secondary filters
- Internationalization support for filter labels
- Integration with filter context system
- Right-aligned modal positioning
- Customizable filter button styling

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `resultsCount` | `Number` | `required` | Number of results found with current filters |

### Filter Configuration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `group` | `String` | `'default'` | Filter group identifier for context |

## Slots

### Primary Filters Slot
| Slot | Description |
|------|-------------|
| `primaryFilters` | Always visible filters displayed on the main page |

### Secondary Filters Slot
| Slot | Description |
|------|-------------|
| `secondaryFilters` | Filters displayed in the modal popout |

## Modal Configuration

The component automatically creates a modal with the following configuration:
- **Modal Name**: `filter-popout-{group}`
- **X Position**: `right`
- **Y Position**: `bottom`
- **Trigger**: Filter button click

## Internationalization

The component uses the following translation keys:

| Key | Default | Usage |
|-----|---------|-------|
| `filter.popout_button` | `"More Filters"` | Filter modal button text |
| `filter.results_count` | `"{count} results"` | Results count in modal footer |

## Examples

### E-commerce Product Filters
```vue
<template>
  <div class="product-catalog">
    <h2>Product Catalog</h2>
    
    <codex-filter-context :group="'products'" :default-values="defaultFilters">
      <template #default="{ filters, combinedFilters }">
        <codex-filter-wrapper :results-count="filteredProductCount" :group="'products'">
          <!-- Primary Filters - Always Visible -->
          <template #primaryFilters>
            <div class="primary-filter-grid">
              <codex-contextual-filter
                :group="'products'"
                :definition="'categories.popular'"
                :filter-key="'category_id'"
                :label="'Category'"
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
              
              <codex-contextual-filter
                :group="'products'"
                :definition="'availability'"
                :filter-key="'in_stock'"
                :label="'In Stock Only'"
                :filter-type="'checkbox'"
                :primary-filter="true"
              />
            </div>
            
            <!-- Search Filter -->
            <div class="search-filter">
              <codex-contextual-filter
                :group="'products'"
                :definition="searchDefinition"
                :filter-key="'search'"
                :label="'Search Products'"
                :filter-type="'text'"
              />
            </div>
          </template>
          
          <!-- Secondary Filters - Modal Popout -->
          <template #secondaryFilters>
            <div class="secondary-filter-sections">
              <div class="filter-section">
                <h4>Brand & Manufacturer</h4>
                
                <codex-contextual-filter
                  :group="'products'"
                  :definition="'brands.all'"
                  :filter-key="'brand_id'"
                  :label="'Brand'"
                  :filter-type="'select'"
                />
                
                <codex-contextual-filter
                  :group="'products'"
                  :definition="'manufacturers'"
                  :filter-key="'manufacturer_id'"
                  :label="'Manufacturer'"
                  :filter-type="'select'"
                />
              </div>
              
              <div class="filter-section">
                <h4>Product Attributes</h4>
                
                <codex-contextual-filter
                  :group="'products'"
                  :definition="'materials'"
                  :filter-key="'material_ids'"
                  :label="'Materials'"
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
                  :definition="'sizes'"
                  :filter-key="'size_ids'"
                  :label="'Sizes'"
                  :filter-type="'checkbox-group'"
                />
              </div>
              
              <div class="filter-section">
                <h4>Ratings & Reviews</h4>
                
                <codex-contextual-filter
                  :group="'products'"
                  :definition="'ratings'"
                  :filter-key="'min_rating'"
                  :label="'Minimum Rating'"
                  :filter-type="'select'"
                />
                
                <codex-contextual-filter
                  :group="'products'"
                  :definition="reviewCountDefinition"
                  :filter-key="'min_reviews'"
                  :label="'Minimum Reviews'"
                  :filter-type="'number'"
                />
              </div>
              
              <div class="filter-section">
                <h4>Shipping & Delivery</h4>
                
                <codex-contextual-filter
                  :group="'products'"
                  :definition="'shipping_options'"
                  :filter-key="'free_shipping'"
                  :label="'Free Shipping'"
                  :filter-type="'checkbox'"
                />
                
                <codex-contextual-filter
                  :group="'products'"
                  :definition="'delivery_speed'"
                  :filter-key="'delivery_speed'"
                  :label="'Delivery Speed'"
                  :filter-type="'radio'"
                />
              </div>
              
              <div class="filter-section">
                <h4>Special Offers</h4>
                
                <codex-contextual-filter
                  :group="'products'"
                  :definition="'promotions'"
                  :filter-key="'on_sale'"
                  :label="'On Sale'"
                  :filter-type="'checkbox'"
                />
                
                <codex-contextual-filter
                  :group="'products'"
                  :definition="'discounts'"
                  :filter-key="'discount_percentage'"
                  :label="'Discount Percentage'"
                  :filter-type="'select'"
                />
              </div>
            </div>
          </template>
        </codex-filter-wrapper>
        
        <!-- Product Grid -->
        <div class="product-grid">
          <product-card
            v-for="product in filteredProducts"
            :key="product.id"
            :product="product"
          />
          
          <div v-if="filteredProducts.length === 0" class="no-results">
            <h3>No products found</h3>
            <p>Try adjusting your filters to see more results.</p>
            <button @click="clearAllFilters" class="clear-filters-btn">
              Clear All Filters
            </button>
          </div>
        </div>
        
        <!-- Load More / Pagination -->
        <div class="pagination-section">
          <button v-if="hasMoreProducts" @click="loadMoreProducts" class="load-more-btn">
            Load More Products
          </button>
        </div>
      </template>
    </codex-filter-context>
  </div>
</template>

<script setup>
const defaultFilters = {
  category_id: [],
  price_range: '',
  in_stock: false,
  search: ''
}

const searchDefinition = {
  type: 'text',
  source: 'static'
}

const reviewCountDefinition = {
  type: 'number',
  source: 'static'
}

const filteredProductCount = ref(245)
const filteredProducts = ref([])
const hasMoreProducts = ref(true)

const clearAllFilters = () => {
  // Implementation would reset all filters
  console.log('Clearing all filters')
}

const loadMoreProducts = () => {
  // Implementation would load additional products
  console.log('Loading more products')
}
</script>
```

### Content Management System Filters
```vue
<template>
  <div class="content-management">
    <h2>Content Library</h2>
    
    <codex-filter-context :group="'content'" :fixed-values="{ status: 'published' }">
      <template #default="{ filters, combinedFilters }">
        <codex-filter-wrapper :results-count="contentCount" :group="'content'">
          <!-- Primary Content Filters -->
          <template #primaryFilters>
            <div class="content-primary-filters">
              <codex-contextual-filter
                :group="'content'"
                :definition="'content_types'"
                :filter-key="'content_type'"
                :label="'Content Type'"
                :filter-type="'radio'"
                :primary-filter="true"
              />
              
              <codex-contextual-filter
                :group="'content'"
                :definition="'publication_status'"
                :filter-key="'publication_status'"
                :label="'Status'"
                :filter-type="'select'"
                :primary-filter="true"
              />
              
              <codex-contextual-filter
                :group="'content'"
                :definition="searchDefinition"
                :filter-key="'search'"
                :label="'Search Content'"
                :filter-type="'text'"
              />
            </div>
          </template>
          
          <!-- Advanced Content Filters -->
          <template #secondaryFilters>
            <div class="content-secondary-filters">
              <div class="filter-section">
                <h4>Content Organization</h4>
                
                <codex-contextual-filter
                  :group="'content'"
                  :definition="'authors.all'"
                  :filter-key="'author_id'"
                  :label="'Author'"
                  :filter-type="'multi-select'"
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
              
              <div class="filter-section">
                <h4>Publishing & Dates</h4>
                
                <codex-contextual-filter
                  :group="'content'"
                  :definition="dateRangeDefinition"
                  :filter-key="'date_range'"
                  :label="'Publication Date'"
                  :filter-type="'select'"
                />
                
                <codex-contextual-filter
                  :group="'content'"
                  :definition="'last_modified'"
                  :filter-key="'modified_range'"
                  :label="'Last Modified'"
                  :filter-type="'select'"
                />
              </div>
              
              <div class="filter-section">
                <h4>Performance Metrics</h4>
                
                <codex-contextual-filter
                  :group="'content'"
                  :definition="viewsDefinition"
                  :filter-key="'min_views'"
                  :label="'Minimum Views'"
                  :filter-type="'number'"
                />
                
                <codex-contextual-filter
                  :group="'content'"
                  :definition="engagementDefinition"
                  :filter-key="'min_engagement'"
                  :label="'Minimum Engagement Rate'"
                  :filter-type="'number'"
                />
                
                <codex-contextual-filter
                  :group="'content'"
                  :definition="'trending'"
                  :filter-key="'is_trending'"
                  :label="'Trending Content'"
                  :filter-type="'checkbox'"
                />
              </div>
              
              <div class="filter-section">
                <h4>Content Properties</h4>
                
                <codex-contextual-filter
                  :group="'content'"
                  :definition="'reading_time'"
                  :filter-key="'reading_time_range'"
                  :label="'Reading Time'"
                  :filter-type="'select'"
                />
                
                <codex-contextual-filter
                  :group="'content'"
                  :definition="'word_count'"
                  :filter-key="'word_count_range'"
                  :label="'Word Count'"
                  :filter-type="'select'"
                />
                
                <codex-contextual-filter
                  :group="'content'"
                  :definition="'has_media'"
                  :filter-key="'has_images'"
                  :label="'Has Images'"
                  :filter-type="'checkbox'"
                />
                
                <codex-contextual-filter
                  :group="'content'"
                  :definition="'has_media'"
                  :filter-key="'has_video'"
                  :label="'Has Video'"
                  :filter-type="'checkbox'"
                />
              </div>
            </div>
          </template>
        </codex-filter-wrapper>
        
        <!-- Content Results -->
        <div class="content-results">
          <div class="results-header">
            <div class="view-options">
              <button @click="setViewMode('grid')" :class="{ active: viewMode === 'grid' }">
                <i class="ri-grid-line"></i> Grid
              </button>
              <button @click="setViewMode('list')" :class="{ active: viewMode === 'list' }">
                <i class="ri-list-check"></i> List
              </button>
              <button @click="setViewMode('table')" :class="{ active: viewMode === 'table' }">
                <i class="ri-table-line"></i> Table
              </button>
            </div>
            
            <div class="sort-options">
              <select v-model="sortBy" @change="applySorting">
                <option value="published_date">Publication Date</option>
                <option value="title">Title</option>
                <option value="author">Author</option>
                <option value="views">Views</option>
                <option value="engagement">Engagement</option>
              </select>
              
              <button @click="toggleSortOrder" class="sort-direction">
                <i :class="sortOrder === 'asc' ? 'ri-arrow-up-line' : 'ri-arrow-down-line'"></i>
              </button>
            </div>
          </div>
          
          <content-list 
            :content="filteredContent" 
            :view-mode="viewMode"
            :filters="combinedFilters"
          />
          
          <div v-if="filteredContent.length === 0" class="no-content">
            <h3>No content found</h3>
            <p>Try adjusting your filters to find more content.</p>
            <button @click="resetFilters" class="reset-filters-btn">
              Reset Filters
            </button>
          </div>
        </div>
      </template>
    </codex-filter-context>
  </div>
</template>

<script setup>
const contentCount = ref(1247)
const filteredContent = ref([])
const viewMode = ref('grid')
const sortBy = ref('published_date')
const sortOrder = ref('desc')

const searchDefinition = {
  type: 'text',
  source: 'static'
}

const dateRangeDefinition = {
  type: 'select',
  source: {
    'today': 'Today',
    'this_week': 'This Week',
    'this_month': 'This Month',
    'this_quarter': 'This Quarter',
    'this_year': 'This Year',
    'custom': 'Custom Range'
  }
}

const viewsDefinition = {
  type: 'number',
  source: 'static'
}

const engagementDefinition = {
  type: 'number',
  source: 'static'
}

const setViewMode = (mode) => {
  viewMode.value = mode
}

const applySorting = () => {
  // Implementation would apply sorting
  console.log(`Sorting by ${sortBy.value} in ${sortOrder.value} order`)
}

const toggleSortOrder = () => {
  sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc'
  applySorting()
}

const resetFilters = () => {
  // Implementation would reset all filters
  console.log('Resetting filters')
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-filters` | Main container class |
| `_c-flex` | Flexbox layout |
| `_c-column` | Flex direction column |
| `_c-justify-between` | Space between justification |
| `_c-items-center` | Center align items |
| `_c-row_lg` | Row direction on large screens |
| `_c-btn-container` | Button container styling |
| `_c-w-full` | Full width |
| `_c-w-auto_lg` | Auto width on large screens |
| `_c-order-0` | Order 0 for layout |
| `_c-order-1_lg` | Order 1 on large screens |
| `_c-filter-btn` | Filter button styling |
| `_c-text-icon` | Icon text styling |
| `_c-filter-card` | Filter modal card styling |
| `_c-card` | Card container styling |
| `_c-content` | Content area styling |
| `_c-gap-xl` | Extra large gap spacing |
| `_c-footer` | Footer area styling |
| `_c-desc` | Description text styling |

## Best Practices

### Layout Organization
- Use primary filters for the most commonly used filtering options
- Reserve secondary filters for advanced or less frequently used options
- Organize secondary filters in logical groups with clear headings
- Ensure primary filters work well on mobile devices
- Consider the visual hierarchy of filter importance

### User Experience
- Provide clear labels for filter groups and individual filters
- Show filter result counts to help users understand the impact
- Include a "Clear All Filters" option for easy reset
- Make the filter modal easy to open and close
- Test filter interactions on various screen sizes

### Performance
- Lazy load secondary filter options until the modal is opened
- Debounce filter changes to prevent excessive API calls
- Cache filter configurations for faster subsequent loads
- Consider pagination for large result sets
- Optimize modal rendering for smooth interactions

### Mobile Responsiveness
- Ensure primary filters work well on small screens
- Make filter buttons touch-friendly with adequate spacing
- Consider collapsing primary filters on very small screens
- Test modal interactions on mobile devices
- Provide clear visual feedback for filter states

### Accessibility
- Ensure proper keyboard navigation through all filter elements
- Use appropriate ARIA labels for filter sections and buttons
- Provide screen reader announcements for filter changes
- Test with assistive technologies
- Maintain logical tab order through filter interfaces

### State Management
- Persist filter states across page navigation when appropriate
- Synchronize filter state with URL parameters for shareable links
- Handle filter loading states gracefully
- Validate filter combinations on both client and server
- Provide clear feedback when filters are being applied

## Component Registration
```javascript
// Global registration
app.component('CodexFilterWrapper', FilterWrapper)

// Local registration  
import FilterWrapper from '@/components/filters/FilterWrapper.vue'

export default {
  components: {
    CodexFilterWrapper: FilterWrapper
  }
}
``` 