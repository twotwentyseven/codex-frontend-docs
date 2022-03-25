# Transactions

Renders a customer's transactions.

## Quick usage

```vue
<codex-transactions></codex-transactions>
```

## Props

> Note: See [common props](./shared/CommonProps.md) for descriptions of other available props.

## Slots


**loading-message**

```html
<codex-transactions>
	<template v-slot:loading-message></template>
</codex-transactions>
```

**error-messages**

```html
<codex-transactions>
	<template v-slot:error-messages="{ errors }"></template>
</codex-transactions>
```

**header**

Rendered inside of `cdx_panel`
```html
<codex-transactions>
	<template v-slot:header></template>
</codex-transactions>
```

**description**

Rendered inside of `cdx_panel`
```html
<codex-transactions>
	<template v-slot:description></template>
</codex-transactions>
```

**no-results**   

Rendered inside of `cdx_noresults`
```html
<codex-transactions>
	<template v-slot:no-results></template>
</codex-transactions>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-subscriptions
`

