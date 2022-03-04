# Bundles

Renders credit cards containing credit information.

## Quick usage

```vue
<codex-credits></codex-credits>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| perPage | The number of credits to show per page (results will be paginated if there are more than this number) | `Number` | `10` | - |
| type | Whether to show unused credits or used credits | `String` | 'unused' | Either 'unused' or 'used' |
| hideIfNoResults | Hide this component if there are no results to show | `Boolean` | `false` | - |

> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

## Slots

**error-messages**

```html
<codex-credits>
	<template v-slot:error-messages="{ errors }"></template>
</codex-credits>
```

**loading-message**

```html
<codex-credits>
	<template v-slot:loading-message></template>
</codex-credits>
```

**header**

Rendered inside of `cdx_title`
```html
<codex-credits>
	<template v-slot:header="{ type }"></template>
</codex-credits>
```

**no-results**   

Rendered inside of `cdx_noresults`
```html
<codex-credits>
	<template v-slot:no-results="{ type }"></template>
</codex-credits>
```

**credits-cards**   

Rendered inside of `cdx_list`
```html
<codex-credits>
	<template v-slot:booking-cards="{ credits, type }"></template>
</codex-credits>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-credits
`

