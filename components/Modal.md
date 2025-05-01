# Modal Component

## Overview
The Modal component is a versatile and customizable overlay dialog that can be opened and closed from anywhere in the DOM. It supports flexible positioning, transitions, and can embed any content while providing options for overlay behavior and close button visibility.

## Basic Usage
```vue
<codex-modal
  :default-state="false"
  modal-name="my-modal"
  x-position="center"
  y-position="center"
>
  <div>Modal Content Here</div>
</codex-modal>
```

## Key Features
- Teleport functionality to render modal anywhere in the DOM
- Configurable positioning (left/center/right and top/middle/bottom)
- Optional overlay and close button
- Custom transition support
- URL state synchronization
- Delayed close functionality
- Event-based control system
- Responsive height calculations
- Nested component support

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| defaultState | Boolean | No | false | Initial open state of the modal |
| modalName | String | No | 'default' | Unique identifier for the modal |
| portalSelector | String\|Boolean | No | 'body' | Target DOM element for teleporting the modal |
| useOverlay | Boolean | No | true | Whether to show a backdrop overlay |
| transition | String | No | 'fade' | Transition animation name |

### Position Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| xPosition | String | No | 'center' | Horizontal position ('left', 'right', 'center') |
| yPosition | String | No | 'center' | Vertical position ('top', 'bottom', 'center') |

### Behavior Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| showCloseButton | Boolean | No | true | Whether to show the close button |
| delayClose | Number\|Boolean | No | false | Delay in ms before closing, false for no delay |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| update:open | Boolean | Emitted when modal open state changes |
| open | None | Emitted when modal opens |
| close | None | Emitted when modal closes |

## Slots
| Slot Name | Description |
|-----------|-------------|
| default | Main content slot with scoped slots for control functions |

## States and Transitions
1. Closed (default)
2. Opening (transition in)
3. Open
4. Closing (with optional delay)
5. Closed (transition out)

## DOM Events
The modal responds to the following custom events:
- `codex.modal.toggle.{modalName}`
- `codex.modal.open.{modalName}`
- `codex.modal.close.{modalName}`
- `codex.modals.close` (global close event)

## Examples

### Basic Modal
```vue
<codex-modal modal-name="basic-modal">
  <div class="_c-content">
    <h2>Basic Modal</h2>
    <p>Modal content goes here</p>
  </div>
</codex-modal>
```

### Positioned Modal with Custom Transition
```vue
<codex-modal
  modal-name="custom-modal"
  x-position="right"
  y-position="top"
  transition="slide-in"
  :use-overlay="false"
>
  <div class="_c-content">
    <h2>Positioned Modal</h2>
    <p>This modal appears in the top-right</p>
  </div>
</codex-modal>
```

### Modal with Delayed Close
```vue
<codex-modal
  modal-name="delayed-modal"
  :delay-close="3000"
>
  <template #default="{ delayClose }">
    <div class="_c-content">
      <h2>Auto-closing Modal</h2>
      <button @click="delayClose()">Close with Delay</button>
    </div>
  </template>
</codex-modal>
```

## CSS Classes
- `_c-modal`: Main modal container
- `_c-modal-inner`: Inner modal content wrapper
- `_c-no-overlay`: Applied when overlay is disabled
- `_c-close`: Close button
- `_c-x-{position}`: Horizontal position classes
- `_c-y-{position}`: Vertical position classes
- `cdx_delayed-close-countdown`: Progress indicator for delayed close
- `cdx_auto-close`: Auto-close animation class

## Best Practices

### Recommended Usage
- Use meaningful modal names for better tracking and control
- Implement proper error handling in modal content
- Consider mobile viewports when positioning modals
- Use appropriate transitions for different use cases

### Accessibility Considerations
- Ensure modal content is keyboard navigable
- Use ARIA labels appropriately
- Maintain proper focus management
- Provide clear close mechanisms

### Performance Considerations
- Lazy load modal content when possible
- Use appropriate transition durations
- Consider content height changes
- Clean up event listeners properly

### Common Pitfalls to Avoid
- Nesting modals without proper management
- Forgetting to handle modal state in component unmounting
- Not considering mobile viewport constraints
- Overloading modal content causing layout issues

### State Management
- Use URL parameters for modal state when appropriate
- Handle state changes through proper events
- Manage modal stack order when using multiple modals
- Clean up modal state on route changes

## Component Registration
The component is registered as `codex-modal` in the application. 