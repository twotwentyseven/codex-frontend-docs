# InstructorCard

Renders a multi-stage component enabling non-customers to register, purchase a plan / credits and then view the timetable.

## Quick usage

```vue
<codex-first-timers></codex-first-timers>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| titles | The stage titles to be displayed in the progress header | `Object` | `{ 1: 'Register', 2: 'Membership / Credits', 3: 'Book' }` | - |
| headingWrapperClass | The class name to apply to the progress heading section | `String` | - | - |
| stageWrapperClass | The class name to apply to the stages section | `String` | - | - |

## Slots

**heading-stages**

```html
<codex-first-timers>
	<template v-slot:heading-stages="{ stage, titles }"></template>
</codex-first-timers>
```

### Registration Slots   

**stage-1-header**

```html
<codex-first-timers>
	<template v-slot:stage-1-header="{ customer }"></template>
</codex-first-timers>
```

**stage-1-content**

```html
<codex-first-timers>
	<template v-slot:stage-1-content="{ handleRegisterStarted, moveToNextStage }"></template>
</codex-first-timers>
```

**stage-1-footer**

```html
<codex-first-timers>
	<template v-slot:stage-1-footer="{ customer }"></template>
</codex-first-timers>
```

### Purchasing Slots   

**stage-2-header**

```html
<codex-first-timers>
	<template v-slot:stage-2-header="{ customer }"></template>
</codex-first-timers>
```

**stage-2-content**

```html
<codex-first-timers>
	<template v-slot:stage-2-content="{ plans, bundles, next }"></template>
</codex-first-timers>
```

**stage-2-footer**

```html
<codex-first-timers>
	<template v-slot:stage-2-footer="{ customer }"></template>
</codex-first-timers>
```

### Timetable Slots   

**stage-3-header**

```html
<codex-first-timers>
	<template v-slot:stage-3-header="{ customer }"></template>
</codex-first-timers>
```

**stage-3-content**

```html
<codex-first-timers>
	<template v-slot:stage-3-content></template>
</codex-first-timers>
```

**stage-3-footer**

```html
<codex-first-timers>
	<template v-slot:stage-3-footer="{ customer }"></template>
</codex-first-timers>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-first-timers
`

