# Videos

Shows a listing of videos.  Authentication optional.

## Quick usage

```html
<videos></videos>
```

## Properties

| Name | Description | Options | Default |
|------|-------------|---------|---------|
| video-detail-url | The URL of the video detail page | A valid URL | '/pages/video' |
| video-event-url | The URL of the live event page | A valid URL | '/pages/event' |
| live | Show only live videos | true, false | false |
| carousel-settings | A Slick Slider configuration | See Slack Docs |  |
| carousel-type | Carousel type - either none (stacked) or a Slick Slider | 'none', 'VueSlickCarousel' | 'VueSlickCarousel' |
| pre-filter | Filter videos from API.  Accepts an object of key/values | { instructor, instructor-handle, duration, event_type, event_type_group, exclude, tags } | {} |
| filter | Show filter dropdowns | { duration, instructors, event_types, tags } | {} |
| related-to | Show related videos to this related video | A ref to a video in the same Vue root | None |
| related | Used in conjunction with related-to, filtered by the passed relationship | 'instructor', 'event_type', 'tags' | None |
| limit | Limit number of videos to show | Number | false |
| limit-more-url | Show a 'see more videos' link and direct to the URL supplied | A valid URL | false |

## URL parameters

None

## Events

There are no events emitted by this component.
