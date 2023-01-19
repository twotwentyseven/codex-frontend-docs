# CodexFit Frontend Documentation

CodexFit - The booking platform with Beast Mode

## Quick Start

The simplest and quickest way to get started with CodexFit frontend components is to use the auto-configure script.

Insert the following script tag into your document `head` and it will communicate with your Codex installation, retrieve settings and configurations, and automatically install scripts, stylesheets and languages into your page.  

You can also use overrides to tailor your settings to fit your requirements.

```html
<head>
  <script src="https://your-site.codexfit.com/autoconfigure.js" type="text/javascript"></script>
</head>
```

**Note:** You will need to replace `yoursite.codexfit.com` with your own installation.

Next, you will need to drop components into your site - in this example we'll use the `<codex-login>` component.

To initialise the component, add a `data-codex` the parent element (ideally a `div` or `section`, but any block level element is fine).  For example:

```html
<body>
  <div data-codex>
    <codex-login />
  </div>
</body>
```

**Note:** Any code inside the `data-codex` will be manipulated by the Codex components, so you should not add `<script>` or `<style>` tags inside, nor should you attach event handlers to elements inside (they may be removed).


## Adding components to your site

Once the above environment is set up, adding components to a page is as simple as dropping the relevant `<codex-xxx>` tag into any element that has the `data-codex` attribute, as per the example above.

Some components can be added in without additional properties (such as the `<codex-login />` and `<codex-logout />` tags), however some components may need additional configuration (i.e. filters applied to the data).  All components are documented here, including any properties (configurations), URL params (such as which URL query strings the component will look for - i.e. /?instructor=john-doe), and for more advanced developers, emitted events and overrides.


