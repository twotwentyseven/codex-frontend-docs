# Component Documentation Template

Please create detailed documentation for the [COMPONENT_NAME] component following this structure:

1. Start with a brief overview of the component's purpose and main functionality.

2. Show basic usage with a simple, practical example.

3. List all key features as bullet points.

4. Document all props in tables, grouped by category if applicable:

   ### Configuration Props
   - Prop name
   - Type
   - Required status
   - Default value
   - Description

   ### Common Props
   Document which common props from `@/config/common` are used:
   | Prop Name | Usage |
   |-----------|-------|
   | propName | How the prop is used in this component |

   ### Common Functions
   Document which utilities from `useCommon` are used:
   | Function | Usage |
   |----------|-------|
   | functionName | How the function is used in this component |

5. Document all events in a table with:
   - Event name
   - Parameters
   - Description

6. Document all slots in a table with:
   - Slot name
   - Description

7. If the component has states or transitions, document them in order of priority.

8. If the component uses internationalization, show the translation keys and their usage.

9. Provide multiple practical examples showing different use cases:
   - Basic usage
   - Complex scenarios
   - Usage with slots
   - Different states/variants
   - Common patterns

10. Document CSS classes used by the component.

11. Include a Best Practices section covering:
    - Recommended usage patterns
    - Common pitfalls to avoid
    - Accessibility considerations
    - Error handling
    - State management
    - Performance considerations

12. Note the component's registration name in the application (e.g., 'codex-[name]').

Please ensure:
- All examples are practical and realistic
- Code snippets use the correct component registration name
- Props, events, and slots are accurately documented
- Best practices are specific to the component's use case
- Documentation includes any relevant notes about browser compatibility, accessibility, or performance considerations
- Common props and functions are properly documented with real usage examples
- Loading states and responsive behavior using common utilities are explained

Use the following format for code examples:
```vue
<codex-component-name
  :prop="value"
  @event="handler"
>
  Content
</codex-component-name>
```

Reference the Button component documentation as an example of the expected detail and structure.