# StripeCard

Renders a set of stripe inputs in the form of a card.

## Quick usage

```vue
<codex-stripe-card></codex-stripe-card>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| redirectUrl | Where to redirect after successful checkout | `String` | `''` | - |
| combined | Whether to render stripe elements individually or combine them together | `Boolean` | `true` | - |
| options | An object of key / option pairs. Passed through to StripeElement components | `Object` | `{}` | - |


## Slots

**stripe-payment-successful**

```html
<codex-stripe-card>
	<template v-slot:stripe-payment-successful></template>
</codex-stripe-card>
```

**stripe-new-card-title**

```html
<codex-stripe-card>
	<template v-slot:stripe-new-card-title></template>
</codex-stripe-card>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-card-form
`

