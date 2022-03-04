# Bookings

Renders booking-cards for future or past customer bookings.

## Quick usage

```vue
<codex-bookings></codex-bookings>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| eventDetailUrl | The slug used for the event's detail page | `String` | '/pages/class/{ id }' | - |
| videoDetailUrl | The slug used for the event's video | `String` | '/pages/video/{ id }' | - |
| perPage | The number of bookings to show per page (results will be paginated if there are more than this number) | `Number` | `10` | - |
| type | Whether to show upcoming bookings or previous bookings | `String` | 'upcoming' | Either 'upcoming' or 'previous' |
| hideIfNoResults | Hide this component if there are no results to show | `Boolean` | `false` | - |

> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

## Slots

**error-messages**

```html
<codex-bookings>
	<template v-slot:error-messages="{ errors }"></template>
</codex-bookings>
```

**loading-message**

```html
<codex-bookings>
	<template v-slot:loading-message></template>
</codex-bookings>
```

**header**

Rendered inside of `cdx_title`
```html
<codex-bookings>
	<template v-slot:header="{ type }"></template>
</codex-bookings>
```

**no-results**   

Rendered inside of `cdx_noresults`
```html
<codex-bookings>
	<template v-slot:no-results="{ type }"></template>
</codex-bookings>
```

**booking-cards**   

Rendered inside of `cdx_list`
```html
<codex-bookings>
	<template v-slot:booking-cards="{ bookings, handleViewEvent, handleCancel, handleWatch, handleJoinZoom }"></template>
</codex-bookings>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-bookings
`

