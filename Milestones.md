# Milestones

Renders a customer's milestone progress.

## Quick usage

```vue
<codex-milestones :circle-data="{ diameter: 160, strokeWidth: 12 }"></codex-milestones>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| chartType | The type of chart to display. Note, currently only `circle` supported. | `String` | `'circle'` | `'circle' or 'progress_bar'` |
| type | The type of milestone to check against | `String` | `'bookings_attended'` | `'bookings_attended' or 'videos_watched'` |
| steps | The named milestone steps (names can be hidden on the Frontend if desired) | `Object` | `{ bronze: 25, silver: 50, gold: 75, platinum: 100 }` | - |
| completedMsg | A message to display once when a customer has reached the final milestone step | `String` | `'Congratulations, all Milestones completed'` | - |
| circleData | The diameter and stroke width (in pixels) to display (use the largest size specified in your design). The final output will be scaled depending on the wrapper size. | `Object` | `{ diameter: 50, strokeWidth: 20 }` | - |

> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

## Slots

**header**

```html
<codex-first-timers>
	<template v-slot:header></template>
</codex-first-timers>
``` 

**milestone-modal**

Note: key is the milestone step name, and value is the milestone (number) for that step.

```html
<codex-first-timers>
	<template v-slot:milestone-modal="{ key, value }"></template>
</codex-first-timers>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-milestones
`

