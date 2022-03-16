# PasswordReset

Renders a customer password reset form.

## Quick usage

```vue
<codex-password-reset></codex-password-reset>
```

## Props

None

## Slots

**error-messages**

```html
<codex-password-reset>
	<template v-slot:error-messages="{ errors }"></template>
</codex-password-reset>
```

**updated**

```html
<codex-password-reset>
	<template v-slot:updated></template>
</codex-password-reset>
```

**title**

```html
<codex-password-reset>
	<template v-slot:title></template>
</codex-password-reset>
```

**message**

Rendered before the form

```html
<codex-password-reset>
	<template v-slot:message></template>
</codex-password-reset>
```

**end-content**

Rendered after the form

```html
<codex-password-reset>
	<template v-slot:end-content></template>
</codex-password-reset>
```

**default**

Rendered after the form has been submitted

```html
<codex-password-reset>
	<template v-slot:default></template>
</codex-password-reset>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-password-reset
`