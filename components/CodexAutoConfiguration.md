# Codex Auto-Configuration System

## Overview
The Codex Auto-Configuration system is a dynamic initialization system that automatically sets up modals, loads required resources, and configures event listeners. It provides a declarative way to configure modals and their properties without manually creating DOM elements.

## Basic Usage
```javascript
Codex().set('auto_configure', {
  modals: {
    'codex-cart': {
      modalProps: { 
        'x-position': 'right',
        'y-position': 'bottom'
      },
      props: {
        'v-on:updated': 'open',
        'v-on:close': 'close'
      }
    }
  }
});
```

## Key Features
- Automatic modal initialization and configuration
- Dynamic resource loading (scripts, styles, translations)
- Event listener management
- Declarative modal configuration
- Automatic DOM element creation
- Built-in event delegation for modal controls
- Moment.js locale support
- Translation management

## Configuration Options

### Resource URLs
| Property | Type | Required | Description |
|----------|------|----------|-------------|
| translations | String | No | URL for translations file |
| scriptUrl | String | No | URL for main application script |
| styleUrl | String | No | URL for stylesheet |
| momentLocaleUrl | String | No | URL for Moment.js locale file |

### Modal Configuration
| Property | Type | Required | Description |
|----------|------|----------|-------------|
| modals | Object | No | Modal configurations keyed by component name |
| modalProps | Object | No | Properties applied to the modal wrapper |
| props | Object | No | Properties applied to the modal content component |

## Events
| Event Name | Description |
|------------|-------------|
| codex-initialise | Fired when translations are loaded |
| codex.modal.open.{modalName} | Opens specific modal |
| codex.modal.close.{modalName} | Closes specific modal |
| codex.modal.toggle.{modalName} | Toggles specific modal |

## Examples

### Basic Modal Configuration
```javascript
Codex().set('auto_configure', {
  modals: {
    'codex-login': {
      modalProps: { 
        'v-slot': '{close}',
        ':delay-close': 3000
      },
      props: {
        'v-on:close': 'close'
      }
    }
  }
});
```

### Modal with Custom Positioning
```javascript
Codex().set('auto_configure', {
  modals: {
    'codex-cart': {
      modalProps: { 
        'x-position': 'right',
        'y-position': 'bottom',
        'v-slot': '{open}'
      },
      props: {
        'v-on:updated': 'open',
        'v-on:close': 'close'
      }
    }
  }
});
```

### Modal with Custom Close Behavior
```javascript
Codex().set('auto_configure', {
  modals: {
    'codex-verify-sms': {
      modalProps: { 
        'v-slot': '{close}',
        ':delay-close': 3000,
        ':show-close-button': false
      },
      props: {
        'v-on:close': 'close'
      }
    }
  }
});
```

## HTML Triggers
```html
<!-- Open Modal -->
<button data-codex-modal-open="codex-login">Login</button>

<!-- Close Modal -->
<button data-codex-modal-close="codex-cart">Close Cart</button>

<!-- Toggle Modal -->
<button data-codex-modal-toggle="codex-register">Toggle Register</button>
```

## Generated DOM Structure
```html
<div id="codex-modal-name" data-codex="">
  <codex-modal modal-name="codex-modal-name" [modal-props]>
    <codex-modal-name [component-props]>
      <!-- Modal content -->
    </codex-modal-name>
  </codex-modal>
</div>
```

## Best Practices

### Recommended Usage
- Configure modals during initial application setup
- Use consistent naming conventions for modal components
- Implement proper event handling
- Configure appropriate close behaviors
- Use data attributes for modal triggers

### Accessibility Considerations
- Ensure modal triggers have proper ARIA labels
- Maintain keyboard navigation support
- Provide clear close mechanisms
- Handle focus management appropriately

### Performance Considerations
- Load resources asynchronously
- Use deferred script loading when appropriate
- Optimize translation file size
- Consider lazy loading for modal content

### Common Pitfalls to Avoid
- Missing required modal properties
- Incorrect event handler configuration
- Duplicate modal IDs
- Improper resource URL configuration
- Missing translation dependencies

### State Management
- Handle modal state changes properly
- Manage resource loading states
- Handle initialization errors
- Maintain proper event listener cleanup

## Component Registration
The auto-configuration system automatically registers modals with the prefix specified in their component name (e.g., 'codex-login', 'codex-cart'). 