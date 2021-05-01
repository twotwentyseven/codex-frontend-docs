# Waitlist Confirm

Shows event details and an acceptance button to accept an invite to convert a waitlist to a booking.  Requires authentication.

## Quick usage

```html
<waitlist-confirm></waitlist-confirm>
```

## Properties

| Name | Description | Options | Default |
|------|-------------|---------|---------|
| id | The ID of the waitlist invitation to confirm | Any valid ID | See URL Parameters |

## URL parameters

If no ID is passed as a parameter, the component will look for either a query strong variable (id), i.e. ?id=xxxx, or it will attempt to take the last segment of the URL, i.e. /page/waitlist-confirm/123

## Events

There are no events emitted by this component.
