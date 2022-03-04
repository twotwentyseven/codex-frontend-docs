# ContentWrapperSettings

This object can be used to pass props / attributes to elements defined using the `contentWrapperType` pattern.

## Example Usage

```vue
<codex-instructors content-wrapper-type="swiper" :content-wrapper-settings="{
	carouselProps: {
		navigation: true,
		slidesPerView: 4,
		spaceBetween: 30,
		breakpoints: {
			0: {
				/*
				 * Note: 'disabled' is a non-standard swiper setting 
				 * for use with <codex-carousel>
				 */

				disabled: true, 
			},
			1024: {
				slidesPerView: 4,
			}
		}
	},
	slideAttrs: {
		'class': 'custom-slide-name',
		'data-custom': 'custom data attribute'
	}
}" >
</codex-instructors>
```

In the above example we are passing a prop called `contentWrapperType` with a value of 'swiper'. This will wrap the main content of `<codex-instructors>` to be wrapped in a swiper carousel (via our own custom `<codex-carousel>` component).

We are also passing a second prop called `contentWrapperSettings`. The exact result of passing this prop will be dependent on the component it is used within, and the value of `contentWrapperType`, but in this example the `carouselProps` will be passed through to `swiper` and the `slideAttrs` will be passed to `swiper-slide` instances. In most non-carousel uses the contents of `contentWrapperSettings` will be spread onto the `contentWrapperType`.