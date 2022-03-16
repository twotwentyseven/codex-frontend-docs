# Plans

Renders plan cards.

## Quick usage

```vue
<codex-plans></codex-plans>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| hideIfNoResults | Hide this component if there are no results to show | `Boolean` | `false` | - |
| hideDisabledBundles | Hide bundles if they are set as disabled | `Boolean` | `false` | - |
| variableStartDate | Allow plans to start on an alternative date, passed on to codex-plan-card | `Boolean` | `false` | - |


> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

## Slots

**error-messages**

```html
<codex-plans>
	<template v-slot:error-messages="{ errors }"></template>
</codex-plans>
```

**header**

Rendered inside of `cdx_panel`
```html
<codex-plans>
	<template v-slot:header></template>
</codex-plans>
```

**no-results**   

Rendered inside of `cdx_noresults`
```html
<codex-plans>
	<template v-slot:no-results></template>
</codex-plans>
```

**plan-card**   

```html
<codex-plans>
	<template v-slot:plan-card></template>
</codex-plans>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-plans
`

