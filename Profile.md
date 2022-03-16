# Plans

Renders a profile update form.

## Quick usage

```vue
<codex-profile></codex-profile>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| intro | Hide this component if there are no results to show | `Boolean` | `false` | - |
| showLabels | Show input labels | `Boolean` | `true` | - |


> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

## Slots

**error-messages**

```html
<codex-profile>
	<template v-slot:error-messages="{ errors }"></template>
</codex-profile>
```

**logged-out**

```html
<codex-profile>
	<template v-slot:logged-out></template>
</codex-profile>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-profile
`

