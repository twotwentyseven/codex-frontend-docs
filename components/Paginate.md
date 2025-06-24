# Paginate Component

## Overview
The Paginate component provides a comprehensive pagination interface with flexible configuration options, loading states, and context-aware data integration. It supports both list-based and inline layouts, break views for large page sets, and integrates with the application's global state management system through dependency injection. The component handles keyboard navigation, accessibility features, and provides extensive customization for navigation controls.

## Basic Usage
```vue
<template>
  <div class="pagination-example">
    <codex-paginate 
      :force-page="currentPage"
      :page-range="5"
      :margin-pages="2"
      :loading="isLoading"
      @update:modelValue="handlePageChange"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'

const currentPage = ref(1)
const isLoading = ref(false)

const handlePageChange = (page) => {
  currentPage.value = page
  console.log('Page changed to:', page)
}
</script>
```

## Key Features
- Intelligent page range calculation with break views
- Loading state with skeleton animation
- Dependency injection for pagination context
- Keyboard navigation support (Enter key activation)
- Configurable navigation buttons (first/last, prev/next)
- Flexible layout options (list or inline)
- Integration with filters and state management
- Accessibility compliance with proper tabindex management
- Customizable button text and icons
- Break view support for large page sets

## Configuration Props

### Navigation Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `pageRange` | `Number` | `3` | Number of pages to show around current page |
| `marginPages` | `Number` | `1` | Number of pages to show at start/end margins |
| `firstLastButton` | `Boolean` | `false` | Show first and last page buttons |
| `hidePrevNext` | `Boolean` | `false` | Hide previous and next buttons when at boundaries |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `prevText` | `String|Boolean` | `'<i class="_c-text-icon ri-arrow-left-s-line"></i>'` | Previous button content |
| `nextText` | `String|Boolean` | `'<i class="_c-text-icon ri-arrow-right-s-line"></i>'` | Next button content |
| `firstButtonText` | `String` | `'<i class="_c-text-icon ri-arrow-left-s-line"></i>'` | First page button content |
| `lastButtonText` | `String` | `'<i class="_c-text-icon ri-arrow-right-s-line"></i>'` | Last page button content |
| `breakViewText` | `String` | `'…'` | Text to show in break views |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `noLiSurround` | `Boolean` | `false` | Use div layout instead of ul/li |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `forcePage` | `Number` | `undefined` | Force specific page selection |
| `loading` | `Boolean` | `false` | Show loading skeleton |
| `group` | `String` | `'default'` | Pagination group identifier for injection |
| `paginationKey` | `String` | `'current_page'` | Key name for pagination state |

### Event Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `clickHandler` | `Function` | `() => {}` | Custom click handler function |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `update:modelValue` | `pageNumber` | Emitted when page selection changes |

## Slots

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `breakViewContent` | N/A | Custom content for break views (ellipsis) |

## Dependency Injection

The component uses Vue's injection system to integrate with pagination context:

```javascript
// Injected dependencies
const pagination = inject(`pagination:${props.group}`)
const filters = inject(`filters:${props.group}`)
const updateState = inject(`updateState:${props.group}`)
```

### Pagination Context Structure
```javascript
{
  current_page: 1,        // Current active page
  last_page: 10,          // Total number of pages
  per_page: 20,           // Items per page
  total: 200              // Total number of items
}
```

## Page Calculation Algorithm

The component uses intelligent algorithms to determine which pages to display:

1. **Simple Range**: When total pages ≤ pageRange, show all pages
2. **Complex Range**: When total pages > pageRange:
   - Show margin pages at start and end
   - Show current page ± (pageRange/2) pages
   - Add break views where gaps exist
   - Ensure selected range doesn't exceed boundaries

## Examples

### Basic Pagination with Custom Styling
```vue
<template>
  <div class="data-table-example">
    <div class="table-container">
      <table class="data-table">
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Email</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in paginatedData" :key="item.id">
            <td>{{ item.id }}</td>
            <td>{{ item.name }}</td>
            <td>{{ item.email }}</td>
            <td>
              <span :class="['status-badge', item.status]">
                {{ item.status }}
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    
    <div class="pagination-container">
      <div class="pagination-info">
        <span>
          Showing {{ startItem }} - {{ endItem }} of {{ totalItems }} results
        </span>
      </div>
      
      <codex-paginate 
        v-model="currentPage"
        :page-range="5"
        :margin-pages="1"
        :first-last-button="true"
        :loading="isLoading"
        :click-handler="handlePageClick"
        first-button-text="First"
        last-button-text="Last"
        prev-text="← Previous"
        next-text="Next →"
        group="data-table"
      />
      
      <div class="pagination-controls">
        <select v-model="itemsPerPage" @change="updatePagination">
          <option value="10">10 per page</option>
          <option value="25">25 per page</option>
          <option value="50">50 per page</option>
          <option value="100">100 per page</option>
        </select>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, provide, onMounted } from 'vue'

const currentPage = ref(1)
const itemsPerPage = ref(25)
const isLoading = ref(false)
const allData = ref([])

// Mock data generation
onMounted(() => {
  generateMockData()
})

const generateMockData = () => {
  const statuses = ['active', 'inactive', 'pending']
  allData.value = Array.from({ length: 247 }, (_, i) => ({
    id: i + 1,
    name: `User ${i + 1}`,
    email: `user${i + 1}@example.com`,
    status: statuses[Math.floor(Math.random() * statuses.length)]
  }))
}

const totalItems = computed(() => allData.value.length)
const totalPages = computed(() => Math.ceil(totalItems.value / itemsPerPage.value))

const paginatedData = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage.value
  const end = start + itemsPerPage.value
  return allData.value.slice(start, end)
})

const startItem = computed(() => {
  return totalItems.value === 0 ? 0 : (currentPage.value - 1) * itemsPerPage.value + 1
})

const endItem = computed(() => {
  const end = currentPage.value * itemsPerPage.value
  return end > totalItems.value ? totalItems.value : end
})

// Provide pagination context for the component
provide('pagination:data-table', computed(() => ({
  current_page: currentPage.value,
  last_page: totalPages.value,
  per_page: itemsPerPage.value,
  total: totalItems.value
})))

const handlePageClick = (page) => {
  isLoading.value = true
  
  // Simulate API call delay
  setTimeout(() => {
    console.log(`Loading page ${page}`)
    isLoading.value = false
  }, 500)
}

const updatePagination = () => {
  currentPage.value = 1
}
</script>

<style scoped>
.data-table-example {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.table-container {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  overflow: hidden;
  margin-bottom: 2rem;
}

.data-table {
  width: 100%;
  border-collapse: collapse;
}

.data-table th,
.data-table td {
  padding: 1rem;
  text-align: left;
  border-bottom: 1px solid #dee2e6;
}

.data-table th {
  background: #f8f9fa;
  font-weight: 600;
  color: #495057;
}

.status-badge {
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.875rem;
  font-weight: 500;
}

.status-badge.active {
  background: #d4edda;
  color: #155724;
}

.status-badge.inactive {
  background: #f8d7da;
  color: #721c24;
}

.status-badge.pending {
  background: #fff3cd;
  color: #856404;
}

.pagination-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: white;
  padding: 1rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.pagination-info {
  color: #6c757d;
  font-size: 0.875rem;
}

.pagination-controls select {
  padding: 0.5rem;
  border: 1px solid #dee2e6;
  border-radius: 4px;
  background: white;
}

@media (max-width: 768px) {
  .pagination-container {
    flex-direction: column;
    gap: 1rem;
  }
}
</style>
```

### Advanced Pagination with Filtering
```vue
<template>
  <div class="advanced-pagination">
    <div class="filters-section">
      <h3>Product Catalog</h3>
      
      <div class="filter-controls">
        <div class="filter-group">
          <label>Category:</label>
          <select v-model="filters.category" @change="applyFilters">
            <option value="">All Categories</option>
            <option value="electronics">Electronics</option>
            <option value="clothing">Clothing</option>
            <option value="books">Books</option>
            <option value="home">Home & Garden</option>
          </select>
        </div>
        
        <div class="filter-group">
          <label>Price Range:</label>
          <select v-model="filters.priceRange" @change="applyFilters">
            <option value="">Any Price</option>
            <option value="0-25">$0 - $25</option>
            <option value="25-50">$25 - $50</option>
            <option value="50-100">$50 - $100</option>
            <option value="100+">$100+</option>
          </select>
        </div>
        
        <div class="filter-group">
          <label>Search:</label>
          <input 
            v-model="filters.search"
            @input="debouncedApplyFilters"
            type="text"
            placeholder="Search products..."
          >
        </div>
        
        <button @click="clearFilters" class="clear-filters">
          Clear Filters
        </button>
      </div>
    </div>
    
    <div class="results-section">
      <div class="results-header">
        <h4>
          {{ filteredData.length }} products found
          <span v-if="hasActiveFilters" class="filter-indicator">
            (filtered)
          </span>
        </h4>
        
        <div class="view-options">
          <label>Items per page:</label>
          <select v-model="pagination.per_page" @change="resetPagination">
            <option :value="12">12</option>
            <option :value="24">24</option>
            <option :value="48">48</option>
          </select>
        </div>
      </div>
      
      <div class="product-grid" v-if="!isLoading">
        <div 
          v-for="product in currentPageData"
          :key="product.id"
          class="product-card"
        >
          <div class="product-image">
            <img :src="product.image" :alt="product.name">
          </div>
          <div class="product-info">
            <h5>{{ product.name }}</h5>
            <p class="product-category">{{ product.category }}</p>
            <p class="product-price">${{ product.price }}</p>
          </div>
        </div>
      </div>
      
      <div v-else class="loading-grid">
        <div v-for="i in pagination.per_page" :key="i" class="product-skeleton">
          <div class="skeleton-image"></div>
          <div class="skeleton-text"></div>
          <div class="skeleton-text short"></div>
        </div>
      </div>
      
      <div class="pagination-section">
        <codex-paginate 
          v-model="pagination.current_page"
          :page-range="7"
          :margin-pages="2"
          :first-last-button="true"
          :loading="isLoading"
          :click-handler="handlePageChange"
          group="product-catalog"
          break-view-text="..."
        >
          <template #breakViewContent>
            <span class="custom-break">⋯</span>
          </template>
        </codex-paginate>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed, provide, onMounted, watch } from 'vue'
import { debounce } from 'lodash-es'

const isLoading = ref(false)
const allProducts = ref([])

const filters = reactive({
  category: '',
  priceRange: '',
  search: ''
})

const pagination = reactive({
  current_page: 1,
  per_page: 24,
  total: 0,
  last_page: 1
})

// Mock product data
onMounted(() => {
  generateProducts()
})

const generateProducts = () => {
  const categories = ['electronics', 'clothing', 'books', 'home']
  const names = {
    electronics: ['Smartphone', 'Laptop', 'Tablet', 'Headphones', 'Camera'],
    clothing: ['T-Shirt', 'Jeans', 'Sneakers', 'Jacket', 'Dress'],
    books: ['Novel', 'Cookbook', 'Biography', 'Guide', 'Textbook'],
    home: ['Lamp', 'Cushion', 'Plant', 'Candle', 'Vase']
  }
  
  allProducts.value = Array.from({ length: 156 }, (_, i) => {
    const category = categories[Math.floor(Math.random() * categories.length)]
    const baseName = names[category][Math.floor(Math.random() * names[category].length)]
    
    return {
      id: i + 1,
      name: `${baseName} ${i + 1}`,
      category,
      price: Math.floor(Math.random() * 200) + 10,
      image: `https://picsum.photos/200/200?random=${i + 1}`
    }
  })
}

const filteredData = computed(() => {
  let filtered = [...allProducts.value]
  
  if (filters.category) {
    filtered = filtered.filter(p => p.category === filters.category)
  }
  
  if (filters.priceRange) {
    const [min, max] = filters.priceRange.includes('+') 
      ? [parseInt(filters.priceRange), Infinity]
      : filters.priceRange.split('-').map(Number)
    
    filtered = filtered.filter(p => p.price >= min && (max === Infinity || p.price <= max))
  }
  
  if (filters.search) {
    const search = filters.search.toLowerCase()
    filtered = filtered.filter(p => 
      p.name.toLowerCase().includes(search) ||
      p.category.toLowerCase().includes(search)
    )
  }
  
  return filtered
})

const currentPageData = computed(() => {
  const start = (pagination.current_page - 1) * pagination.per_page
  const end = start + pagination.per_page
  return filteredData.value.slice(start, end)
})

const hasActiveFilters = computed(() => {
  return filters.category || filters.priceRange || filters.search
})

// Update pagination when filtered data changes
watch(filteredData, (newData) => {
  pagination.total = newData.length
  pagination.last_page = Math.ceil(newData.length / pagination.per_page)
  
  // Reset to page 1 if current page is beyond available pages
  if (pagination.current_page > pagination.last_page) {
    pagination.current_page = 1
  }
}, { immediate: true })

// Provide pagination context
provide('pagination:product-catalog', computed(() => pagination))
provide('filters:product-catalog', filters)

const applyFilters = () => {
  isLoading.value = true
  pagination.current_page = 1
  
  setTimeout(() => {
    isLoading.value = false
  }, 300)
}

const debouncedApplyFilters = debounce(applyFilters, 500)

const clearFilters = () => {
  Object.assign(filters, {
    category: '',
    priceRange: '',
    search: ''
  })
  applyFilters()
}

const resetPagination = () => {
  pagination.current_page = 1
  pagination.last_page = Math.ceil(filteredData.value.length / pagination.per_page)
}

const handlePageChange = (page) => {
  isLoading.value = true
  
  // Simulate API call
  setTimeout(() => {
    console.log(`Loading page ${page} with filters:`, filters)
    isLoading.value = false
  }, 400)
}
</script>

<style scoped>
.advanced-pagination {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.filters-section {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 2rem;
}

.filter-controls {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-top: 1rem;
}

.filter-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.filter-group label {
  font-weight: 600;
  color: #495057;
}

.filter-group select,
.filter-group input {
  padding: 0.75rem;
  border: 1px solid #dee2e6;
  border-radius: 4px;
}

.clear-filters {
  padding: 0.75rem 1.5rem;
  background: #dc3545;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  align-self: end;
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
  border-bottom: 1px solid #dee2e6;
}

.filter-indicator {
  color: #007bff;
  font-size: 0.875rem;
  font-weight: normal;
}

.view-options {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1.5rem;
  padding: 2rem;
}

.product-card {
  border: 1px solid #dee2e6;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.2s, box-shadow 0.2s;
}

.product-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.product-image img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.product-info {
  padding: 1rem;
}

.product-info h5 {
  margin: 0 0 0.5rem 0;
  color: #212529;
}

.product-category {
  color: #6c757d;
  font-size: 0.875rem;
  margin: 0 0 0.5rem 0;
  text-transform: capitalize;
}

.product-price {
  color: #28a745;
  font-weight: 600;
  font-size: 1.125rem;
  margin: 0;
}

.loading-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1.5rem;
  padding: 2rem;
}

.product-skeleton {
  border: 1px solid #dee2e6;
  border-radius: 8px;
  overflow: hidden;
}

.skeleton-image {
  height: 200px;
  background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
  animation: loading 1.5s infinite;
}

.skeleton-text {
  height: 1rem;
  background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
  animation: loading 1.5s infinite;
  margin: 1rem;
  border-radius: 4px;
}

.skeleton-text.short {
  width: 60%;
}

.pagination-section {
  padding: 2rem;
  border-top: 1px solid #dee2e6;
  display: flex;
  justify-content: center;
}

.custom-break {
  color: #6c757d;
  font-weight: bold;
}

@keyframes loading {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

@media (max-width: 768px) {
  .filter-controls {
    grid-template-columns: 1fr;
  }
  
  .results-header {
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
  }
  
  .product-grid,
  .loading-grid {
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 1rem;
    padding: 1rem;
  }
}
</style>
```

### Inline Pagination with Custom Controls
```vue
<template>
  <div class="inline-pagination-demo">
    <div class="content-header">
      <h3>Article List</h3>
      <div class="header-controls">
        <button @click="refreshData" :disabled="isLoading" class="refresh-btn">
          <i class="ri-refresh-line" :class="{ spinning: isLoading }"></i>
          Refresh
        </button>
      </div>
    </div>
    
    <div class="article-list">
      <article 
        v-for="article in currentArticles"
        :key="article.id"
        class="article-item"
      >
        <div class="article-meta">
          <span class="article-date">{{ formatDate(article.date) }}</span>
          <span class="article-category">{{ article.category }}</span>
        </div>
        <h4 class="article-title">{{ article.title }}</h4>
        <p class="article-excerpt">{{ article.excerpt }}</p>
        <div class="article-actions">
          <button @click="readArticle(article)" class="read-btn">
            Read More
          </button>
          <button @click="bookmarkArticle(article)" class="bookmark-btn">
            <i :class="article.bookmarked ? 'ri-bookmark-fill' : 'ri-bookmark-line'"></i>
          </button>
        </div>
      </article>
    </div>
    
    <div class="inline-pagination-wrapper">
      <codex-paginate 
        v-model="currentPage"
        :page-range="3"
        :margin-pages="1"
        :no-li-surround="true"
        :hide-prev-next="false"
        :loading="isLoading"
        :click-handler="handleArticlePageChange"
        prev-text="‹ Prev"
        next-text="Next ›"
        group="articles"
      />
    </div>
    
    <div class="pagination-summary">
      <p>
        Page {{ currentPage }} of {{ totalPages }} 
        ({{ totalArticles }} articles total)
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, provide, onMounted } from 'vue'

const currentPage = ref(1)
const articlesPerPage = ref(5)
const isLoading = ref(false)
const allArticles = ref([])

onMounted(() => {
  generateArticles()
})

const generateArticles = () => {
  const categories = ['Technology', 'Design', 'Business', 'Science', 'Health']
  const titles = [
    'The Future of Web Development',
    'Understanding Modern Design Patterns',
    'Building Scalable Applications',
    'Data Science Fundamentals',
    'Healthy Living Tips'
  ]
  
  allArticles.value = Array.from({ length: 73 }, (_, i) => ({
    id: i + 1,
    title: `${titles[i % titles.length]} ${Math.floor(i / titles.length) + 1}`,
    excerpt: `This is an informative article about ${categories[i % categories.length].toLowerCase()} topics that provides valuable insights and practical advice for readers interested in learning more.`,
    category: categories[i % categories.length],
    date: new Date(Date.now() - Math.random() * 365 * 24 * 60 * 60 * 1000),
    bookmarked: Math.random() > 0.7
  }))
}

const totalArticles = computed(() => allArticles.value.length)
const totalPages = computed(() => Math.ceil(totalArticles.value / articlesPerPage.value))

const currentArticles = computed(() => {
  const start = (currentPage.value - 1) * articlesPerPage.value
  const end = start + articlesPerPage.value
  return allArticles.value.slice(start, end)
})

// Provide pagination context
provide('pagination:articles', computed(() => ({
  current_page: currentPage.value,
  last_page: totalPages.value,
  per_page: articlesPerPage.value,
  total: totalArticles.value
})))

const formatDate = (date) => {
  return date.toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'short',
    day: 'numeric'
  })
}

const refreshData = () => {
  isLoading.value = true
  
  setTimeout(() => {
    generateArticles()
    isLoading.value = false
  }, 1000)
}

const handleArticlePageChange = (page) => {
  isLoading.value = true
  
  setTimeout(() => {
    console.log(`Loading articles page ${page}`)
    isLoading.value = false
  }, 300)
}

const readArticle = (article) => {
  console.log('Reading article:', article.title)
}

const bookmarkArticle = (article) => {
  article.bookmarked = !article.bookmarked
  console.log(`${article.bookmarked ? 'Bookmarked' : 'Unbookmarked'}:`, article.title)
}
</script>

<style scoped>
.inline-pagination-demo {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

.content-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.refresh-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s;
}

.refresh-btn:disabled {
  background: #6c757d;
  cursor: not-allowed;
}

.spinning {
  animation: spin 1s linear infinite;
}

.article-list {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.article-item {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: transform 0.2s, box-shadow 0.2s;
}

.article-item:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

.article-meta {
  display: flex;
  gap: 1rem;
  margin-bottom: 0.5rem;
}

.article-date {
  color: #6c757d;
  font-size: 0.875rem;
}

.article-category {
  background: #e9ecef;
  color: #495057;
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 500;
}

.article-title {
  margin: 0 0 1rem 0;
  color: #212529;
  line-height: 1.4;
}

.article-excerpt {
  color: #6c757d;
  line-height: 1.6;
  margin: 0 0 1rem 0;
}

.article-actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.read-btn {
  padding: 0.5rem 1rem;
  background: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.875rem;
}

.bookmark-btn {
  padding: 0.5rem;
  background: none;
  border: 1px solid #dee2e6;
  border-radius: 4px;
  cursor: pointer;
  color: #6c757d;
  transition: all 0.2s;
}

.bookmark-btn:hover {
  background: #f8f9fa;
  color: #007bff;
}

.inline-pagination-wrapper {
  display: flex;
  justify-content: center;
  margin: 2rem 0;
}

.pagination-summary {
  text-align: center;
  color: #6c757d;
  font-size: 0.875rem;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

@media (max-width: 768px) {
  .content-header {
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
  }
  
  .article-meta {
    flex-direction: column;
    gap: 0.5rem;
  }
  
  .article-actions {
    flex-direction: column;
    gap: 0.5rem;
    align-items: stretch;
  }
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-pagination` | Main pagination container |
| `_c-page` | Individual page item (li element) |
| `_c-page-link` | Page link element |
| `_c-active` | Currently selected page |
| `_c-disabled` | Disabled page (first/last boundaries) |
| `_c-prev` | Previous button container |
| `_c-next` | Next button container |
| `_c-prev-link` | Previous button link |
| `_c-next-link` | Next button link |
| `_c-break` | Break view container |
| `_c-break-link` | Break view link |
| `_c-text-icon` | Icon styling within pagination |

## Internationalization

The component uses the following translation keys:

### Navigation Button Labels
| Key | Usage |
|-----|-------|
| `button.next` | Next page button fallback text |

### Translation Usage Examples
```vue
<!-- Next button with translation fallback -->
<a @click="nextPage()" 
   class="_c-next-link _c-page-link" 
   v-html="nextText || $t('button.next')"
/>
```

### Translation Notes

#### Minimal Translation Requirements
The Paginate component has minimal translation needs as it primarily uses:
- Icon-based navigation (arrow icons for prev/next/first/last)
- Numeric page indicators
- Ellipsis characters for break views

#### Customizable Button Text
Most button text is configurable through props:
- `prevText` - Previous button content (defaults to left arrow icon)
- `nextText` - Next button content (defaults to right arrow icon, fallback to translation)
- `firstButtonText` - First page button content
- `lastButtonText` - Last page button content
- `breakViewText` - Break view text (defaults to "…")

#### Accessibility Considerations
While the component doesn't use extensive translations, it supports:
- Proper tabindex management for keyboard navigation
- ARIA labels through parent component integration
- Screen reader friendly navigation structure

## Best Practices

### Configuration
- Choose appropriate pageRange values (3-7 works best)
- Use marginPages to show context at boundaries
- Configure loading states for better UX
- Test with various data set sizes
- Consider mobile-specific configurations

### Performance
- Implement proper data fetching strategies
- Use dependency injection for state management
- Avoid unnecessary re-renders
- Cache pagination calculations when possible
- Monitor component lifecycle

### Accessibility
- Ensure keyboard navigation works properly
- Use proper tabindex management
- Provide meaningful button text
- Test with screen readers
- Handle focus management correctly

### User Experience
- Show loading states during data fetching
- Provide clear navigation feedback
- Display pagination context (current page, total)
- Handle edge cases gracefully
- Consider mobile touch interactions

### Integration
- Use consistent group names for related components
- Provide proper pagination context through injection
- Handle filter changes appropriately
- Sync URL state when needed
- Test with various data sources

### Error Handling
- Handle missing pagination context gracefully
- Provide fallback for invalid page numbers
- Display meaningful error states
- Log pagination issues for debugging
- Test with edge cases (empty data, single page)

## Component Registration
```javascript
// Global registration
app.component('CodexPaginate', Paginate)

// Local registration  
import Paginate from '@/components/molecules/Paginate.vue'

export default {
  components: {
    CodexPaginate: Paginate
  }
}
``` 
</rewritten_file>