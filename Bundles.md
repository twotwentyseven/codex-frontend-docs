# Bundles

Shows a list of bundles available to the customer.  Clicking a bundle adds to cart.

## Quick usage

```html
<bundles></bundles>
```

## Properties

| Name | Description | Options | Default |
|------|-------------|---------|---------|
| filter-bundles | Only show matching bundles | A comma separated list of bundle handles | None (Show all) |
| filter-bundle-types | Only show bundles with matching bundle types | A comma separated list of bundle type handles | None (Show all) |
| :hide-disabled-bundles | Hide bundles that cannot be purchased instead of disabling them | true, false | false |
| :hide-if-no-results | Hide entire component if no bundles are available | true, false | false |


## Slots

This component has one default slot that is added just before the credit bundles are listed.

## URL parameters

None

## Events

This component emits a `codex` event to the `document` once all bundles have been loaded from the API, with a payload of:

```
{ detail: 'bundles', action: 'loaded' }
```
