# Bundles

Renders bundle cards.

## Quick usage

```vue
<codex-bundles></codex-bundles>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| hideIfNoResults | Hide this component if there are no results to show | `Boolean` | `false` | - |
| hideDisabledBundles | Hide bundles if they are set as disabled | `Boolean` | `false` | - |
| contentWrapperType | The element / component to wrap bundle cards in. Note: At the moment the only component supported is `swiper` | `String` | `'div'` | - |
| contentWrapperSettings | Settings to pass through to the content wrapper type. See [contentWrapperSettings](./shared/ContentWrapperSettings) for more information | `Object` | `{}` | - |



## Slots

**error-messages**

```html
<codex-bundles>
	<template v-slot:error-messages="{ errors }"></template>
</codex-bundles>
```

**loading-message**

```html
<codex-bundles>
	<template v-slot:loading-message></template>
</codex-bundles>
```

**header**

Rendered inside of `cdx_panel`
```html
<codex-bundles>
	<template v-slot:header></template>
</codex-bundles>
```

**no-results**   

Rendered inside of `cdx_noresults`
```html
<codex-bundles>
	<template v-slot:no-results></template>
</codex-bundles>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-bundles
`

