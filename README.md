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

Note: You will need to replace `yoursite.codexfit.com` with your own installation.

Next, you will need to drop components into your site - in this example we'll use the `<codex-login>` component.

To initialise the component, add a `data-codex` the parent element (ideally a `div` or `section`, but any block level element is fine).  For example:

```html
<body>
  <div data-codex>
    <codex-login />
  </div>
</body>
```

