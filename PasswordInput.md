# PasswordInput

Renders an input of type `password`.

## Quick usage

```vue
<codex-password-input id="Password" v-model="password"></codex-password-input>
```

## Props

| Name | Description | Type / Required | Default | Validation |
| - | - | - | - | - |
| id | The id applied to the input | `String` / `true` | - | - |
| placeholder | The input's placeholder text | `String` / `false` | - | - |
| required | Whether the input is required for form submissions | `Boolean` / `false` | `true` | - |
| label | Assigned to aria-label | `String` / `false` | 'Password' | - |

## Model

This component requires a `v-model` binding of type `String`.


## Override

`codex-template-password-input`

