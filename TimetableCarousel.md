# Timetable Carousel

Shows a timetable of events visible to customer, wrapped in a Slick Slider.  If customer authenticated also shows bookmarked events.

## Quick usage

```html
<timetable-carousel></timetable-carousel>
```

## Properties

| Name               | Description                                                                                                 | Options                                                                                       | Default                                        |
|--------------------|-------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------|
| event-detail-url     | URL to go to when event is clicked.  Event ID is appended to this URL with a /                              | Any valid URL                                                                                 | /pages/class                                   |
| days-to-show         | How many days per 'page' to show                                                                            | Any valid number                                                                              | 7                                              |
| days-to-scroll       | If using a carousel, how many days at a time to slide through                                               | Any valid number                                                                              | 7                                              |
| start-date          | Override the starting point of the timetable                                                                | Any valid date (i.e. YYYY-MM-DD)                                                              | Today                                          |
| :show-bookmarks-only | Only show timetable events that have been bookmarked by customer                                            | true, false                                                                                   | false                                          |
| :filter-keys        | Accepts an array of filters to look for, either on URL (i.e. ?studio=1) or combined with :filterProps below | 'studio', 'location', 'event_type', 'event_type_group', 'instructor'                          | ['studio', 'location', 'event_type', 'event_type_group', 'instructor'] |
| :filter-props       | Sets filters to supplied values                                                                             | An array of objects, with a filterKey and value, i.e. [{ studio: 1 }, { instructor: "simon"}] | []                                             |
| :show-filters       | Show in-built filters                                                                                       | 'studio', 'location', 'event_type', 'event_type_group', 'instructor'                          | ['location', 'event_type_group', 'instructor'] |
| settings           | Deprecated                                                                                                  | Deprecated                                                                                    | Deprecated                                     |
| :carousel-settings  | Accepts an object with Slick Slider settings                                                                | Any valid Slick Slider config                                                                 | See Slick Docs                                 |


## URL parameters (filtering)

If `:filterkeys`is set, or left default, the component will look for matching query strings to filter by.

i.e. if the url is `/timetable?location=1` the component will set the location filter to 1.

## Events

There are no events emitted by this component
