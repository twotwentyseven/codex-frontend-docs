# Instructors

Renders instructor cards and filters.

## Quick usage

```vue
<codex-instructors instructor-detail-url="/pages/instructor/{ id }"></codex-instructors>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| instructorDetailUrl | The slug used for the instructor's detail page | `String` | '/pages/class/{ id }' | - |
| showTags | The tags to display within instructor cards | `Array` | `[]` | - |
| onlyShowBookmarked | Only show instructors that the customer has bookmarked | `String` | 'upcoming' | Either 'upcoming' or 'previous' |
| hideFilters | Hide `<codex-filters>`. Default filters can still be used but not updated. | `Boolean` | `false` | - |

> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

## Slots

**error-messages**

```html
<codex-instructors>
	<template v-slot:error-messages="{ errors }"></template>
</codex-instructors>
```

**loading-message**

```html
<codex-instructors>
	<template v-slot:loading-message></template>
</codex-instructors>
```

**no-results**   

Rendered inside of `cdx_noresults`
```html
<codex-instructors>
	<template v-slot:no-results="{ type }"></template>
</codex-instructors>
```

**instructor-cards**   

Rendered inside of `cdx_list`
```html
<codex-instructors>
	<template v-slot:instructor-card="{ filteredResource, showTags, handleInstructorClick }"></template>
</codex-instructors>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-instructors
`

