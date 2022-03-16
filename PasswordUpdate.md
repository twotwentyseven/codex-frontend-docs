# PasswordUpdate

Renders a customer password update form.

## Quick usage

```vue
<codex-password-update></codex-password-update>
```

## Props

None

## Slots

**error-messages**

```html
<codex-password-update>
	<template v-slot:error-messages="{ errors }"></template>
</codex-password-update>
```

**updated**

```html
<codex-password-update>
	<template v-slot:updated></template>
</codex-password-update>
```

**title**

```html
<codex-password-update>
	<template v-slot:title></template>
</codex-password-update>
```

**default**

Rendered after the form has been submitted

```html
<codex-password-update>
	<template v-slot:default></template>
</codex-password-update>
```

**login**

Rendered if the customer isn't logged in

```html
<codex-password-update>
	<template v-slot:default></template>
</codex-password-update>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-password-update
`