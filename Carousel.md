# Carousel

Wraps its default slot content in a carousel. Currently only supports `swiper.js`. This component utilises a render function template in order to wrap default slot content.

## Quick usage

```vue
<codex-carousel 
	:carouselProps="{
		navigation: true,
		slidesPerView: 4,
		spaceBetween: 30,
		breakpoints: {
			0: {
				disabled: true, // this will disable the carousel until the next breakpoint
			},
			1024: {
				slidesPerView: 4,
			}
		}
	}"
></codex-carousel >
```

## Props

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| carouselProps | The props to be passed through to the underlying carousel component | `Object` | `{}` | - |
| carouselAttrs | The attributes to be passed through to the underlying carousel component | `Object` | `{}` | - |
| slideAttrs | The attributes to be passed through to the carousel slides | `Object` | `{}` | - |
| matchHeight | ** Requires jQuery (sorry) ** selectors to height match on mounting and slide changes. See [matchHeight](https://github.com/liabru/jquery-match-height) for available options | `Object` | `{ selectors: [], options: {} }` | - |


## Slots

None

## URL parameters

None

## Events

There are no events emitted by this component.

## Override

None

