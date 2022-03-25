# VerifySMS

Renders an SMS verification UI.

## Quick usage

```vue
<codex-verify-sms></codex-verify-sms>
```

## Props

None

## Slots

**error-messages**

```html
<codex-verify-sms>
	<template v-slot:error-messages="{ errors }"></template>
</codex-verify-sms>
```

**logged-out**

```html
<codex-verify-sms>
	<template v-slot:logged-out></template>
</codex-verify-sms>
```

**header**

```html
<codex-verify-sms>
	<template v-slot:header></template>
</codex-verify-sms>
```

**description**

```html
<codex-verify-sms>
	<template v-slot:description></template>
</codex-verify-sms>
```

**buttons**   

```html
<codex-verify-sms>
	<template v-slot:buttons></template>
</codex-verify-sms>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-verify-sms
`

