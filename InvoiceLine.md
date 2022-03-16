# Invoice

Renders an an invoice line, for use within <codex-invoice></codex-invoice>

## Quick usage

```vue
<codex-invoice-line v-for="line in (invoice ? invoice.order_lines : [])" :line="line"></codex-invoice-line>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| line | The line item object | `Object` | - | - |


## Slots

None

## URL parameters

None

## Events

`view`

## Override

`
codex-template-invoice-line
`

