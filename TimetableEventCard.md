# TimetableEventCard

Renders an event card with limited event details. For use within `<codex-timetable>`.

## Quick usage

```vue
<codex-timetable-event-card
	:event="event"
	:customer="customer"
	:showEventDate="showDateOnEventCard"
	:isBookmarked="isBookmarked"
	:toggleBookmark="toggleBookmark"
	:bookmarkIdentifier="generateBookmarkIdentifier(event)"	:goToEvent="handleGoToEvent(event)"
>
</codex-timetable-event-card>
```

## Props

**Required**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| event | The event object | `Object` | - | - |
| customer | The customer object | `Object` | - | - |
| isBookmarked | A function to determine whether an event is bookmarked | `Function` | - | - |
| toggleBookmark | A function to toggle bookmarked status  | `Function` | - | - |
| bookmarkIdentifier | An unique bookmark identifier  | `String` | -| - |
| goToEvent | A function to open the event modal or redirect to the event detail page | `Function` | - | - |

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| showEventDate | Show the event date | `Boolean` | `false` | - |


## Override

`
codex-template-timetable-event-card
`

