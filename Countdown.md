# Countdown Timer

Shows a live countdown timer.

## Quick usage

```html
<countdown-timer date="2022-05-05 02:27:00"></countdown-timer>
```

> Hint: You can pass through a date dynamically, i.e. from an event:

```html
<countdown-timer :date="event.start_at"></countdown-timer>
```

## Properties

| Name | Description | Options | Default |
|------|-------------|---------|---------|
| date | Date/time to event start | Any valid date YYYY-MM-DD HH:MM:SS | Required |
| :show-empty-days | Show days as 00 if empty | true, false | false |
| :show-seconds | Show live seconds counter | true, false | true |


## URL parameters

None

## Events

There are no events emitted by this component.
