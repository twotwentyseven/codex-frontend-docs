# Submit

Renders a button of type `Submit` with the option of enabling a cancellable confirmation check before submitting.

## Quick usage

```vue
<codex-submit :disabled="disabled" :processing="processing"></codex-submit>

<!-- 
	'disabled' and 'processing' must be in the template's scope
-->
```

## Props
**Required Props**
| Name | Description | Type / Required | Default | Validation |
| - | - | - | - | - |
| disabled | Whether the submit button should be disabled | `Boolean` / `false` | `false` | - |
| processing | Whether the parent form is processing | `Boolean` / `false` | `false` | - |
  
---
**Optional Props**

| Name | Description | Type / Required | Default | Validation |
| - | - | - | - | - |
| requireConfirmation | Show the cancel/confirm buttons before submitting | `Boolean` / `false` | `false` | - |
| defaultText | The button's default text | `String` / `false` | 'Submit' | - |
| disabledText | The button's text when disabled | `String` / `false` | 'Submit' | - |
| processingText | The button's text when processing | `String` / `false` | 'Submitting' | - |
| cancelText | The cancel button's text | `String` / `false` | 'Cancel' | - |
| confirmText | The confirm button's text | `String` / `false` | 'Confirm' | - |
| disabledClass | The class name applied when disabled | `String` / `false` | 'cdx_btn-disabled' | - |
| processingClass | The class name applied when in default state | `String` / `false` | 'cdx_btn-processing' | - |
| confirmClass | The class name applied when in default state | `String` / `false` | 'cdx_btn-processing' | - |



## Override

`
codex-template-submit
`

