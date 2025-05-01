# Router Configuration

## Overview

The Codex application supports Vue Router integration for creating dynamic, client-side navigation within components. This document explains how to configure routing for your Codex components.

## Basic Router Configuration

To enable routing on a Codex component, use the following data attributes:

```html
<div id="my-section" 
     data-codex 
     data-codex-router="true" 
     data-codex-router-base="/my-section/">
</div>
```

### Basic Router Attributes

| Attribute | Description | Example |
|-----------|-------------|---------|
| `data-codex-router` | Enables routing for this component | `"true"` |
| `data-codex-router-base` | Sets the base URL path for all routes | `"/account/"` |
| `data-codex-router-mode` | Sets the router mode | `"hash"` or `"history"` |
| `data-codex-router-routes` | Simple JSON route configuration | See example below |

### Tab Label Configuration

The tab system supports three ways to display labels:

1. Simple Text/Translation:
```javascript
{
    "path": "/overview",
    "meta": {
        "label": "@:account.overview",
        "icon": "ri-home-4-line"
    }
}
```

2. Label with Component (e.g., for notifications):
```javascript
{
    "path": "/notifications",
    "meta": {
        "label": "@:account.notifications",
        "icon": "ri-notification-line",
        "labelComponent": "codex-notification-count",
        "labelProps": {
            "type": "unread"
        }
    }
}
```

3. Complete Tab Replacement (e.g., for logout):
```javascript
{
    "path": "/logout",
    "meta": {
        "replaceTab": true,
        "labelComponent": "codex-logout-button",
        "labelProps": {
            "variant": "tab",
            "redirectTo": "/login"
        }
    }
}
```

### Simple Routes Example

```html
<div id="simple-tabs" 
     data-codex 
     data-codex-router="true"
     data-codex-router-base="/tabs/" 
     data-codex-router-routes='[
       {
         "path": "/",
         "component": "codex-tab-content",
         "meta": {
           "label": "@:account.overview",
           "icon": "ri-home-4-line"
         }
       },
       {
         "path": "/notifications",
         "component": "codex-tab-content",
         "meta": {
           "label": "@:account.notifications",
           "icon": "ri-notification-line",
           "labelComponent": "codex-notification-count",
           "labelProps": {
             "type": "unread"
           }
         }
       },
       {
         "path": "/logout",
         "component": "codex-tab-content",
         "meta": {
           "replaceTab": true,
           "labelComponent": "codex-logout-button",
           "labelProps": {
             "variant": "tab"
           }
         }
       }
     ]'>
</div>
```

### Tab Content Components

The `codex-tab-content` component is a powerful component that can be used to create tabbed interfaces with predefined content presets. You can either specify child components directly or use one of the built-in presets.

#### Using Presets

The following presets are available for tab content:

| Preset Name | Description | Components Included |
|-------------|-------------|-------------------|
| `accountOverview` | Account overview page | Title, Available Credits, Bookings Completed, Subscriptions Overview, Upcoming Bookings, Waitlists, Video Views |
| `accountCredits` | Credit management | Title, Unused Credits, Used Credits |
| `accountDetails` | Account details | Title, Personal Details, Contact Preferences |
| `accountFavourites` | Favorite items | Title, Favorites List |
| `accountHistory` | Booking history | Title, Booking History, Purchase History |
| `accountMembership` | Membership details | Title, Membership Details, Membership Benefits |
| `accountMilestones` | User achievements | Title, Milestones Overview, Achievements |
| `accountNotifications` | Notification settings | Title, Notification Preferences, Notification History |

Example using a preset:
```html
<div data-codex 
     data-codex-router="true"
     data-codex-router-routes='[
       {
         "path": "/credits",
         "component": "codex-tab-content",
         "props": {
           "preset": "accountCredits"
         },
         "meta": {
           "label": "Credits",
           "icon": "ri-money-pound-circle-line"
         }
       }
     ]'>
</div>
```

#### Using Custom Child Components

You can also specify custom child components directly using the `childComponents` array in the route's meta data:

```html
<div data-codex 
     data-codex-router="true"
     data-codex-router-routes='[
       {
         "path": "/",
         "component": "codex-tab-content",
         "meta": {
           "label": "@:account.overview",
           "icon": "ri-home-4-line",
           "childComponents": [
             { 
               "component": "codex-title", 
               "props": {
                 "tag": "div",
                 "className": "_c-title",
                 "content": "@:account.hello|name:customer.first_name"
               }
             },
             {
               "component": "codex-available-credits",
               "props": {
                 "targetPath": "credits",
                 "perPage": 10
               }
             }
           ]
         }
       }
     ]'>
</div>
```

#### Navigation Between Tabs

Components can include navigation properties to enable jumping between tabs:

- `targetPath`: Specifies which tab/path to navigate to
- `onTabChange`: Automatically included to handle tab changes

Example of a component with navigation:
```html
{
  "component": "codex-available-credits",
  "props": {
    "targetPath": "credits",
    "perPage": 10
  }
}
```

### Component Child Configuration

Components can be configured with child components through the route configuration. This can be done in two ways:

1. Using the `meta.childComponents` property (Recommended):
```html
<div data-codex 
     data-codex-router="true"
     data-codex-router-routes='[
       {
         "path": "/",
         "component": "ParentComponent",
         "meta": {
           "label": "Overview",
           "childComponents": [
             { "component": "ChildComponent1" },
             { "component": "ChildComponent2", "props": { "customProp": "value" } }
           ]
         }
       }
     ]'>
</div>
```

Child components specified this way will override any default childComponents defined in the parent component. If no childComponents are specified, the component will use its default configuration.

## Complex Router Configuration

For more complex routing needs (nested routes, navigation guards, etc.), use the `data-codex-router-config` attribute to reference a predefined configuration:

```html
<div id="product-browser" 
     data-codex 
     data-codex-router="true"
     data-codex-router-base="/shop/" 
     data-codex-router-config="product-browser">
</div>
```

### Defining Complex Router Configurations

Before initializing your app, define complex router configurations in the global `window.codex.routerConfigs` object:

```javascript
window.codex.routerConfigs = {
  'product-browser': {
    routes: [
      {
        path: '/',
        component: 'ProductList',
        children: [
          { 
            path: ':category',
            component: 'CategoryView',
            props: true
          }
        ]
      },
      {
        path: '/product/:id',
        component: 'ProductDetail',
        props: true
      }
    ],
    // scrollBehavior: (to, from, savedPosition) => {
    //   return savedPosition || { top: 0, behavior: 'smooth' };
    // }
  }
};
```

### Available Configuration Options

| Option | Description | Example |
|--------|-------------|---------|
| `routes` | Array of route objects | See Vue Router documentation |
| `scrollBehavior` | Controls scroll behavior on navigation | Function or object |

## Examples

### Complex Routing Example

```html
<div id="product-browser" 
     data-codex 
     data-codex-router="true"
     data-codex-router-base="/shop/" 
     data-codex-router-config="product-browser">
</div>
```

## Translation Patterns in Router Configuration

The router configuration supports translation patterns using the `@:` syntax. This allows for dynamic, translated content in route labels and child components.

### Translation Pattern Syntax

- Simple translation: `@:translation.key`
- With parameters: `@:translation.key|name:customer.first_name`
- Multiple parameters: `@:translation.key|first:customer.first_name,last:customer.last_name`

### Example with Translations

```html
<div data-codex 
     data-codex-router="true"
     data-codex-router-routes='[
       {
         "path": "/",
         "component": "codex-account-overview",
         "meta": {
           "label": "@:account.overview",
           "icon": "ri-home-line",
           "childComponents": [
             { 
               "component": "codex-title",
               "props": {
                 "content": "@:account.hello|name:customer.first_name",
                 "tag": "div"
               }
             },
             { 
               "component": "codex-subtitle",
               "props": {
                 "title": "@:account.welcome|first:customer.first_name,last:customer.last_name"
               }
             }
           ]
         }
       }
     ]'>
</div>
```

### Translation Features

| Feature | Description | Example |
|---------|-------------|---------|
| Simple Translation | Basic translation key | `"@:account.overview"` |
| Parameter Resolution | Include dynamic values | `"@:account.hello|name:customer.first_name"` |
| Multiple Parameters | Multiple parameters | `"@:welcome|first:customer.first_name,last:customer.last_name"` |
| Nested Translations | Translations in child components | See example above |

### Implementation Details

The translation system works by:
1. Creating a context object with the required data (e.g., customer information)
2. Processing translation patterns by resolving parameters against this context
3. Applying the resolved values to the translation keys

Example implementation in a component:
```javascript
// Create the translation context
const translationContext = computed(() => ({
    customer: customer.value
}));

// Process translations with context
const processedProps = processTranslations(props, translationContext.value);
```

### Important Notes

- Translation patterns are processed using the provided context
- Values are resolved reactively and will update when the source data changes
- The `@:` prefix identifies a translation pattern
- Regular strings (without `@:`) pass through unchanged
- Parameters are resolved using dot notation (e.g., `customer.first_name`)

## Troubleshooting

- If routes aren't working, check browser console for errors
- For hash mode, URLs will include `#` before the route path
- Make sure your route components are properly defined

## Further Reading

- [Vue Router Documentation](https://router.vuejs.org/)
- [Vue Router Navigation Guards](https://router.vuejs.org/guide/advanced/navigation-guards.html)
- [Vue Router Nested Routes](https://router.vuejs.org/guide/essentials/nested-routes.html)