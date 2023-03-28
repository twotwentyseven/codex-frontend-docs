# LoginRegister

Toggle between login, register and recover password

## Quick usage

```html
<codex-login-register></codex-login-register>
```

## Props

| Name | Description | Type / Required | Default | Validation |
|- | - | - | - | - |
| login_title | The title for login | `String`/`false` | 'Login' | none |
| login_overview | The subtitle/overview for login | `String`/`false` | '' | none |
| recover_title | The title for recover password | `String`/`false` | 'Reset Password' | none |
| recover_overview | The subtitle/overview for recover password | `String`/`false` | '' | none |
| showLoginDefault | Show Login component by default | `Boolean` | `false` | none |
| showRegisterDefault | Show Register component by default | `Boolean` | `false` | none |
| showPasswordResetDefault | Show Password Reset component by default | `Boolean` | `false` | none |
| showLabels | Show input labels | `Boolean` | `false` | none |
| accountUrl | Account Url show on successful register and login | `String` | `/pages/account` | none |
| purchaseUrl | Purchase/Buy Url show on successful register and login | `String` | `/pages/buy` | none |
| bookingUrl | Book/Timetable Url show on successful register and login | `String` | `/pages/timetable` | none |

> Note: show#Component#Default props can all be false blank and the default will show a login and regsiter button

## Slots  

None

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

```
codex-template-login-register
```

