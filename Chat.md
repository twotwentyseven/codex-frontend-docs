# Chat

Creates an interactive chat window for use in events and videos.

## Quick usage

```vue
<chat id="1234"></chat>
```

## Properties

| Name | Description | Options | Default |
|------|-------------|---------|---------|
| id | *Required* The chat room ID.  You can use an event or video ID, as long as it's unique. | A number or word that is unique | None |
| :display-leave | Announce when a customer enters the room | true, false | true |
| :start-hidden | Whether chat should start minimised instead of open | true, false | false |


## Other requirements

A `pusher_id` must be set on the global `window.codex` object that *must* match the one setup within Codex.

The `app_id` is used to namespace the chats to prevent collisions with other Codex installations.

Optional 'Quick Buttons' can also be setup within the global `window.codex` object.

``` javascript
window.codex = {
	api: 'https://codex.local',
	app_id: 'codex',
	chat: {
	    pusherKey: 'e67a0937abcdef123456',
	    messageButtons: [
		    {
		        icon: '😂',
		        message: '😂'
		    },
		    {
		        icon: '👍',
		        message: '👍'
		    },
		    {
		        icon: '😭',
		        message: '😭'
		    }
	    ]
	}
};
```

## Slots

**loading-message**

```html
<codex-chat>
	<template v-slot:loading-message></template>
</codex-chat>
```

## URL parameters

None

## Events

There are no events emitted by this component.
