# BookingCard

Renders an event card with limited event details. Intended for use within `<codex-bookings>`.

## Quick usage

```vue
<codex-booking-card 
	v-for="booking in bookings"
	:key="booking.id"
	:booking="booking"
	@view-event="handleViewEvent"
	@cancel="handleCancel"
	@watch="handleWatch"
	@join-zoom="handleJoinZoom">
</codex-booking-card>
```

## Props

**Required**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| booking | The booking object | `Object` | - | - |

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| showInstructorPhoto | The customer object | `Boolean` | `true` | - |

## Slots

**content-slot**

Rendered inside of `cdx_content`.

```html
<codex-booking-card>
	<template v-slot:content-slot="{ booking, cancelling, customer, confirmCancel, isRefundable }"></template>
</codex-booking-card>
```

**inner-footer**

Rendered at the bottom of `cdx_inner`.

```html
<codex-booking-card>
	<template v-slot:inner-footer="{ booking, cancelling, customer, confirmCancel, isRefundable }"></template>
</codex-booking-card>
```

## URL parameters

None

## Events

click, cancel, watch, join-zoom, view-event

## Override

`
codex-template-booking-card
`

