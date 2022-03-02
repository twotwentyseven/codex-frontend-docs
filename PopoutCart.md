# PopoutCart

Renders a cart with built-in open/close functionality.

## Quick usage

```vue
<codex-popout-cart purchase-url="{{ settings.codex-pricing-url }}" bundle-url="{{ settings.codex-pricing-url }}" plan-url="{{ settings.codex-pricing-url }}">
</codex-popout-cart>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| bundleUrl | The URL to direct users to purchase bundles | `String` | '/pages/purchase' | - |
| planUrl | The URL to direct users to purchase plans | `String` | '/pages/purchase' | - |
| stripeOptions | Stripe Options | `Object` | `{}` | - |
| stripeNumberOptions | Stripe Number Options | `Object` | `{}` | - |
| stripeExpiryOptions | Stripe Expiry Options | `Object` | `{}` | - |
| stripeCvcOptions | Stripe CVC Options | `Object` | `{}` | - |
| autoOpenWhenItemsAdded | Open the cart when an item is added | `Boolean` | `true` | - |


## Slots

**summary-header**

```html
<codex-popoutcart>
	<template v-slot:summary-header></template>
</codex-popoutcart>
```

**checkout-header**

```html
<codex-popoutcart>
	<template v-slot:checkout-header></template>
</codex-popoutcart>
```

**empty-cart**

```html
<codex-popoutcart>
	<template v-slot:empty-cart></template>
</codex-popoutcart>
```

**login-slot**

```html
<codex-popoutcart>
	<template v-slot:login-slot></template>
</codex-popoutcart>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

None

