# InstructorCard

Renders an individual instructor card, displaying basic instructor information. Intended for use inside of `<codex-instructors>`

## Quick usage

```vue
<codex-instructor-card v-for="instructor in filteredResource" :instructor="instructor" :key="instructor.id"></codex-instructor-card>
```

## Props

**Required**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| instructor | The Instructor object | `Object` | - | - |

**Optional**

| Name | Description | Type | Default | Validation |
| - | - | - | - | - |
| showTags | A list of tags to display within the card, if set on the instructor | `Array` | - | - |


## Slots

None

## URL parameters

None

## Events

`view`

## Override

`
codex-template-instructor-card
`

