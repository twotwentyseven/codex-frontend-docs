# Installation

- [Initial Configuration](#initial-configuration)
- [Setting the Stage](#setting-the-stage)
    - [Adding Additional Stages](#adding-additional-stages)


<a name="initial-configuration"></a>
## Initial Configuration

All of the configuration for Codex can be done automatically using our `autoconfigure.js` script - a one line installation method that will pull settings from your Codex environment (including Stripe Keys, locale, currency and much more), and configure almost everything you need, out of the box.

Simply include the following script in your HTML header, preferably as close to the opening `<head>` tag as possible.

```html
<head>
    <script src="https://your-site.codexfit.com/autoconfigure.js" type="text/javaScript"></script>
</head>
```


<a name="setting-the-stage"></a>
## Setting the stage

Codex uses advanced reactive 'components' that can be embedded into nearly any standard website - these do a wide variety of things, from displaying timetables, to logging in customers, and storing their details for use around your website.

Each component will need to be placed into a 'stage' that Codex is aware of, so it can expand into your page - a stage can be any valid block level element, with a unique ID (a `div` or `section` is usually ideal).

Out of the box, Codex defaults to one stage, looking for an ID of `codex-app` 

```html
<div id="codex-app">
    <p>You can embed your components within this 'stage':</p>
    <codex-login></codex-login>
</div>
```

You can embed as many codex components into this 'stage' as you wish - there is no hard limit.

> **Warning**  
> You should not include any CSS or scripts within this 'stage' - if you need to add scripts or CSS, please do them outside of the stage.


<a name="adding-additional-stages"></a>
### Adding Additional Stages

If you have multiple different areas that incorporate Codex components, and they need to live in areas not completely covered by the default stage, you can add additional stages.

First, define a block level element with a unique ID, and place your components inside:

```html
<section id="additional-stage">
    <codex-logout></codex-logout>
    <codex-bundles></codex-bundles>
</section>
```

Then, either in the `head` tag, or immediately after your stage definition, you can inform Codex of the additional stage ID:

```html
<script>
    Codex().addStage('additional-stage')
</script>
```

There is no fixed limit on the number of stages that can be created.


Next, update your `.env` configuration file to use Laravel's `sqlite` database driver. You may remove the other database configuration options:
