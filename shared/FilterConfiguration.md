# Configuring Filters

Many components can take a `filters` prop, and internally use `<codex-filters>` to filter a resource. A resource can be any `Array` that contains items of type `Object`. A full list of components that have this functionality can be found at the end of this file.

## Config Object

The configuration object can be used to set-up how a resource should be filtered. It should contain either `key` / `value` pairs, or an array that include settings for individual filters, for example what selector to filter by and what type of UI to display:

```js
{
	instructorName: {
		selector: 'first_name',
		type: 'listbox'
	}
}
```

or

```js

	['instructor', 'event_type', 'duration_in_minutes']

```


The only reserved key name is `options` which can be used to [define instance settings](#instance-options).

The `values` objects can contain the following properties.


| Property | Description | Type |Example(s) | Default |
| :--- | :--- | :--- | :--- | :--- |
| `key` | The unique key for this filter.  If used in combination with `api`, it will be used as the query. | `String` | `instructor` | - |
| `api` | If set to true, filtering will be done server-side, instead of client-side. | `Boolean` | `true` | `false` |
| `default` | Allows filters to be set as active during first load. Also see [filtering with URL params](#url-params) | `Array` | `[1, 3]` | - |
| `multi` | Whether to allow multi-selection for this specific filter | `Boolean` | `true` / `false` | `true` |
| `options` | The list of selectable options. Can be hard-coded using an `Array` of objects that contain a `value` and `displayValue` or fetched from an API by specifying a `String` in the following format:<br/><br/>`api/PUBLIC_SERIVCE:VALUE:DISPLAY_VALUE`<br/><br/>If left `undefined` the options will fallback to dynamically generating a list of all possible values for the given selector based on the current data, removing any duplicates. | `String`<br/>or<br/>`Array`<br/>or<br/>`undefined` | string:<br/>`api/locations:id:name`<br/><br/>array:<br/>`[{ value: 1, displayValue: 'Nathan' }, { value: 2, displayValue: 'Eric' }]` | - |
| `selector` | The key to filter by. Can include object notation to check for nested values | `String` | `first_name`<br/>`studio.location.name`<br/>`event_type.group.id` | - |
| `title` | The placeholder / label value to be displayed in the UI | `String` | `Instructor Name` | Note: If no title is provided the object's key is used |
| `type` | The type of filter UI to display | `String` | `listbox`<br/>`multi-slider` | `listbox`  |
| `units` | For use with `multi-slider` only, creates a units label | `String` | `Minutes` | - |


The following are some examples of setting up filter configuration:

```vue

Use pre-defined filters:

<codex-instructors :filters="['instructor', 'event_type', 'duration_in_minutes']"></codex-instructors>

Mix of pre-defined filters with custom:

<codex-instructors :filters="['instructor', 'event_type', 'duration_in_minutes', {  
        key: 'metafields->difficulty',
        title: 'Difficulty',
        api: true,
        options: [
            { displayValue: 'Low', value: 'low'},
            { displayValue: 'Moderate', value: 'Moderate'},
            { displayValue: 'High', value: 'High'}
        ],
    }]"></codex-instructors>

Fully custom:

<codex-instructors :filters="{
	name: {
		selector: "full_name",
		multi: false,
		title: "Instructor Name"
	},
	musicGenre: {
		selector: "tags",
		multi: true,
		options: [
			{ value: 'music:hiphop', displayValue: 'Hip Hop' },
			{ value: 'music:pop', displayValue: 'Pop' }
		]
	}
}"></codex-instructors>


```

This would displayed the instructors component and generate the following set of filters for it:

| Key | Title (Displayed in the UI) | Type | Options (Displayed in the UI) | Multi-Select |
| :--- | :--- | :--- | :--- | :--- |
| `name` | Instructor Name | `listbox` | Dynamically generated - Creates an array of all unique values for `instructor.full_name` | No |
| `musicGenre` | musicGenre | `listbox` | Statically created - 'Hip Hop' or 'Pop' | Yes |


```vue
<codex-videos :filters="{
	instructor: {
		selector: 'instructor.id',
		options: 'api/instructors:id:name',
		title: 'Instructor',
		multi: true
	},
	duration: {
		selector: 'duration',
		title: 'Workout Length',
		type: 'multi-slider',
		units: 'Minutes'
	},
}">
</codex-videos>
```

In this example, we display the videos component and generate the following set of filters for it:

| Key | Title (Displayed in the UI) | Type | Options (Displayed in the UI) | Multi-Select |
| :--- | :--- | :--- | :--- | :--- |
| `instructor` | Instructor | `listbox` | Fetched from API - all available instructor names pulled from the `instructors` public service | Yes |
| `duration` | Workout Length | `multi-slider` | Dynamically generated - min / max values are created from the available dataset | N/A |


## Instance Options

As well as individual filter objects, a generic `options` object can be included in the filter config to specify instance settings. These settings apply to filter-related logic and components inside of the parent they are defined on:

| Property | Description | Type |Example(s) | Default |
| :--- | :--- | :--- | :--- | :--- |
| `searchKeys` | The selector(s) to perform a search against when using the search box. Some components have a default set of search keys | `Array` | `['id', 'name', 'description']` | Component Specific |
| `showSearch` | Whether to render the search box | `Boolean` | `true` / `false` | `true` |
| `or` | An array of optional filter keys. A resource will be considered a match if it matches one of these keys as well as all non-optional keys  | `Array` | `['workoutType', 'musicGenre']` | [] |

## Pre-defined filters

There are pre-defined filters you can use for simplicity - these include:

* instructor
* studio
* location
* event_type
* event_type_group
* duration_in_minutes

## URL Params

You can use URL params to set defaults for filters described in the filter config object. For example, the following combination would pre-select the studio location with the `id` of `4`:

`?location=4`

```
<codex-timetable :filters="{
	location: {
		selector: 'studio.location.id'
	}
}"></codex-timetable>

```

Multiple selections can be set, as follows:

`?location=4&location=6&event_type=1`

This would pre-select locations `4` and `6`, as well as event_type `1`.

Note: if the key used in the URL doesn't match a key in the filters config then it won't work as intended.

## Components

*To be updated...*

- [codex-instructors](../Instructors.md)
- [codex-timetable](../Timetable.md)
- [codex-video-collection](../VideoCollection.md)
- [codex-video-collections](../VideoCollections.md)
- [codex-videos](../Videos.md)

