# Video

Renders a video detail page.

## Quick usage

```vue
<codex-video></codex-video>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| videoId | The ID of the video to load | `String` | - | - |
| autoplay | Whether to start playing the video on load | `Boolean` | - | - |
| playerReportingInterval | How often to log the view | `Number` | 30 | - |
| purchaseUrl | The URL used to buy credits / plans | `String` | `'/pages/memberships'` | - |
| showCountdown | Show a countdown timer if the event is in the future | `Boolean` | `true` | - |
| use-native-controls | Uses teh vanilla Vimeo player instead of the styled up CodexFit video player | `Boolean` | `false` | - |
| color | The video player accent color | `String` | `'#6AC7F9'` | - |

> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.


## Slots

**error-messages**

```html
<codex-video>
	<template v-slot:error-messages="{ errors }"></template>
</codex-video>
```

**loading-message**

```html
<codex-video>
	<template v-slot:loading-message></template>
</codex-video>
```

**container-start**

```html
<codex-video>
	<template v-slot:container-start="{ video }"></template>
</codex-video>
```

**container-end**

```html
<codex-video>
	<template v-slot:container-end="{ video }"></template>
</codex-video>
```

**wrapper-start**

```html
<codex-video>
	<template v-slot:wrapper-start="{ video }"></template>
</codex-video>
```

**wrapper-end**

```html
<codex-video>
	<template v-slot:wrapper-end="{ video }"></template>
</codex-video>
```


## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-video
`

