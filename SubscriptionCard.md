# SubscriptionCard

Renders a subscription cards. For use within `<codex-subscriptions>`.

## Quick usage

```vue
<codex-subscription-card></codex-subscription-card>
```

## Props

**Required**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| subscription | The subscription object | `Object` | - | - |

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| isProcessing | Whether changes to the subscription are being processed | `Boolean` | `false` | - |
| isRecentlyCancelled | Whether the subscription has been recently cancelled | `Boolean` | - | - |
| showAsModel | Whether to show the manage subscription UI as a modal | `String` | `'subscription'` | - |


## Slots

**header**

```html
<codex-subscription-card>
	<template v-slot:header></template>
</codex-subscription-card>
```

**manage-header**   

```html
<codex-subscription-card>
	<template v-slot:manage-header></template>
</codex-subscription-card>
```

**manage-description**   

```html
<codex-subscription-card>
	<template v-slot:manage-description></template>
</codex-subscription-card>

**manage-footer**   

```html
<codex-subscription-card>
	<template v-slot:manage-footer></template>
</codex-subscription-card>
```

**manage-cancel**   

```html
<codex-subscription-card>
	<template v-slot:manage-cancel></template>
</codex-subscription-card>
```

**manage-cancelled**   

```html
<codex-subscription-card>
	<template v-slot:manage-cancelled></template>
</codex-subscription-card>

**manage-pause**   

```html
<codex-subscription-card>
	<template v-slot:manage-pause></template>
</codex-subscription-card>

**manage-paused**   

```html
<codex-subscription-card>
	<template v-slot:manage-paused></template>
</codex-subscription-card>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-subscriptions
`

