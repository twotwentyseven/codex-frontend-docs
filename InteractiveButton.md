# Interactive Button

Renders a button that is reactive and shows status.

## Quick usage

```html
<interactive-button
    default-text="Submit"
    processing-text="Submitting..."
    success-text="Submitted!"
    error-text="Problem submitting"
    :error="error"
    :processing="loading"
    :success="success"
    @clicked="doAction"
    >
</interactive-button>
```

## Properties

| Name | Description | Options | Default |
|------|-------------|---------|---------|
| :debounce | Disables click event whilst in processing state (stops double submissions) | true, false | true |
| :disableOnSuccess | Disables the button once action is successful | true, false | false |
| :returnToDefaultTimer | Switches the buttons state back to default after xxx milliseconds | A duration in milliseconds or false to disable | 5000 |
| disabledText | Button label when disabled | String | none |
| defaultText | Button label when in default state | String | none |
| processingText | Button label when processing action | String | none |
| successText | Button label when successful | String | none |
| errorText | Button label when error | String | none |
| disabledClass | Class that is given to button when disabled | String | none |
| defaultClass | Class that is given to button when default | String | none |
| processingClass | Class that is given to button when processing | String | none |
| successClass | Class that is given to button when successful | String | none |
| errorClass | Class that is given to button when error | String | none |
| :error | Value that will be 'truthy' in event of an error | true, false | none |
| :success | Value that will be 'truthy' in event of success | true, false | none |
| :processing | Value that will be 'truthy' when processing | true, false | none |
| :disabled | Disable button | true, false | false |

## URL parameters

None

## Events

This button emits a `clicked` event when clicked.
