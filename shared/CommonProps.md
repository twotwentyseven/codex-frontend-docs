# Common Props

Most components share a set of props and methods that can be used for common tasks if required.

## Props

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| title | Usually used as the component's main header | `String` | - |
| titleTag | The html tag to wrap the component's title in | `String` | `'h1'` | `'h1' \| 'h2' \| 'h3' \| 'h4' \| 'h5' \| 'h6'` |
| contentWrapperType | The component / element to wrap the component's main content in | `String` | `div` | - |
| contentWrapperSettings | An object containing settings to pass through to the main content wrapper and / or children. See [Content Wrapper Settings](./ContentWrapperSettings.md).  | `Object` | - | - |
| imageWrapperType | Similar to `contentWrapperType` but specifically for the main image contents. Used in less components. | `String` | `div` | - |
| imageWrapperSettings | An object containing settings to pass through to the image content wrapper.  | `Object` | - | - |


