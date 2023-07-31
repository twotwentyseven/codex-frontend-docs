# PlanCard

Renders an individual plan card, displaying plan information. Intended for use inside of `<codex-Plans>`

## Quick usage

```vue
<codex-plan-card v-for="plan in filteredResource" :plan="plan" :purchasedPlans="purchasedPlans" :hideIfDisabled="hideDisabledPlans" :key="plan.id"></codex-plan-card>
```

## Props

**Required**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| plan | The plan object | `Object` | - | - |

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| priceInButton | Show the plan price inside the add to cart button | `Boolean` | `false` | - |
| hideIfDisabled | Hide plan if it is set as disabled | `Boolean` | `false` | - |
| variableStartDate | Allow plans to start on an alternative date | `Boolean` | `false` | - |
| daysOfWeekToShow | Allows selective day of week when using variableStartDate. Supply as an array of days of week (starting Sunday at 0, finishing Saturday at 6) | `Array` | `[0,1,2,3,4,5,6]` | - |


## Slots

```html
<codex-plan-card>
	<template v-slot:footer="{ add, formatCurrency, canPurchase, isInCart, startDates, startDateRequired }">
</codex-plan-card>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-plan-card
`

