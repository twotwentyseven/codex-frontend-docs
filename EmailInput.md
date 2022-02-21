# EmailInput

Renders an input of type `email`.

## Quick usage

```vue
<codex-email-input id="Email" v-model="email"></codex-email-input>
```

## Props

| Name | Description | Type / Required | Default | Validation |
| - | - | - | - | - |
| id | The id applied to the input | `String` / `true` | - | - |
| placeholder | The input's placeholder text | `String` / `false` | - | - |
| required | Whether the input is required for form submissions | `Boolean` / `false` | `true` | - |
| label | Assigned to aria-label | `String` / `false` | 'Email Address' | - |

## Model

This component requires a `v-model` binding of type `String`.

## Override

`codex-template-email-input`

