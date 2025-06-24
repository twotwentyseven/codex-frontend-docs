# Referral Component

## Overview
The Referral component provides a comprehensive referral system interface with URL sharing, social media integration, and multiple sharing methods. It features referral URL generation, clipboard copy functionality, native sharing API support, social media sharing, and customizable sharing modal for effective referral program management.

## Basic Usage
```vue
<codex-referral
  :title="'Refer Friends'"
  :title-tag="'h2'"
/>
```

## Key Features
- Automatic referral URL generation
- Clipboard copy functionality with fallback
- Native sharing API integration
- Social media sharing buttons
- Email and SMS sharing options
- Share modal with multiple platforms
- Customer authentication integration
- Responsive sharing interface
- Toast notifications for user feedback
- Fallback sharing methods for older browsers

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| title | String\|Boolean | No | false | Custom title for the referral section |
| titleTag | String | No | 'h1' | HTML tag for the title element |

### Common Props
All common props from `@/config/common` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| referral-shared | `platform: String, url: String` | Emitted when referral is shared via any method |
| url-copied | `url: String` | Emitted when referral URL is copied to clipboard |

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content (overrides default title and description) -->
</template>
```

### Content Slot
```vue
<template #content>
  <!-- Custom content and sharing interface -->
</template>
```

## Referral URL Generation
The component automatically generates referral URLs:
- **Base URL**: Uses current domain
- **Path Structure**: `/{website_url}?register=true&referral_code={customer.referral_code}`
- **Code Source**: Uses authenticated customer's referral code
- **Real-time Updates**: URL updates when customer data changes

## Sharing Methods
The component supports multiple sharing approaches:

### Native Sharing API
- **Modern Browsers**: Uses `navigator.share()` when available
- **Fallback**: Opens share modal if native sharing unavailable
- **Share Data**: Includes title, text, and URL

### Clipboard Copy
- **Modern API**: Uses `navigator.clipboard.writeText()` for secure contexts
- **Fallback**: Traditional `document.execCommand('copy')` for older browsers
- **User Feedback**: Toast notifications for success/failure

### Social Media Platforms
- **Facebook**: Direct sharing with URL
- **Twitter**: Tweet with custom text and URL
- **LinkedIn**: Professional network sharing
- **Email**: Mailto link with subject and body
- **SMS**: SMS link with message text

## Share Modal
When native sharing is unavailable, displays modal with:
- **Platform Buttons**: Visual buttons for each sharing method
- **Copy Function**: Direct copy functionality
- **Social Icons**: Platform-specific iconography
- **Accessibility**: Proper labels and keyboard navigation

## Internationalization

The `Referral` component uses translation keys for referral sharing interface and social media integration:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `refer.refer_title` | Default title for referral section | Component header |
| `refer.refer_info` | Referral program description | Information paragraph |
| `refer.referral_url` | Referral URL field label | Input field label |
| `refer.copy` | Copy button aria-label and action text | Accessibility and UI |
| `refer.share` | Share button aria-label and modal title | Sharing functionality |
| `refer.share_description` | Share modal description | Modal content |
| `refer.share_facebook` | Facebook sharing aria-label | Social media accessibility |
| `refer.share_twitter` | Twitter sharing aria-label | Social media accessibility |
| `refer.share_linkedin` | LinkedIn sharing aria-label | Social media accessibility |
| `refer.share_email` | Email sharing aria-label | Email sharing accessibility |
| `refer.share_sms` | SMS sharing aria-label | SMS sharing accessibility |
| `refer.successfully_copied` | Success message for copy action | Toast notification |
| `refer.copy_failed` | Error message for copy failure | Toast notification |
| `refer.success_msg` | Success message for sharing | Toast notification |

### Social Media Content Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `refer.share_title` | Social media share title | Native share dialog |
| `refer.twitter_text` | Twitter post text | Twitter sharing |
| `refer.mail_text` | Email subject line | Email sharing |
| `refer.mail_body` | Email body text | Email sharing |
| `refer.sms_text` | SMS message text | SMS sharing |

### Implementation Examples

```vue
<!-- Header content -->
<codex-title :tag="titleTag" :content="title || $t('refer.refer_title')"/>
<codex-paragraph tag="p" :content="$t('refer.refer_info')"/>

<!-- Referral URL input -->
<codex-input-field
    v-model="displayUrl"
    :readonly="true"
    :label="$t('refer.referral_url')"
    @click.prevent="copyToClipboard"
/>

<!-- Action buttons -->
<button @click.prevent="copyToClipboard" :aria-label="$t('refer.copy')">
    <i class="ri-file-copy-line"></i>
</button>

<button @click.prevent="shareFunction" :aria-label="$t('refer.share')">
    <i class="ri-share-line"></i>
</button>

<!-- Share modal -->
<div class="_c-title">{{ $t('refer.share') }}</div>
<div class="_c-description">{{ $t('refer.share_description') }}</div>

<!-- Social sharing buttons -->
<button @click.prevent="copyToClipboard" :aria-label="$t('refer.copy')">
    <i class="ri-file-copy-line"></i>
</button>

<button @click.prevent="openSocialShare('facebook')" :aria-label="$t('refer.share_facebook')">
    <i class="ri-facebook-line"></i>
</button>

<button @click.prevent="openSocialShare('twitter')" :aria-label="$t('refer.share_twitter')">
    <i class="ri-twitter-line"></i>
</button>

<button @click.prevent="openSocialShare('linkedin')" :aria-label="$t('refer.share_linkedin')">
    <i class="ri-linkedin-line"></i>
</button>

<button @click.prevent="openSocialShare('email')" :aria-label="$t('refer.share_email')">
    <i class="ri-mail-line"></i>
</button>

<button @click.prevent="openSocialShare('sms')" :aria-label="$t('refer.share_sms')">
    <i class="ri-message-2-line"></i>
</button>
```

### Social Share URL Configuration

```javascript
const socialShareUrls = computed(() => ({
    facebook: `https://www.facebook.com/share.php?u=${encodeURIComponent(referralUrl.value)}`,
    twitter: `https://twitter.com/intent/tweet?text=${encodeURIComponent(t('refer.twitter_text'))}&url=${encodeURIComponent(referralUrl.value)}`,
    linkedin: `https://www.linkedin.com/sharing/share-offsite/?url=${encodeURIComponent(referralUrl.value)}`,
    email: `mailto:?subject=${encodeURIComponent(t('refer.mail_text'))}&body=${encodeURIComponent(t('refer.mail_body'))} ${encodeURIComponent(referralUrl.value)}`,
    sms: `sms:?&body=${encodeURIComponent(t('refer.sms_text'))} ${encodeURIComponent(referralUrl.value)}`
}));
```

### Native Share API Integration

```javascript
const shareData = computed(() => ({
    title: t('refer.share_title'),
    text: t('refer.twitter_text'),
    url: referralUrl.value
}));
```

### Toast Notifications

```javascript
// Success and error feedback
toast.success(t('refer.successfully_copied'));
toast.error(t('refer.copy_failed'));
toast.success(t('refer.success_msg'));
```

### Referral URL Generation

The component generates referral URLs using customer data:

```javascript
const referralPath = computed(() => 
    `/${window.codex.urls.website_url}?register=true&referral_code=${customer.value?.referral_code}`
);
```

### Notes
- Comprehensive social media sharing support with platform-specific content
- Accessibility-first approach with aria-labels for all buttons
- Fallback sharing modal when native Web Share API is unavailable
- Toast notifications for user feedback on actions
- URL encoding for safe social media sharing
- Integration with customer referral codes for tracking

## Examples

### Basic Implementation
```vue
<codex-referral />
```

### With Custom Title
```vue
<codex-referral
  :title="'Invite Your Friends'"
  :title-tag="'h3'"
/>
```

### Custom Header Content
```vue
<codex-referral>
  <template #header>
    <div class="referral-header">
      <h2>Share the Love</h2>
      <p>Invite friends and earn rewards when they join!</p>
      <div class="referral-benefits">
        <div class="benefit">
          <i class="reward-icon"></i>
          <span>You get $10 credit</span>
        </div>
        <div class="benefit">
          <i class="friend-icon"></i>
          <span>Friend gets $10 credit</span>
        </div>
      </div>
    </div>
  </template>
</codex-referral>
```

### Custom Content with Analytics
```vue
<codex-referral>
  <template #content>
    <div class="custom-referral-interface">
      <div class="referral-stats">
        <div class="stat">
          <span class="value">{{ referralStats.totalReferred }}</span>
          <span class="label">Friends Referred</span>
        </div>
        <div class="stat">
          <span class="value">${{ referralStats.totalEarned }}</span>
          <span class="label">Credits Earned</span>
        </div>
      </div>
      
      <div class="referral-url-container">
        <codex-input-field
          type="text"
          :value="referralUrl"
          :readonly="true"
          :label="'Your Referral Link'"
          @click="copyToClipboard"
        />
        <div class="share-actions">
          <button @click="copyToClipboard" class="copy-btn">
            <i class="copy-icon"></i>
            Copy
          </button>
          <button @click="shareFunction" class="share-btn">
            <i class="share-icon"></i>
            Share
          </button>
        </div>
      </div>
    </div>
  </template>
</codex-referral>
```

### With Event Tracking
```vue
<codex-referral
  @referral-shared="trackReferralShare"
  @url-copied="trackUrlCopy"
/>

<script setup>
const trackReferralShare = (platform, url) => {
  analytics.track('referral_shared', {
    platform: platform,
    referral_url: url,
    customer_id: customer.value?.id
  })
}

const trackUrlCopy = (url) => {
  analytics.track('referral_url_copied', {
    referral_url: url,
    customer_id: customer.value?.id
  })
}
</script>
```

### In Rewards Section
```vue
<div class="rewards-section">
  <div class="rewards-overview">
    <h2>Earn Rewards</h2>
    <p>Refer friends and both earn credits!</p>
  </div>
  
  <codex-referral 
    :title="false"
    :title-tag="'h3'"
  />
  
  <div class="referral-history">
    <!-- Display referral history -->
  </div>
</div>
```

### Mobile-Optimized Implementation
```vue
<codex-referral>
  <template #content>
    <div class="mobile-referral-interface">
      <div class="referral-url-display">
        <input 
          type="text" 
          :value="displayUrl" 
          readonly 
          class="url-input"
          @click="copyToClipboard"
        />
      </div>
      
      <div class="mobile-share-grid">
        <button @click="copyToClipboard" class="share-option">
          <i class="copy-icon"></i>
          <span>Copy Link</span>
        </button>
        
        <button @click="shareFunction" class="share-option">
          <i class="share-icon"></i>
          <span>Share</span>
        </button>
        
        <button @click="openSocialShare('sms')" class="share-option">
          <i class="sms-icon"></i>
          <span>Text</span>
        </button>
        
        <button @click="openSocialShare('email')" class="share-option">
          <i class="email-icon"></i>
          <span>Email</span>
        </button>
      </div>
    </div>
  </template>
</codex-referral>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-referral-card`: Referral-specific card styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-referral-container`: Referral interface container
- `_c-copy-btn`: Copy button styling
- `_c-share-btn`: Share button styling
- `_c-text-icon`: Icon styling
- `_c-dialog-header`: Modal header styling
- `_c-dialog-content`: Modal content styling
- `_c-title`: Title styling
- `_c-description`: Description text styling
- `_c-share-targets`: Share buttons container

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Provide clear feedback for copy/share actions
- Use appropriate fallbacks for older browsers
- Implement proper error handling for sharing failures
- Track referral sharing for analytics
- Use meaningful share text for different platforms
- Provide accessible sharing interfaces
- Handle network connectivity issues

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing fallback sharing methods
- Forgetting to provide user feedback for actions
- Not handling clipboard API failures
- Missing error handling for social sharing
- Insufficient mobile optimization
- Not tracking referral analytics
- Missing accessibility considerations

### Accessibility Considerations
- Provide clear labels for all sharing buttons
- Use appropriate ARIA attributes for interactive elements
- Ensure keyboard navigation for all sharing methods
- Provide screen reader friendly sharing feedback
- Include proper focus management for modals
- Use semantic HTML for sharing interface
- Ensure adequate color contrast for all elements
- Handle disabled states properly

### Error Handling
- Handle clipboard API failures gracefully
- Provide fallbacks when sharing APIs unavailable
- Display clear error messages for failed operations
- Handle network connectivity issues
- Manage authentication errors appropriately
- Provide retry mechanisms for failed shares
- Clear error states when operations succeed

### State Management
- Track customer authentication changes
- Handle referral code updates dynamically
- Manage modal state properly
- Coordinate share state across methods
- Handle concurrent sharing operations
- Track sharing analytics appropriately
- Manage URL generation state

### Performance Considerations
- Lazy load sharing modal components
- Optimize URL generation performance
- Minimize re-renders during state changes
- Handle large sharing operations efficiently
- Implement proper cleanup for event listeners
- Cache referral URL appropriately
- Optimize social sharing URL generation

### Security Considerations
- Validate referral codes properly
- Sanitize sharing content appropriately
- Handle URL generation securely
- Protect against XSS in share content
- Validate clipboard operations
- Secure social sharing URLs
- Handle authentication tokens properly

### Browser Compatibility
- Test clipboard API across browsers
- Provide fallbacks for sharing APIs
- Handle feature detection properly
- Test modal functionality across devices
- Ensure mobile sharing works correctly
- Handle different screen sizes appropriately
- Test social sharing across platforms

### Analytics Integration
- Track sharing method preferences
- Monitor referral conversion rates
- Analyze sharing platform effectiveness
- Track copy vs share usage
- Monitor error rates for sharing methods
- Analyze mobile vs desktop sharing patterns
- Track referral URL generation patterns

## Component Registration
The component is registered as `codex-referral` in the application. 