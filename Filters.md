# Filters

Renders the filtering UI. Intended for use within larger components with filterable resources.

## Quick usage

The below is an example of using this component within `<codex-videos>`

```vue
<codex-filters
	:filters="hydratedFilters"
	:handleFilterSelection="handleFilterSelection"
	:handleRangeSelection="handleRangeSelection"
	:filtersActive="isFiltered"
	:activeOptions="activeOptions"
	:removeSelection="removeSelection"
	:removeAllFilters="removeAllFilters"
	:toggleBookmarkedFilter="toggleBookmarkedFilter"
	:onlyShowBookmarked="bookmarksOnly"
	showBookmarkToggle
>
</codex-filters>
```

## Props

**Required**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| showBookmarkToggle | Show/hide the bookmark toggle | `Boolean` | true | - |
| onlyShowBookmarked | Only show bookmarked resources | `Boolean` | false | - |



**Required - useFilters**

The following props are all returned from [useFilters](../composition/useFilters) which is invoked inside of a parent's `setup` function. They all need to be passed through to `<codex-filters>`

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| filters | The filters object. Includes hydrated options. | `String` | - | - |
| handleFilterSelection | A function to update filter selections | `Function` | - | - |
| handleRangeSelection | A function to update range selections | `Function` | - | - |
| filtersActive | Whether any filters are currently active | `Boolean` | - | - |
| activeOptions | An array of the currently active filters, including their `key`, `value`, `displayValue` and `index` | `Array` | - | - |
| removeSelection | A function to remove a selected filter | `Function` | - | - |
| removeAllFilters | A function to remove all selected filters | `Function` | - | - |
| filters | The hydrated filters object. This is returned from [useFilters](../composition/useFilters) | `String` | - | - |


> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

## Slots

**error-messages**

```html
<codex-instructor>
	<template v-slot:error-messages="{ errors }"></template>
</codex-instructor>
```

**loading-message**

```html
<codex-instructor>
	<template v-slot:loading-message></template>
</codex-instructor>
```

**container-start**   

Rendered before the main content
```html
<codex-instructor>
	<template v-slot:container-start="{ instructor }"></template>
</codex-instructor>
```

**container-end**   

Rendered after the main content
```html
<codex-instructor>
	<template v-slot:container-end="{ instructor }"></template>
</codex-instructor>
```

**wrapper-start**   

Rendered near the top of `cdx_content`
```html
<codex-instructor>
	<template v-slot:wrapper-start="{ instructor }"></template>
</codex-instructor>
```
**wrapper-end**   

Rendered near the bottom of `cdx_content`
```html
<codex-instructor>
	<template v-slot:wrapper-end="{ instructor }"></template>
</codex-instructor>
```


## URL parameters

The instructor handle can be passed in as the last segment of the URL path instead of as a prop, e.g:

`/pages/instructor/eric-sheforgen`

## Events

There are no events emitted by this component.

## Override

`
codex-template-instructor
`

