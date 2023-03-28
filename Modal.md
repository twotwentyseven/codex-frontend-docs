# Plans

Renders Modal component.

## Quick usage

```vue
<codex-modal>
    // Insert component
</codex-modal>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| portalSelector | Select what element to attach modal to | `String` | `body` | - |
| showCloseButton | Disable or Enable the close button, for example the sms-verify component would disable the close button | `Boolean` | `true` | - |
| useOverlay | Add an overlay behind the modal | `Boolean` | `true` | - |
| modalName | Used to trigger the modal | `String` | `default` | - |
| delayClose | On a successfully emited such as register or login the modal will close | `[Number, Boolean]` | `false` | `false` |
| fullScreen | width = 100% or width = auto | `Boolean` | `false` | - |
| fullScreenHeight | height = 100% or height = auto | `Boolean` | `false` | - |
| xPosition | X position of the modal accepts values of ['left', 'right', 'center'] | `String` | `center` | - |
| yPosition | Y position of the modal accepts values of ['top', 'bottom', 'center'] | `String` | `center` | - |
| transition | Vue transition, custom css will be require for transitions | `String` | `fade` | - |


## Slots

**default**

Rendered inside of `slot`
```html
<codex-modal>
    <codex-login-register></codex-login-register>
</codex-modal>
```

## URL parameters

None

## Override

`
codex-template-modal
`

