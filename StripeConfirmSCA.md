# StripeConfirmSCA

Renders a card-style form to update a stripe card.

## Quick usage

```vue
<codex-stripe-card-update></codex-stripe-card-update>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| paymentIntentId | Passed to loadStripeIntent | `String` | - | - |
| nextPageUrl | The link url once payment has been processed | `/account` | - | - |
| nextPageText | The link text once payment has been processed | `/account` | - | - |

> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.


## Slots

**error-messages**

```html
<codex-stripe-confirm-sca>
	<template v-slot:error-messages></template>
</codex-stripe-confirm-sca>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-card-form
`

