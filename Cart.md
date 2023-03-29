# Cart

Renders a cart and all the controls necessary to manage it.  Currently only used as a child component of [Checkout](Checkout.md)

## Quick usage

```vue
<codex-cart :cart="123"></codex-cart>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| id | Event ID, used to load full event details | `String` or `Number` | - | - |
| eventObject | An object containing full or partial event details, usually passed through via the timetable | `Object` | - | - |
| isModal | Whether or not the component is displayed in modal form | `Boolean` | `false` | - |
| closeModal | A handler function to close the modal if `isModal` is true  | `Function` | - | - |
| autoBook | Whether to auto-book customers onto events | `Boolean` | `true` | - |
| eventUrl | The URL pathname. Used to check for an event ID supplied as part of the URL | `String` | '/pages/event' | - |
| accountUrl | The URL used to manage user bookings | `String` | '/pages/account' | - |
| purchaseUrl | The URL used to buy credits / plans | `String` | '/pages/buy' | - |
| videoUrl | The URL used to direct users towards an event stream | `String` | '/pages/video' | - |
| slotSize | A unitless value that determines how layout positions are calculated relative to the backend grid system | `Number` | `50` | - |
| minSlotSize | A minimum slot size (pixels) to prevent layouts from becoming too small on small devices | `Number` | `50` | - |
| showWhenLoggoutOut | Whether to show the event to logged out users | `Boolean` | `false` | - |

Also see [common props](./shared/CommonProps.md) for descriptions of other available props.


## Slots

**error-messages**

```html
<codex-event>
	<template v-slot:error-messages="{ errors }"></template>
</codex-event>
```

**loading-message**

```html
<codex-event>
	<template v-slot:loading-message></template>
</codex-event>
```

**image**

Rendered inside of `cdx_event-photo-wrapper`
```html
<codex-event>
	<template v-slot:image="{ event }"></template>
</codex-event>
```

**wrapper-start**

Rendered near the top of `cdx_content`
```html
<codex-event>
	<template v-slot:wrapper-start="{ event }"></template>
</codex-event>
```
**wrapper-end**

Rendered near the bottom of `cdx_content`
```html
<codex-event>
	<template v-slot:wrapper-end="{ event }"></template>
</codex-event>
```

**container-start**

Rendered near the top of `cdx_event-inner`
```html
<codex-event>
	<template v-slot:container-start="{ event }"></template>
</codex-event>
```
**container-end**

Rendered near the bottom of `cdx_event-inner`
```html
<codex-event>
	<template v-slot:container-end="{ event }"></template>
</codex-event>
```

**linebreak**

Rendered after event information for use as a flex linebreak
```html
<codex-event>
	<template v-slot:linebreak></template>
</codex-event>
```

**oneclick-cancel-msg**

```html
<codex-event>
	<template v-slot:oneclick-cancel-msg="{ cancellableSlots }"></template>
</codex-event>
```

**booking-error-messages**

```html
<codex-event>
	<template v-slot:booking-error-messages="{ errors }"></template>
</codex-event>
```

**event-full**

Rendered when an event is fully-booked
```html
<codex-event>
	<template v-slot:booking-event-full="{ event }"></template>
</codex-event>
```

**event-unavailable**

Rendered when an event is unavailable to book
```html
<codex-event>
	<template v-slot:booking-event-unavailable></template>
</codex-event>
```

**login**

Rendered when there is no customer logged in
```html
<codex-event>
	<template v-slot:login="{ event }"></template>
</codex-event>
```




## URL Parameters

The event id can be passed as a query parameter as follows:

`/pages/event?id=123`

or as part of the pathname

`/pages/event/123`

## Override

`
codex-template-event
`
