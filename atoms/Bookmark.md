# Bookmark Component

## Overview
The Bookmark component provides an interactive heart-shaped bookmark button for user-authenticated content management. It features Vue.js transitions, internationalization support, composable-based bookmark state management, conditional rendering based on authentication, and ARIA accessibility compliance for favoriting content items.

## Basic Usage
```vue
<codex-bookmark
  :type="'article'"
  :item="articleId"
/>
```

## Key Features
- Authentication-dependent rendering (only shows for logged-in users)
- Interactive heart icon transitions (outline/filled states)
- Bookmark state management through composables
- Vue transition animations between bookmark states
- ARIA accessibility labels with internationalization
- Flexible item type and identifier support
- Click-to-toggle bookmark functionality
- Remix icon integration for consistent iconography

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| type | String | Yes | - | Category/type of bookmarkable content (article, video, etc.) |
| item | String\|Number\|Object | Yes | - | Unique identifier or object for the bookmarkable item |

## Dependencies

### Composables
| Composable | Usage |
|------------|-------|
| useBookmarks | Provides `isBookmarked()` and `toggleBookmark()` functions for state management |
| useCustomer | Provides `customer` property for authentication state checking |

### Internationalization
| Translation Key | Usage |
|-----------------|-------|
| bookmark.bookmark | ARIA label text for unbookmarked state |
| bookmark.bookmarked | ARIA label text for bookmarked state |

## Events
The Bookmark component does not emit custom events. State management is handled internally through the bookmark composable.

## Icon System
The component uses Remix icons for visual representation:
- **Unbookmarked State**: `ri-heart-2-line` (outline heart)
- **Bookmarked State**: `ri-heart-2-fill` (filled heart)
- **Transition**: Smooth fade transition between icon states

## Authentication Integration
The component integrates with authentication:
- **Authenticated Users**: Button renders and functions normally
- **Unauthenticated Users**: Component does not render (v-if="customer")
- **Customer State**: Uses useCustomer composable for auth status
- **Conditional Display**: Respects authentication requirements

## Bookmark State Management
State management through composables:
- **State Check**: `isBookmarked(type, item)` determines current bookmark status
- **State Toggle**: `toggleBookmark(type, item)` handles bookmark/unbookmark actions
- **Persistence**: Bookmark state persisted through composable implementation
- **Reactivity**: Component updates automatically when bookmark state changes

## Accessibility Features
The component provides comprehensive accessibility:
- **ARIA Labels**: Dynamic labels based on bookmark state
- **Button Semantics**: Proper button element for interaction
- **Screen Reader**: Clear state announcements
- **Internationalization**: Localized accessibility text
- **Focus Management**: Standard button focus behavior

## Transition System
Vue transitions provide smooth state changes:
- **Transition Name**: 'fade' transition between icon states
- **Mode**: 'out-in' ensures clean icon swapping
- **Duration**: CSS-controlled fade timing
- **Visual Feedback**: Clear state change indication

## Use Cases
Suitable for:
- **Content Favorites**: Articles, videos, products, tutorials
- **User Collections**: Personal content curation
- **Reading Lists**: Save-for-later functionality
- **Social Features**: Like/favorite social content
- **E-commerce**: Wishlist/favorites functionality

## Internationalization
The component requires these translation keys:
```javascript
{
  "bookmark": {
    "bookmark": "Add to bookmarks",
    "bookmarked": "Remove from bookmarks"
  }
}
```

## Examples

### Article Bookmark
```vue
<template>
  <article class="article-card">
    <h3>{{ article.title }}</h3>
    <p>{{ article.excerpt }}</p>
    
    <codex-bookmark
      :type="'article'"
      :item="article.id"
    />
  </article>
</template>

<script setup>
const article = ref({
  id: 123,
  title: 'Vue.js Best Practices',
  excerpt: 'Learn the essential patterns...'
})
</script>
```

### Video Content Bookmark
```vue
<template>
  <div class="video-player">
    <video-player :src="video.url" />
    
    <div class="video-controls">
      <codex-bookmark
        :type="'video'"
        :item="video"
      />
      
      <span class="video-title">{{ video.title }}</span>
    </div>
  </div>
</template>

<script setup>
const video = ref({
  id: 456,
  title: 'Advanced Vue Composition API',
  url: '/videos/vue-composition.mp4',
  duration: '15:30'
})
</script>
```

### Product Wishlist Bookmark
```vue
<template>
  <div class="product-card">
    <img :src="product.image" :alt="product.name" />
    
    <div class="product-info">
      <h4>{{ product.name }}</h4>
      <p class="price">${{ product.price }}</p>
    </div>
    
    <div class="product-actions">
      <button class="add-to-cart">Add to Cart</button>
      
      <codex-bookmark
        :type="'product'"
        :item="product.id"
      />
    </div>
  </div>
</template>

<script setup>
const product = ref({
  id: 'prod-789',
  name: 'Wireless Headphones',
  price: 199.99,
  image: '/images/headphones.jpg'
})
</script>
```

### Recipe Collection Bookmark
```vue
<template>
  <div class="recipe-list">
    <div
      v-for="recipe in recipes"
      :key="recipe.id"
      class="recipe-item"
    >
      <div class="recipe-header">
        <h5>{{ recipe.name }}</h5>
        
        <codex-bookmark
          :type="'recipe'"
          :item="recipe"
        />
      </div>
      
      <p class="recipe-description">{{ recipe.description }}</p>
      <div class="recipe-meta">
        <span>{{ recipe.cookTime }} mins</span>
        <span>{{ recipe.difficulty }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
const recipes = ref([
  {
    id: 'recipe-001',
    name: 'Chocolate Chip Cookies',
    description: 'Classic homemade cookies...',
    cookTime: 25,
    difficulty: 'Easy'
  }
])
</script>
```

### Blog Post Bookmark
```vue
<template>
  <div class="blog-post">
    <header class="post-header">
      <h1>{{ post.title }}</h1>
      
      <div class="post-meta">
        <span>By {{ post.author }}</span>
        <span>{{ formatDate(post.publishedAt) }}</span>
        
        <codex-bookmark
          :type="'blog-post'"
          :item="post.slug"
        />
      </div>
    </header>
    
    <div class="post-content" v-html="post.content"></div>
  </div>
</template>

<script setup>
const post = ref({
  slug: 'vue-3-features',
  title: 'Exploring Vue 3 New Features',
  author: 'Jane Developer',
  publishedAt: new Date('2024-01-15'),
  content: '<p>Vue 3 introduces many exciting features...</p>'
})

const formatDate = (date) => {
  return date.toLocaleDateString()
}
</script>
```

### Conditional Bookmark Display
```vue
<template>
  <div class="content-card">
    <div class="card-header">
      <h3>{{ content.title }}</h3>
      
      <!-- Only show bookmark for bookmarkable content -->
      <codex-bookmark
        v-if="content.isBookmarkable"
        :type="content.type"
        :item="content.id"
      />
    </div>
    
    <div class="card-content">
      {{ content.description }}
    </div>
  </div>
</template>

<script setup>
const content = ref({
  id: 'content-123',
  type: 'tutorial',
  title: 'CSS Grid Layout Guide',
  description: 'Master CSS Grid with practical examples',
  isBookmarkable: true
})
</script>
```

### Bookmark with Custom Styling
```vue
<template>
  <div class="premium-article">
    <div class="article-actions">
      <codex-bookmark
        :type="'premium-article'"
        :item="article.id"
        class="premium-bookmark"
      />
      
      <button class="share-button">Share</button>
    </div>
    
    <h2>{{ article.title }}</h2>
  </div>
</template>

<style scoped>
.premium-bookmark {
  --bookmark-color: #gold;
  --bookmark-size: 1.5rem;
}

.premium-bookmark :deep(.ri-heart-2-fill) {
  color: var(--bookmark-color);
  font-size: var(--bookmark-size);
}
</style>
```

### Multiple Item Type Bookmarks
```vue
<template>
  <div class="media-gallery">
    <div
      v-for="item in mediaItems"
      :key="item.id"
      class="media-item"
    >
      <img :src="item.thumbnail" :alt="item.title" />
      
      <div class="media-info">
        <h4>{{ item.title }}</h4>
        
        <codex-bookmark
          :type="item.mediaType"
          :item="item.id"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
const mediaItems = ref([
  {
    id: 'img-001',
    mediaType: 'image',
    title: 'Sunset Photography',
    thumbnail: '/thumbs/sunset.jpg'
  },
  {
    id: 'vid-002',
    mediaType: 'video',
    title: 'Time-lapse Clouds',
    thumbnail: '/thumbs/clouds.jpg'
  }
])
</script>
```

### Bookmark State Monitoring
```vue
<template>
  <div class="bookmark-demo">
    <div class="demo-content">
      <h3>{{ demoItem.title }}</h3>
      
      <codex-bookmark
        :type="'demo'"
        :item="demoItem.id"
      />
    </div>
    
    <div class="bookmark-status">
      <p>Status: {{ isCurrentlyBookmarked ? 'Bookmarked' : 'Not Bookmarked' }}</p>
      <p>Total Bookmarks: {{ bookmarkCount }}</p>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'
import useBookmarks from '@/composition/useBookmarks'

const demoItem = ref({
  id: 'demo-123',
  title: 'Bookmark Demo Item'
})

const { isBookmarked, getBookmarkCount } = useBookmarks()

const isCurrentlyBookmarked = computed(() => 
  isBookmarked('demo', demoItem.value.id)
)

const bookmarkCount = computed(() => 
  getBookmarkCount()
)
</script>
```

### Social Media Bookmark
```vue
<template>
  <div class="social-post">
    <div class="post-header">
      <div class="user-info">
        <img :src="post.user.avatar" class="avatar" />
        <span>{{ post.user.name }}</span>
      </div>
      
      <codex-bookmark
        :type="'social-post'"
        :item="post"
      />
    </div>
    
    <div class="post-content">
      <p>{{ post.content }}</p>
      <img v-if="post.image" :src="post.image" class="post-image" />
    </div>
    
    <div class="post-actions">
      <button class="like-button">Like</button>
      <button class="share-button">Share</button>
    </div>
  </div>
</template>

<script setup>
const post = ref({
  id: 'post-456',
  user: {
    name: 'Alice Johnson',
    avatar: '/avatars/alice.jpg'
  },
  content: 'Just learned about Vue 3 Composition API!',
  image: '/posts/vue-learning.jpg',
  timestamp: new Date()
})
</script>
```

### Bookmark Collection Manager
```vue
<template>
  <div class="bookmark-manager">
    <h2>My Bookmarks</h2>
    
    <div class="bookmark-filters">
      <button
        v-for="type in bookmarkTypes"
        :key="type"
        @click="filterBy(type)"
        :class="{ active: activeFilter === type }"
      >
        {{ type }}
      </button>
    </div>
    
    <div class="bookmarked-items">
      <div
        v-for="item in filteredBookmarks"
        :key="`${item.type}-${item.id}`"
        class="bookmarked-item"
      >
        <h4>{{ item.title }}</h4>
        
        <codex-bookmark
          :type="item.type"
          :item="item.id"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'
import useBookmarks from '@/composition/useBookmarks'

const activeFilter = ref('all')
const { getAllBookmarks } = useBookmarks()

const allBookmarks = computed(() => getAllBookmarks())

const bookmarkTypes = computed(() => {
  const types = new Set(allBookmarks.value.map(item => item.type))
  return ['all', ...Array.from(types)]
})

const filteredBookmarks = computed(() => {
  if (activeFilter.value === 'all') {
    return allBookmarks.value
  }
  return allBookmarks.value.filter(item => item.type === activeFilter.value)
})

const filterBy = (type) => {
  activeFilter.value = type
}
</script>
```

### Bookmark Analytics Integration
```vue
<template>
  <div class="content-with-analytics">
    <article>
      <h1>{{ article.title }}</h1>
      <div class="article-content">{{ article.content }}</div>
    </article>
    
    <div class="article-actions">
      <codex-bookmark
        :type="'article'"
        :item="article.id"
        @click="trackBookmarkAction"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import useBookmarks from '@/composition/useBookmarks'
import { trackEvent } from '@/utils/analytics'

const article = ref({
  id: 'article-789',
  title: 'Advanced JavaScript Techniques',
  content: 'In this article, we explore...'
})

const { isBookmarked } = useBookmarks()

const trackBookmarkAction = () => {
  const action = isBookmarked('article', article.value.id) ? 'unbookmark' : 'bookmark'
  
  trackEvent('bookmark_action', {
    action: action,
    content_type: 'article',
    content_id: article.value.id,
    content_title: article.value.title
  })
}
</script>
```

## CSS Classes
- `_c-bookmark`: Base bookmark button styling
- `_c-bookmarked`: Applied when item is bookmarked (for visual state indication)
- `ri-heart-2-line`: Remix icon class for unbookmarked state
- `ri-heart-2-fill`: Remix icon class for bookmarked state
- `fade`: Transition class for icon state changes

## Best Practices

### Recommended Usage Patterns
- Use descriptive type strings that match your content categories
- Provide unique, stable identifiers for items
- Combine with authentication checks for user-specific features
- Use consistent bookmark types across your application
- Implement bookmark state persistence through proper composable design
- Test bookmark functionality across different user states
- Provide visual feedback for bookmark state changes

### Common Pitfalls to Avoid
- Not checking authentication state before rendering
- Using non-unique or changing item identifiers
- Missing internationalization for accessibility labels
- Not handling bookmark state persistence properly
- Forgetting to test transition animations
- Using complex objects as item identifiers without proper serialization
- Not providing proper ARIA labels for screen readers

### Accessibility Considerations
- Use proper button semantics for interactive elements
- Provide clear ARIA labels that describe current state
- Ensure sufficient color contrast for bookmark icons
- Support keyboard navigation and activation
- Test with screen readers for proper state announcements
- Use semantic HTML structure for bookmark buttons
- Provide visual state indicators beyond color alone

### Performance Considerations
- Use lightweight item identifiers to minimize memory usage
- Implement efficient bookmark state checking
- Avoid frequent bookmark state re-calculations
- Use computed properties for bookmark status
- Optimize composable state management
- Consider lazy loading for bookmark collections
- Cache bookmark state when appropriate

### State Management
- Use reactive composables for bookmark state
- Handle bookmark persistence consistently
- Coordinate bookmark state across components
- Provide proper error handling for bookmark actions
- Handle authentication state changes gracefully
- Synchronize bookmark state with backend services
- Manage bookmark collections efficiently

### Authentication Integration
- Always check authentication before rendering bookmark buttons
- Handle authentication state changes smoothly
- Provide appropriate fallbacks for unauthenticated users
- Integrate with your authentication system properly
- Handle authentication errors gracefully
- Test bookmark behavior across authentication states

### User Experience
- Provide immediate visual feedback for bookmark actions
- Use consistent bookmark iconography across the application
- Handle loading states during bookmark operations
- Provide clear indication of bookmark status
- Support undo functionality where appropriate
- Handle network errors gracefully for bookmark sync

### Content Organization
- Use consistent type categorization for bookmarkable content
- Organize bookmarks in meaningful collections
- Provide search and filtering for bookmark collections
- Handle bookmark synchronization across devices
- Implement bookmark export and import functionality
- Support bookmark organization and tagging

### Analytics and Tracking
- Track bookmark usage patterns for insights
- Monitor bookmark engagement metrics
- Analyze content popularity through bookmark data
- Track user engagement with bookmarked content
- Use bookmark data for content recommendations
- Monitor bookmark performance and errors

### Data Management
- Design efficient bookmark storage schema
- Handle bookmark data migration properly
- Implement bookmark cleanup for deleted content
- Handle large bookmark collections efficiently
- Provide bookmark backup and restore functionality
- Optimize bookmark data synchronization

## Component Registration
The component is registered as `codex-bookmark` in the application. 