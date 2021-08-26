# Instructors

Shows a list of instructors.  Optionally filtered by instructor ID or Handle.

## Quick usage

```html
<codex-instructors
  instructor-detail-url="/pages/instructor/{ id }/detail"
  only-handles="eric, sam, rich"
  exclude-ids="[1, 3, 6]"
>
</codex-instructors>
```

## Properties

| Name               | Description                                                                                                 | Options                                                                                       | Default                                        |
|--------------------|-------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------|
| instructor-detail-url | URL to go to when event is clicked.  String is parsed and { keys } replaced with instructor data.  e.g. /instructors/{ handle } | Any valid string | None |
| only-handles | Only show instructors that have matching handles | Array or String (comma separated) | ['anthony', 'paul', 'simone'] |
| exclude-handles  | Exclude instructors that have matching handles | Array or String (comma separated) | ['david', 'simon'] |
| only-ids         | Only show instructors that have matching IDs | Array or String (comma separated) | [1, 2, 3] |
| exclude-handles         | Exclude instructors that have matching IDs | Array or String (comma separated) | [5, 6, 7] |


## URL parameters (filtering)

n/a

## Events

There are no events emitted by this component
