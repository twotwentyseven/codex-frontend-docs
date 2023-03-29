# GiftCards

Renders a list of Gift Cards available to purchase.  A select field will be shown if a Gift Card is available in multiple denominations.  Clicking on a Gift Card will add it  to the cart.

## Quick usage

```vue
<codex-gift-cards></codex-gift-cards>
```

## Props

**Optional**

| Name | Description                                                    | Type      | Default | Validation |
|------|----------------------------------------------------------------|-----------|---------|---------|
| hideIfNoResults | Suppress rendering of the entire component if no results found | `Boolean` | false   | - |

Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

Also see [filter props](./shared/FilterProps.md) for descriptions of other available props.


## Slots

**error-messages**

```html
<codex-gift-cards>
	<template v-slot:error-messages="{ errors }"></template>
</codex-gift-cards>
```

## Override

`
codex-template-gift-cards
`
