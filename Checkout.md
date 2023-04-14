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
| announcementBar | Adds an annoucement bar to the cart example use would be to navigate to physical products that use an alternative cart to checkout - passed through to cart | `String` | -       | - |
| announcementUrl | href added to the annoucement bar - passed through to cart | `String` | -       | - |

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

## CSS Variables

    --codex-cart-min-width: 32.5rem;
    --codex-cart-max-width: 32.5rem;

    --codex-cart-header-font: "Montserrat", sans-serif;
    --codex-cart-header-font-family: "Montserrat", sans-serif;
    --codex-cart-body-font: "Montserrat", sans-serif;
    --codex-cart-body-font-family: "Montserrat", sans-serif;

    --codex-cart-background-color: 255 255 255;
    --codex-cart-border-color: 48 48 48;
    --codex-cart-border-width: 1px;
    --codex-cart-overlay-gradient: 0.4;
    --codex-cart-text-primary: 0 0 0;
    --codex-cart-text-secondary: 0 0 0;
    --codex-cart-text-tertiary: 0 0 0;
    --codex-cart-line-color: 0 0 0;
    --codex-cart-btn-primary:  48 48 48;
    --codex-cart-btn-primary-border:  48 48 48;
    --codex-cart-btn-primary-text-color: 255 255 255;
    --codex-cart-btn-secondary: transparent;
    --codex-cart-btn-secondary-border: 48 48 48;
    --codex-cart-btn-secondary-text-color: 48 48 48;
    --codex-cart-link-color: 48 48 48;
    --codex-cart-close-btn: 48 48 48;

    
    --codex-cart-voucher-background: 48 48 48;
    --codex-cart-voucher-text-color: 255 255 255;

    
    --codex-cart-announcement-background: 200 200 200;
    --codex-cart-announcement-text-color: 0 0 0; 
    
    --codex-cart-remove-color: 241 38 49;
    --codex-cart-item-header-font-size: 0.875rem;
    --codex-cart-item-header-line-height: 1.25rem;

    --codex-cart-item-font-size: 0.75rem;
    --codex-cart-item-line-height: 1rem;

    
    --codex-cart-summary-background: 255 0 0;
    --codex-cart-summary-border: 0 0 0;
    --codex-cart-summary-text-color: 0 0 0;

    --codex-cart-loader-color: 48 48 48;

    --success: 48 48 48;
    --success-inverted: rgb(48 48 48);

    --stroke-color: #000;

## Override

`
codex-template-checkout
`