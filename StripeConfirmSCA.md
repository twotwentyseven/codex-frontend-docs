# StripeConfirmSCA

Renders a component that allows a customer to complete 3dSecure 2 (SCA) authorisation for a PaymentIntent (for example if FOH make a purchase in the admin area using a customer's saved card).

This component needs to be placed on a page with a URL that matches the one set in the `Frontend SCA URL` value in System config.  Links to this page are automatically generated when the system notifies the customer that they need to confirm a payment and you won't typically need to link to this page yourself.


## Quick usage

```vue
<codex-stripe-confirm-sca></codex-stripe-confirm-sca>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| nextPageUrl | The URL of the page to link the customer to after authentication | `String` | `/account` | - |
| nextPageText | The text shown on the post authentication button | `String` | - | - |

Also see [common props](./shared/CommonProps.md) for descriptions of other available props.


## Slots

**error-messages**

```html
<codex-bookings>
	<template v-slot:error-messages="{ errors }"></template>
</codex-bookings>
```


## URL Parameters

**Payment Intent ID**

A Stripe PaymentIntent ID can be passed as a query parameter as follows:

`/pages/checkout?id=pi_abcdef123456ghijkl`

The ID of the payment intent that requires user authentication

## Override

`
codex-template-confirm-sca
`