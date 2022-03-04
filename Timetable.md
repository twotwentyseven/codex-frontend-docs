# Timetable

Renders a timetable of events.

## Quick usage

```vue
<codex-timetable></codex-timetable>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| eventDetailUrl | The slug used for the event detail page | `String` | '/pages/class' | - |
| eventPurchaseUrl | The slug use to redirector users to a purchasing page | `String` | '/pages/buy' | - |
| minDaysToLoad | The minimum number of days to load. The actual number loaded depends on carousel settings supplied | `Number` | `7` | - |
| limitEvents | Limit the number of events to show. Useful when not displayed in a carousel | `Number` or `Boolean` | `false` | - |
| startDate | The first date to check for events | `String` | '' | - |
| showFromStartOfDay | Show all events in a day, even if they are in the past | `Boolean` | `true` | - |
| showBookmarksOnly | Only show bookmarked events | `Boolean` | `false` | - |
| hideFilters | Hide `codex-filters`. Default filters can still be used but not changed. | `Boolean` | `false` | - |
| showCalendarNav | Show the quick date selector | `Boolean` | `true` | - |
| updateNavPositionOnClick | Update the calendar nav to a relevant date when navigating via the main carousel | `Boolean` | `true` | - |
| showDateHeader | Show the date above events grouped on the same day | `Boolean` | `true` | - |
| showDateOnEventCard | Show the event date on timetable event cards | `Boolean` | `false` | - |
| showElapsedEvents | Show events that have already finished | `Boolean` | `false` | - |
| showEventAsModal | Show the event details in a modal when clicked | `Boolean` | `true` | - |
| portalSelector | Where to attach the event modal when shown, must be a valid selector | `Boolean` | 'body' | - |
| mainCarouselSettings | Main timetable settings. See [Swiper's documentation](https://swiperjs.com/vue) for supported properties | `Object` | - | - |
| navCarouselSettings | Calendar Nav settings. See [Swiper's documentation](https://swiperjs.com/vue) for supported properties | `Object` | - | - |
| timetableWrapper | The type of carousel to use. Only Swiper.js supported for now | `String` | 'swiper' | - |
| filters | The filter configuration object. See [Configuring Filters](shared/FilterConfiguration.md) for more documentation | `Object` | - | - |

Also see [common props](./shared/CommonProps.md) for descriptions of other available props.



## Slots

**error-messages**

```html
<codex-timetable>
	<template v-slot:error-messages="{ errors }"></template>
</codex-timetable>
```

**header**

```html
<codex-timetable>
	<template v-slot:header></template>
</codex-timetable>
```

**time-filters**

```html
<codex-timetable>
	<template v-slot:time-filters="{ handleTimeFilter, selectedTimeSlot }"></template>
</codex-timetable>
```

**timetable-nav**

```html
<codex-timetable>
	<template v-slot:timetable-nav="{ navCarouselSettings, handleDateSelection, setNavCarousel, handleActiveDateIndexChange }">
	</template>
</codex-timetable>
```

**event-card**

```html
<codex-timetable>
	<template v-slot:event-card="{ event, customer, showEventDate, isBookmarked, toggleBookmark, bookmarkIdentifier, goToEvent }">
	</template>
</codex-timetable>
```

**event-modal**

```html
<codex-timetable>
	<template v-slot:event-modal="{ id, eventObject, purchaseUrl, isModal, closeModal }">
	</template>
</codex-timetable>
```

## URL Parameters

Any key/value, where the key matches a key supplied in the `filters` prop. For example, with a filters config that looks like the following:

```vue
<codex-timetable :filters="{
	location: {
		selector: "studio.location",
		multi: true
	}
}"></codex-timetable>
```

To pass a default filter value to location, append the following query to the URL:

`?location=1`

Or to select multiple default values you can do the following:

`?location=1&location=4`

## Override

`
codex-template-timetable
`

