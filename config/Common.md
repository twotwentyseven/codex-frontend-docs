# Common Configuration Module

## Overview
The Common Configuration module provides shared functionality, props, and utilities used across multiple components in the Codex system. It includes common props definitions, media query handling, and utility functions for formatting and data manipulation.

## Key Features
- Shared prop definitions
- Media query handling
- Currency formatting
- String manipulation
- Tag filtering
- Mobile detection
- Loading state management

## Common Props

### UI Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| enableBorder | Boolean | No | true | Enable border styling |
| className | String | No | '' | Additional CSS classes |
| showPoweredBy | Boolean | No | true | Show powered by branding |

### Navigation Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| targetPath | String | No | null | Navigation target path |
| onTabChange | Function | No | - | Tab change handler |

### Content Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| title | String | No | - | Content title |
| titleTag | String | No | 'h1' | HTML tag for title (h1-h6, div) |
| truncateLength | Number/Boolean | No | false | Length to truncate text |
| filterTags | Array | No | [] | Tags for filtering content |

## Composables

### useMediaQuery
A composable for handling responsive media queries.

```javascript
const isMobile = useMediaQuery('(max-width: 767px)');
```

#### Parameters
| Name | Type | Description |
|------|------|-------------|
| query | String | Media query string |

#### Returns
| Type | Description |
|------|-------------|
| Ref<boolean> | Reactive reference to media query match state |

### useCommon
A composable providing common functionality used across components.

```javascript
const { formatCurrency, truncateString, filteredTags } = useCommon(props);
```

#### Parameters
| Name | Type | Description |
|------|------|-------------|
| props | Object | Component props object |

#### Returns
| Name | Type | Description |
|------|------|-------------|
| open | Ref<boolean> | Toggle state |
| toggleOpen | Function | Toggle handler |
| handleClick | Function | Click event handler |
| formatCurrency | Function | Currency formatter |
| truncateString | Function | String truncation utility |
| filteredTags | Function | Tag filtering utility |
| isMobile | Ref<boolean> | Mobile state detector |
| loadingItems | ComputedRef | Loading items array |

## Utility Functions

### formatCurrency
Formats numbers as currency based on locale settings.

```javascript
const price = formatCurrency(1000); // "£10.00"
```

#### Parameters
| Name | Type | Description |
|------|------|-------------|
| value | Number | Amount to format |

#### Configuration
Uses `window.codex` settings:
- `locale`: Currency locale (default: 'Europe/London')
- `currency`: Currency code (default: 'GBP')
- `currencyDecimals`: Decimal places (default: 2)

### truncateString
Truncates text to specified length with ellipsis.

```javascript
const text = truncateString("Long text...", 10); // "Long te..."
```

#### Parameters
| Name | Type | Description |
|------|------|-------------|
| text | String | Text to truncate |
| length | Number | Maximum length |

### filteredTags
Filters tags based on provided patterns.

```javascript
const tags = filteredTags({
  display_tags: ['tag1', 'tag2']
});
```

#### Parameters
| Name | Type | Description |
|------|------|-------------|
| collection | Object | Object containing tags |
| filterTags | Array | Filter patterns |

## Best Practices

### Using Common Props
- Import and spread commonProps in component prop definitions
- Override defaults when needed
- Document any modified behavior
- Maintain prop naming consistency

### Using Composables
- Call composables at setup/composition time
- Handle cleanup for media queries
- Destructure only needed functionality
- Pass required props consistently

### Performance Considerations
- Use shallowRef for media queries
- Cache computed values
- Optimize currency formatting calls
- Handle mobile detection efficiently

### Common Pitfalls
- Missing prop validations
- Incorrect currency decimals
- Unhandled media query cleanup
- Inconsistent prop usage
- Missing mobile breakpoints

## Usage Examples

### Basic Props Usage
```javascript
import { commonProps } from '@/config/common';

export default {
  props: {
    ...commonProps,
    customProp: Boolean
  }
};
```

### Composable Usage
```javascript
import { useCommon } from '@/config/common';

export default {
  setup(props) {
    const { formatCurrency, isMobile } = useCommon(props);
    
    return {
      formattedPrice: computed(() => formatCurrency(props.price)),
      isMobile
    };
  }
};
```

### Media Query Usage
```javascript
import { useMediaQuery } from '@/config/common';

export default {
  setup() {
    const isTablet = useMediaQuery('(max-width: 1024px)');
    
    return {
      isTablet
    };
  }
};
``` 