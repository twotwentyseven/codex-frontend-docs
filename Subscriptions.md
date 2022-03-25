# Subscriptions

Renders a customer's subscriptions.

## Quick usage

```vue
<codex-subscriptions></codex-subscriptions>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| type | What type of subscriptions to load. Any value other than `'active'` will load ended subscriptions | `Boolean` | `'active'` | - |
| cancellationReasonRequired | Whether a reason is required prior to cancellation | `Boolean` | `false` | - |
| noun | How to refer to subscriptions in the UI | `String` | `'subscription'` | - |


> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

## Slots


**loading-message**

```html
<codex-subscriptions>
	<template v-slot:loading-message></template>
</codex-subscriptions>
```

**error-messages**

```html
<codex-subscriptions>
	<template v-slot:error-messages="{ errors }"></template>
</codex-subscriptions>
```

**header**

Rendered inside of `cdx_panel`
```html
<codex-subscriptions>
	<template v-slot:header></template>
</codex-subscriptions>
```

**no-results**   

Rendered inside of `cdx_noresults`
```html
<codex-subscriptions>
	<template v-slot:no-results></template>
</codex-subscriptions>
```

**subscription-card**   

```html
<codex-subscriptions>
	<template v-slot:subscription-card></template>
</codex-subscriptions>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-subscriptions
`

