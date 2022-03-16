# Instructors

Renders the instructor detail page.

## Quick usage

```vue
<codex-instructor handle="eric-sheforgen"></codex-instructor>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| handle | The instructor's handle | `String` | - | - |
| showTags | The instructor's tags to display | `Array` | `[]` | - |

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

