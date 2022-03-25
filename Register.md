# Register

Renders a customer registration form.

## Quick usage

```vue
<codex-register></codex-register>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| intro | Hide this component if there are no results to show | `Boolean` | `false` | - |
| showLabels | Show input labels | `Boolean` | `true` | - |
| showDateLabel | Show the label for date fields | `Boolean` | `true` | - |
| showIndividualDateLabels | Show the label for individual date fields (e.g 'Day') | `Boolean` | `false` | - |
| formFields | The fields to show in the form. See [form fields object](#setting-up-form-fields) | `Object` | `false` | - |
| buttonText | An object containing text for the form submission button in various states | `Object` | `{ defaultText: "Register", disabledText: "Register", processingText: "Registering..." }` |

> Note: Also see [common props](./shared/CommonProps.md) for descriptions of other available props.

## Setting Up Form Fields

It's possible to override the default fields provided by the register component by passing a custom `formFields` object:

```js
{
	/* The key is used as model key and must */
	/* match what the backend expects ...... */

	'first_name': {
		type: 'text', // REQUIRED --- 'text' | 'email' | 'password' | 'select' | 'date' | 'phone' | 'checkbox'
		label: 'First Name', // REQUIRED
		placeholder: 'Enter your first name', // OPTIONAL --- will use the label value if not defined
		required: true, // OPTIONAL --- will add the required attribute to the field
	},
	'gender': {
		type: 'select',
		label: 'Gender',

		// REQUIRED --- options must be supplied when using a select input
		options: [
			{ value: 'female', displayValue: 'Female'},
			{ value: 'male', displayValue: 'Male' },
			{ value: 'rather_not_say', displayValue: 'Rather Not Say'}
		],

		// REQUIRED --- must mark as a metafield when it is not standard customer data
		metafield: true, 
	}
}
```

## Slots

**error-messages**

```html
<codex-register>
	<template v-slot:error-messages="{ errors }"></template>
</codex-register>
```

**loading-message**

```html
<codex-register>
	<template v-slot:loading-message></template>
</codex-register>
```

**end-content**

```html
<codex-register>
	<template v-slot:end-content></template>
</codex-register>
```

**logged-in-title**

```html
<codex-register>
	<template v-slot:logged-in-title></template>
</codex-register>

**logged-in-links**

```html
<codex-register>
	<template v-slot:logged-in-links></template>
</codex-register>

**logout**

```html
<codex-register>
	<template v-slot:logout></template>
</codex-register>
```

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

`
codex-template-register
`

