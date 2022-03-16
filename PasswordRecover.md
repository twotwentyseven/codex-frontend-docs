# PasswordRecover

Renders a customer password reset request form.

## Quick usage

```vue
<codex-password-recover></codex-password-recover>
```

## Props

None

## Slots

**error-messages**

```html
<codex-password-recover>
	<template v-slot:error-messages="{ errors }"></template>
</codex-password-recover>
```

**title**

```html
<codex-password-recover>
	<template v-slot:title></template>
</codex-password-recover>
```

**message**

Rendered before the form

```html
<codex-password-recover>
	<template v-slot:message></template>
</codex-password-recover>
```

**end-content**

Rendered after the form

```html
<codex-password-recover>
	<template v-slot:end-content></template>
</codex-password-recover>
```

**default**

Rendered after the form has been submitted

```html
<codex-password-recover>
	<template v-slot:default></template>
</codex-password-recover>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-password-recover
`