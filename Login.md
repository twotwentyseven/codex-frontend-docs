# Login

Shows a login form.

## Quick usage

```html
<codex-login></codex-login>
```

## Props

| Name | Description | Type / Required | Default | Validation |
|- | - | - | - | - |
| title | The title inside of `v-slot:title` | `String`/`false` | 'Login' | none |
| titleTag | The html tag to use for the title | `String`/`false` | 'h1' | `['h1', 'h2', 'h3', 'h4', 'h5', 'h6']`
|


## Slots  

**title**   

```vue
<!-- Login.vue -->


<slot name="title" :title="title" :titleTag="titleTag">
	<component :is="titleTag" class="cdx_title">{{ title }}</component>
</slot>
```

```html
<!-- html template -->

<template v-slot:title>
	<h1>New Title</h1>
</template>
```

**message**

```vue
<!-- Login.vue -->

<slot v-if="!errorMessages || !errorMessages.length" name="message">
	<div class="cdx_overview">
		Login to get started.
	</div>
</slot>
```

```html
<!-- html template -->

<template v-slot:message>
	<div class="another-classname">Custom Message</div>
</template>
```

**error-messages**

```vue
<!-- Login.vue -->

<slot v-else name="error-messages" :errors="errorMessages">
	<codex-error-messages message="There was a problem logging you in" :errors="errorMessages" />
</slot>
```

```html
<!-- html template -->

<template v-slot:error-messages>
	<ul>
		<li v-for="error in errors" :key="error">{{ error }}</li>
	</ul>
</template>
```


## URL parameters

None

## Events

There are no events emitted by this component.

## Override

```
codex-template-login
```

