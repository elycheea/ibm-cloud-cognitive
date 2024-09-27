---
name: (C4IBM Products team only) Code completion checklist 🐦
about: Code evaluation before the release of a component as stable
---

As part of the Carbon ecosystem, we’ve aligned on a “[definition of done]()” to
ensure that all components are built with the same quality and assurance.

Before component code can be released as stable, it must meet all of the
following requirements.

## Review for release

### Prerequisites

- [ ] The component(s) has been built to spec. (Link to completed design spec
      review).
- [ ] The component(s) have been reviewed for accessibility[^1].

### Guiding principles

Careful consideration of API design ensures that future changes to the system
can be made in an iterative way that minimizes disruption.

- [ ] The component API **prioritizes the developer experience** above
      complexity of implementation. - [ ] The component API feels approachable.
- [ ] The component supports **interoperability** and supports a wide band of
      coverage: <!-- prettier-ignore-start --> - [ ] API is not
      [limited to specific versions of React](#). - [ ] API is not
      [limited to specific versions of node](#). <!-- prettier-ignore-end -->
- [ ] If the component replaces another, the previous version should be
      deprecated.
- [ ] The component is broken into logical pieces and supports multiple
      configurations that may be required by the business.
- [ ] The exposed API is consistent with existing Carbon conventions and are
      consistent internally.

### API design

The component should follow the
[Carbon style guide](https://github.com/carbon-design-system/carbon/blob/main/docs/style.md).

- [ ] Components are functional components using hooks.
- [ ] Public components support a ref (via `React.forwardRef`).
- [ ] Public components support a Devtools attribute
- [ ] Where applicable and if possible, `className`, `data-testid`, and
      `...rest` are placed on the outermost, parent, or root element.
- [ ] If dynamic or inline styles are necessary, they are set on the
      `CSSStyleDeclaration` interface object[^2], usually using a
      `useIsomorphicEffect` hook.
- [ ] `useCallback` and `useMemo` are used sparingly[^3], except in the
      following conditions: - The identity of a function or object is required
      as a dependency in a dependency array. - Performance issues were observed
      due to allocations that can be reproduced and resolved using these
      techniques.
- [ ] Hooks that require a reference to a DOM node accept `ref` as an
      argument[^4].
- [ ] Hooks that use callbacks make use of the saved callback pattern[^5].
- [ ] Event handlers match the exact name (`onClick`), or follow the convention
      of `handleOnClick` if already used in the current scope.
- [ ] No warnings, errors or log messages in the console.

### Styles

- [ ] The component responds to the currently set Carbon theme.
- [ ] Component styles use tokens available in the design system.
- [ ] Component styles do not contain magic numbers[^6] or colors that are not
      tokenized.
- [ ] Component styles avoid nested selectors.
- [ ] Component styles use only as much specificity as needed.
- [ ] All significant DOM elements have meaningful classes[^7].
- [ ] Selectors leverage the `$pkg.prefix` or `$carbon.prefix`, and prefixes are
      not hardcoded.
- [ ] Relevant Sass values are annotated with SassDoc. - [ ] SassDoc comments
      use three forward slashes.
- [ ] Annotations and comments are added above the specific line of code rather
      than inline.
- [ ] No linter warnings or errors are produced.

### Globalization

Assets must be reusable within products and offerings worldwide.

- [ ] Labels, text, or other strings within a component should use a prop.
- [ ] In complex component translation, a map of translation IDs with defaults
      is defined with a default `translateWithId` prop[^8].

### Responsiveness

Assets must work across a wide range of screen sizes so that IBM products and
offerings can be used on as many devices as possible.

- [ ] Component layout, functionality, and UX works on all device sizes from
      very large to ~`320px` wide.
- [ ] Component styles use media queries where needed.
- [ ] Component is usable and accessible cross-browser.
- [ ] All components utilizing motion include reduced-motion queries.

### Standards

- [ ] Carbon tokens (theme, layout, motion) are used unless the design specifies
      otherwise.
- [ ] Code is formatted according to prettier rules (no use of
      `//prettier-ignore`).
- [ ] Code is clear, maintainable and follows standard React practices and the
      code guidelines.
- [ ] Copyright header in every source file (js, css, scss etc.) with the
      appropriate years.
- [ ] Each component has an interface with all props typed.
- [ ] Each component interface is exported for use in consuming projects.
- [ ] The component has been exported properly. <!-- prettier-ignore-start --> -
      [ ] The component(s) JS is exported in `/src/components/index.js`. - [ ]
      The component SCSS is included in `/src/components/_index.scss`. - [ ] The
      component has a flag in `package-settings.js`.
      <!-- prettier-ignore-end -->
- [ ] The component SCSS file explicitly imports the SCSS for each of the Carbon
      and IBM Products components imported and used by the JavaScript code.
- [ ] If the component or change requires migration by consumers, an automated
      code migration script has been written and made available through
      `@carbon/upgrade`.

### Documentation

- [ ] A default story and playground exists in Storybook.
- [ ] Additional stories are provided for variants or common design scenarios.
- [ ] The stories expose all relevant props and actions.
- [ ] Long-form documentation, including code samples and the props table, is
      provided in Storybook.
- [ ] Source code is satisfactorily commented and provides DocGen comments for
      all public components and their props.
- [ ] Code docs are written and ready to publish to the website.
- [ ] There is an example (or multiple sandboxes if appropriate) on CodeSandbox
      and Stackblitz for the components.

### Testing

We aim to test components from a user-focused perspective. This means we avoid
testing implementation details, and instead focus on writing tests that closely
resemble how the components are used. While our testing infrastructure may
differ slightly from `@carbon/react`, the
[strategy and principles](https://github.com/carbon-design-system/carbon/blob/main/docs/style.md#testing)
behind our approach are the same.

#### Unit testing

- [ ] The component API and functionality has been thoroughly tested using
      `jest` and `testing-library`. <!-- prettier-ignore-start --> - [ ] There
      is a set of test cases for the components. - [ ] Each prop in the
      component API has a test to ensure that the prop behaves as expected and
      that outputs are appropriate. <!-- prettier-ignore-end -->
- [ ] Component unit test coverage should meet or exceed 80% of functions,
      lines, statements, etc.
- [ ] Component includes testing in a server-side environment.
- [ ] Usage of `istanbul ignore` or `.skip`s are appropriate and not extensive.
- [ ] No warnings, errors, or log messages in the test output.

#### Visual regression tests

- [ ] The component has at least one test for VRT using the default story.
- [ ] “Problematic” or concerning component states, stories, or viewport widths
      may also be added to VRT.

#### Accessibility verification tests

- [ ] The component tests include at least one test of its default state by
      [`accessibility-checker`](https://www.npmjs.com/package/accessibility-checker)/
- [ ] The component tests include tests for any additional complex states
      (open/closed, highlighted, expanded, focused, hovered, clicked, etc) by
      `accessibility-checker`.

[^1]:
    Accessibility reviews include manual testing via screen reader/voiceover,
    typically by the accessibility QA team, and playbacks via the Carbon
    Accessibility Guild.

[^2]:
    [Authoring dynamic/inline styles](https://github.com/carbon-design-system/carbon/blob/main/docs/style.md#authoring-dynamicinline-styles)
    from the Carbon style guide

[^3]:
    [Using `useCallback` and `useMemo`](https://github.com/carbon-design-system/carbon/blob/main/docs/style.md#using-usecallback-and-usememo)
    from the Carbon style guide

[^4]:
    [Hooks that rely on refs](https://github.com/carbon-design-system/carbon/blob/main/docs/style.md#hooks-that-rely-on-refs)
    from the Carbon style guide

[^5]:
    [Hooks that use a callback](https://github.com/carbon-design-system/carbon/blob/main/docs/style.md#hooks-that-use-a-callback)
    from the Carbon style guide

[^6]:
    [Avoid magic numbers](https://github.com/carbon-design-system/carbon/blob/main/docs/style.md#avoid-magic-numbers)
    from the Carbon style guide

[^7]:
    See Carbon’s
    [Developer Handbook](https://github.com/carbon-design-system/carbon/blob/main/.github/CONTRIBUTING.md)
    for guidance on
    [class names](https://github.com/carbon-design-system/carbon/blob/main/docs/developer-handbook.md#class-names).

[^8]:
    [Translating a component](https://github.com/carbon-design-system/carbon/blob/main/docs/style.md#translating-a-component)
    from the Carbon style guide
