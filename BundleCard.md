# BundleCard

Renders an individual bundle card, displaying bundle information. Intended for use inside of `<codex-bundles>`

## Quick usage

```vue
<codex-bundle-card v-for="bundle in filteredResource" :bundle="bundle" :purchasedBundles="purchasedBundles" :hideIfDisabled="hideDisabledBundles" :key="bundle.id"></codex-bundle-card>
```

## Props

**Required**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| bundle | The bundle object | `Object` | - | - |

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| purchasedBundles | A customer's purchased bundles | `Array` | - | - |
| hideIfDisabled | Hide bundle if it is set as disabled | `Boolean` | `false` | - |


## Slots

None

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-bundle-card
`

