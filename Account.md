# Account

Renders a container component for the account area. `<router-view>` setup is handled in here.

## Quick usage

Codex account must be rendered inside a root element with an ID of 'codex-account'. This is to allow [vue-router](https://router.vuejs.org/) to be used in the account area without interfering with other components / and standard links on the site.

```vue
<div id="codex-account">
	<codex-account>
		<codex-account-nav-links :routes="window.codex.routerConfig.routes"></codex-account-nav-links>
	</codex-account>
</div>
```

## Setup

`<codex-account>` is different to other components because we decorate then children's render functions with some additional configuration. We can setup routes inside of the `window.codex` configuration object and pass them as a prop to `<codex-account-nav-links>`. This nav-links component doesn't have to be rendered inside of `<codex-account>` but it does still need to be inside of the root element otherwise vue-router will not work correctly.

Each route needs a `path` to check against, and a `component` that should be rendered as the view when the URL matches the combined `base` and `path`. The available components to render as route views are also pre-defined inside of `configureVueApp.js`.

A typical `window.codex` configuration object with some router config looks like this:

```js
{
	api: 'https://client-name.codexfit.com',
	stageIds: ['codex-account', /* other stageIds */],

	/* ... additional config ... */

	routerConfig: {
		
		/* Define the base URL used by the router */
		
		base: '/pages/account/',

		/*
		 * Define the routes. The 'title' property is used as link text. 
		 * A children array can also be passed to a route if desired
		 * to state which components should be rendered inside of the main
		 * view. Props for these components can also be passed in here.
		 * Alternatively, slots can be used within <codex-account> to provide
		 * far great control over child components.
		 */
		
		routes: [
			{ path: '/', component: 'overview', title: 'Account Overview' },
			{ path: '/details', component: 'details', title: 'My Profile',
				children: [
					{
						component: 'codex-profile',
						props: {
							fieldsPerRow: {
								small: 1,
								medium: 2,
								large: 3
							},
							title: 'My Custom Component Title'
						}
					}
				]
			},
			{
				path: '/favourites', component: 'favourites', title: 'My Favourites',
				children: [
					{
						component: 'codex-instructors',
						props: {}
					},
					{
						component: 'codex-videos',
						props: {}
					}
				]
			}
		]
	}
}
```

In reality, most client sites are going to have some kind of design or customisation beyond what can be achieved by solely using the above setup. `<codex-account>` has a default slot that can be used to render the account-links and any other kind of common layout, and it also creates slots for each route:

## Account Slots

**default**

```html
<codex-account>
	<template v-slot:default>
		<codex-account-nav-links :routes="window.codex.routerConfig.routes">
		</codex-account-nav-links>
	</template>
</codex-account>

<!-- or simply -->

<codex-account>
	<codex-account-nav-links :routes="window.codex.routerConfig.routes">
	</codex-account-nav-links>
</codex-account>
```

**router-view-slots**

For each available router-view, there is a named slot that be used for overrides. The slot name is determined by the component name.

```html
<codex-account>
	<template v-slot:account-overview="{ component, ...slotProps }">
		<component v-bind="slotProps" :is="component">
		</component>
	</template>
</codex-account>
```

## View Slots

Each view has the following three slots that can be used to add additional content to the account area pages.

**header, content, footer**

```html
<codex-account>
	<template v-slot:account-overview="{ component, ...slotProps }">
		<component v-bind="slotProps" :is="component">
			<template v-slot:header="{ customer }"></template>
			<template v-slot:content>
				<codex-milestones :circle-data="{ diameter: 160, strokeWidth: 12 }"></codex-milestones>
			</template>
			<template v-slot:footer="{ customer }"></template>
		</component>
	</template>
</codex-account>
```

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| layout | Determines which class name to append to `cdx_page-view` | `String` | `'stacked'` | Must be either 'stacked' or 'tabs' |


## Routes

Vue-router works by matching the URL pathname to a particular view. It also takes over standard link behaviour and instead utilises HTML5 history state. As a result the whole account area can be handled via one single page, e.g. `/pages/account` and vue-router handles the rest. To read more about how this works read the [Vue-Router docs](https://router.vuejs.org/).
