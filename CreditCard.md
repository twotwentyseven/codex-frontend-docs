# CreditsCard

Renders an event card with limited event details. Intended for use within `<codex-credit-card>`.

## Quick usage

```vue
<codex-credit-card
	v-for="credit in credits"
	:credit="credit"
	:type="type"
	:key="credit.id"
></codex-credit-card>
```

## Props

**Required**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| credit | The credit object | `Object` | - | - |

## Slots

**content-slot**

None

## URL parameters

None

## Events

None

## Override

`
codex-template-credit-card
`

