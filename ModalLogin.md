# Modal Login

Shows a modal overlay login form.

## Quick usage

```html
<modal-login></modal-login>
```

## Properties

| Name | Description | Options | Default |
|------|-------------|---------|---------|
| auto-close | Hide modal after xxx milliseconds following a successful login. Set to false to disable. | false, Any valid integer | false |


## URL parameters

None

## Events

This component attaches an event listener to the document, looking for an event of `codex-login-toggle`.

You can trigger this listener with:

```javscript
document.dispatchEvent(new Event('codex-login-toggle'));
```

Doing so will toggle the modal open/close state class.

## Other

This component will add the class of `codex-modal-login-enabled` to the document body once initialised.