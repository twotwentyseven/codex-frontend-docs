# Video

Renders a video card, for use within the `<codex-videos>` component.

## Quick usage

```vue
<codex-video-card></codex-video-card>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| video | The video object | `Object` | `{}` | - |
| durationWatched | How much of the video has already been watched | `Numer` \| `false` |

> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.


## Slots

**error-messages**

```html
<codex-video-card>
	<template v-slot:error-messages="{ errors }"></template>
</codex-video-card>
```

**loading-message**

```html
<codex-video-card>
	<template v-slot:loading-message></template>
</codex-video-card>
```

**container-start**

```html
<codex-video-card>
	<template v-slot:container-start="{ video }"></template>
</codex-video-card>
```

**container-end**

```html
<codex-video-card>
	<template v-slot:container-end="{ video }"></template>
</codex-video-card>
```

**wrapper-start**

```html
<codex-video-card>
	<template v-slot:wrapper-start="{ video }"></template>
</codex-video-card>
```

**wrapper-end**

```html
<codex-video-card>
	<template v-slot:wrapper-end="{ video }"></template>
</codex-video-card>
```


## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-video
`

