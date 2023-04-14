# Bookings Completed

Renders booking-cards for all customer bookings that have been attended/completed.

## Quick usage

```vue
<codex-bookings-completed></codex-bookings-completed>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| hideIfNoResults | Hide this component if there are no results to show | `Boolean` | `false` | - |

> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

## Slots

**error-messages**

```html
<codex-bookings-completed>
	<template v-slot:error-messages="{ errors }"></template>
</codex-bookings-completed>
```

**loading-message**

```html
<codex-bookings-completed>
	<template v-slot:loading-message></template>
</codex-bookings-completed>
```

**header**

Rendered inside of `cdx_title`
```html
<codex-bookings-completed>
	<template v-slot:header="{ type }"></template>
</codex-bookings-completed>
```

**no-results**   

Rendered inside of `cdx_noresults`
```html
<codex-bookings-completed>
	<template v-slot:no-results="{ type }"></template>
</codex-bookings-completed>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-bookings-completed
`

