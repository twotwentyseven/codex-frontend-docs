# Checkout

Renders a checkout component that contains a child component for either a cart or an order.

The cart component includes cart controls, login/registration, and checkout controls necessary to manage it.

The order component automatically checks and displays payment status and allows selection of payment method in the case of failures.

This is one of 2 components that are necessary to enable customers to purchase through your 

## Quick usage

```vue
<codex-checkout></codex-checkout>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
|------|-------------|------|---------|---------|
| announcementBar | Unknown - passed through to cart | `String` | -       | - |
| announcementUrl | Unknown - passed through to cart | `String` | -       | - |

Also see [common props](./shared/CommonProps.md) for descriptions of other available props.


## Slots

**empty-cart**

```html
<codex-checkout>
	<template v-slot:empty-cart></template>
</codex-checkout>
```

**cart-success-cta**

```html
<codex-checkout>
	<template v-slot:cart-success-cta></template>
</codex-checkout>
```



## URL Parameters

**Order ID**

An order ID can be passed as a query parameter as follows:

`/pages/checkout?order_id=123`

This will cause the component to display the order details including payment status

**Payment Intent ID**

A Stripe PaymentIntent ID can be passed as a query parameter as follows:

`/pages/checkout?payment_intent=pi_abcdef123456ghijkl`

This will cause the component to load the order associated with the specified Payment Intent and display the details including payment status

## Override

`
codex-template-checkout
`
