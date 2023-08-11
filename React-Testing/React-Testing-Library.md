### learning react testing library

Blog = https://www.robinwieruch.de/react-testing-library/

# Render and Screen

The render function in React Testing Library is used to create a simulated or
fake DOM (also known as a virtual DOM) for a component. This virtual DOM is an
in-memory representation of the component's rendered output.

By rendering the component into this fake DOM, you can then use the screen
object or other testing library utilities to interact with and query the
elements within that simulated DOM.

When you call screen.debug(), it logs a visual representation of the selected
DOM nodes (HTML) to the console.log() This can help you inspect the rendered
output of your components and analyze the DOM structure at a specific point in
your test.

### Query

# about queue

# Manual Queries

On top of the queries provided by the testing library, you can use the regular
querySelector DOM API to query elements. Note that using this as an escape hatch
to query by class or id is not recommended because they are invisible to the
user. Use a testid if you have to, to make your intention to fall back to
non-semantic queries clear and establish a stable API contract in the HTML.

```
// @testing-library/react
const {container} = render(<MyComponent />)
const foo = container.querySelector('[data-foo="bar"]')
```

you can use javascript selector like qureyselector or getelementbyid but it is
not recommended becouse user can not use them but you can in case of emergeny.

# Browser extension

Do you still have problems knowing how to use Testing Library queries?

There is a very cool Browser extension for Chrome named Testing Playground, and
it helps you find the best queries to select elements. It allows you to inspect
the element hierarchies in the Browser's Developer Tools, and provides you with
suggestions on how to select them, while encouraging good testing practices.

# Search type:

Most Common Search type are getByText and getByRole you should manly relay on
them to select your element,

1. Screen.getByText("XYZ") = Finds an element in the DOM that contains the
   specified text.

If no element is found that matches the specified text, getByText will throw an
error, indicating that the element could not be found. This behavior helps
ensure that the expected element with the specified text is present in the
rendered output.

It's important to note that getByText searches for elements that contain the
exact specified text. If you want to perform partial matching or
case-insensitive matching, you can use regular expressions or other matching
techniques in combination with getByText.

- FOR Partial Matching: screen.getByText(/Welcome/)

- FOR Case-Insensitive Matching: screen.getByText(/Welcome/i)

2. Screen.getByRole("XYZ") =

The getByRole method is particularly useful when testing accessible components
that rely on ARIA roles for conveying their purpose or behavior. It ensures that
the correct element is selected based on its role, rather than relying on
specific attributes or text content.

In addition to the basic roles like "button", other common roles that can be
used with getByRole include "link", "heading", "textbox", "checkbox", "radio",
"list", "listitem", and many more. You can also use custom roles defined in your
application.

By utilizing getByRole, you can easily select and interact with elements in the
rendered output based on their ARIA roles, facilitating more robust and
accessible testing of your components.

- Note:

When you use getByRole, it looks for elements in the rendered output that have a
matching ARIA role specified through the role attribute. It does not select
elements based on the inferred or implicit behavior of the elements.

it do select implicit aria role

For example, if you have an element with a button appearance and behavior but it
does not have the role="button" attribute explicitly set, getByRole('button')
will not select it.

3. Screen.getByLabelText("XYZ") =

The getByLabelText query method looks for both the for attribute of <label>
elements and the aria-label attribute.

4. Screen.getByPlaceholderText("XYZ") =

The getByPlaceholderText query method is used to find an input element in the
rendered output based on its placeholder text. It allows you to select elements
by matching the text content of their placeholder attribute.

The getByPlaceholderText method is particularly useful when you want to select
an input element based on the descriptive text displayed in the input field when
it is empty or before the user enters any value.

5. Screen.getByAltText("XYZ") =

The getByAltText method is particularly useful when you want to select an image
element based on its alternative text description. The alt text provides a
textual description of the image content, making it more accessible for users
who cannot view the image or rely on assistive technologies.

By utilizing getByAltText, you can easily select an image element based on its
alt text, allowing you to test the presence and behavior of images in your
application.

6. Screen.getByDisplayValue("XYZ") =

getByDisplayValue allows you to select an input element based on its value
attributes. generally used to select input,Textarea.

7. Screen.getByTitle("XYZ") =

getByTitle Returns the element that has the matching title attribute. Will also
find a title element within an SVG.

8. Screen.getByTestId("XYZ") =

getByTestId select any element that has data-testid attribute in it. this is a
last resort search type which you should only use when getByRole and getByText
are not useful.

# Each-query with options

## getByRole

```
getByRole(
  // If you're using `screen`, then skip the container argument:
  container: HTMLElement,
  role: string,
  options?: {
    hidden?: boolean = false,
    name?: TextMatch,  // label or aria-label
    description?: TextMatch,
    selected?: boolean,
    busy?: boolean,
    checked?: boolean,
    pressed?: boolean,
    suggest?: boolean,
    current?: boolean | string,
    expanded?: boolean,
    queryFallbacks?: boolean,
    level?: number,
    value?: {
      min?: number,
      max?: number,
      now?: number,
      text?: TextMatch,
    }
  }): HTMLElement
```

here is the detailed explaination of options that you can use with ByRole search
queue.

1. hidden: This option allows you to include elements that are normally excluded
   from the accessibility tree in the query. By default, elements with
   aria-hidden="true" are excluded. Setting hidden: true includes these elements
   in the query. For example, if you have a hidden button inside a container,
   getByRole('button') would not find it, but getAllByRole('button', { hidden:
   true }) would.

2. name or (accessable name): https://www.tpgi.com/what-is-an-accessible-name/

NOTE: anytype of element that can take test as <>Text</> that text is it's
accessable name those who can't take text as child use other means to define
accessible name like input use label or use aria-labelBy as last resout.

3. selected: You can filter the returned elements based on their selected state
   by setting selected: true or selected: false. This option is useful for
   elements like tabs or list items that can have a selected state indicated by
   the aria-selected attribute.

4. busy: The busy option allows you to filter elements based on their busy
   state. Elements with the aria-busy="true" attribute can be filtered by
   setting busy: true, and elements with aria-busy="false" can be filtered by
   setting busy: false. This option is commonly used for displaying loading
   indicators or progress spinners.

extra info about aria-busy: The aria-busy attribute is an accessibility
attribute that can be applied to elements in order to indicate that they are
currently busy or in a state of processing. It is typically used to provide
feedback to users when an action is being performed and to indicate that the
element is not yet ready for interaction.

When an element has the aria-busy="true" attribute, it conveys to assistive
technologies and screen readers that the element is in a busy state. This can be
used to notify users that an operation is in progress, such as when a form is
being submitted or when content is being loaded asynchronously.

4. checked: You can filter elements based on their checked state using the
   checked option. Setting checked: true filters elements with
   aria-checked="true", and setting checked: false filters elements with
   aria-checked="false". This option is commonly used with checkboxes or radio
   buttons.

5. current: This option allows you to filter elements based on their current
   state. By setting current: true, you can filter elements with
   aria-current="true", and by setting current: false, you can filter elements
   with aria-current="false". The aria-current attribute is often used in
   navigation menus or lists to indicate the currently active item.

6. pressed: The pressed option is used to filter elements based on their pressed
   state. Elements with the aria-pressed="true" attribute can be filtered by
   setting pressed: true, and elements with aria-pressed="false" can be filtered
   by setting pressed: false. This option is commonly used for buttons or toggle
   switches.

extra info about aria-pressed: The aria-pressed attribute is an accessibility
attribute that can be applied to elements to indicate their pressed or toggled
state. It is commonly used with interactive elements like buttons or toggle
switches to provide feedback on their current state to users who rely on
assistive technologies.

The aria-pressed attribute can have the following values:

- true: Indicates that the element is in a pressed or toggled state.
- false: Indicates that the element is not pressed or toggled.
- undefined or not present: The pressed state of the element is not explicitly
  defined.

7. suggest: By default, when querying elements, the testing library may provide
   suggestions for the query based on its understanding of the accessibility
   tree. You can disable these suggestions by setting suggest: false. Setting
   suggest: true enables the suggestions for the query. This option allows you
   to control whether or not to receive suggestions during the
   query.throwSuggestions is currently in experimental mode

8. expanded: The expanded option is used to filter elements based on their
   expanded state. Setting expanded: true filters elements with
   aria-expanded="true", and setting expanded: false filters elements with
   aria-expanded="false". This option is often used with menus or collapsible
   sections.

extra info about aria-expanded: The aria-expanded attribute is an ARIA attribute
that is used to indicate the expanded or collapsed state of an element that
controls the visibility or accessibility of additional content. It is primarily
used in conjunction with interactive elements like buttons, links, or menu items
that have expandable or collapsible sections associated with them.

The aria-expanded attribute can have two possible values:

- "true": Indicates that the controlled content is currently expanded and
  visible.
- "false": Indicates that the controlled content is currently collapsed and not
  visible.

By using the aria-expanded attribute, assistive technologies can convey the
current state of the controlled content to users with disabilities. Screen
readers, for example, can announce whether a collapsible section is expanded or
collapsed, providing users with important contextual information about the
visibility of the content.

9. queryFallbacks: By default, only the first role of an element is considered
   in the query. If you need to query an element by any of its fallback roles,
   you can set queryFallbacks: true. This option enables all fallback roles and
   allows you to match elements with any of those roles. It's useful when you
   want to support older environments that may not understand newer roles.

ex: <div role="switch checkbox" /> getByRole('switch') will work but what if we
want to take the fallback role which is checkbox then getByRole('checkbox', {
queryFallbacks: true })

10. level: The level option is used to query elements with the heading role
    based on their level. For example, getByRole('heading', { level: 2 }) would
    query elements with the heading role at level 2, such as <h2> elements. The
    level option can be used with the semantic HTML headings <h1> to <h6> or
    with the aria-level attribute.

extra infro about aria-level: The aria-level attribute is an ARIA (Accessible
Rich Internet Applications) attribute that is used to convey the hierarchical
level or nesting level of an element within a document structure. It is
primarily used for elements that have a hierarchical relationship, such as
headings, list items, or table rows.

The aria-level attribute accepts a numerical value, starting from 1, that
represents the level of nesting or hierarchy. The value indicates the importance
or significance of the element within the document structure. The higher the
value, the deeper the element is nested or the more significant it is
considered.

11. Value:

- The value option allows you to query a range widget (in this case, an element
  with the role "spinbutton") based on its current value or a specific value.
- For example, getByRole('spinbutton', { value: { now: 5, min: 0, max: 10, text:
  'medium' } }) allows you to query a spin button with specific values defined
  by now (current value), min (minimum value), max (maximum value), and text (a
  text representation of the value).
- You can also query with a subset of these properties. For instance,
  getByRole('spinbutton', { value: { now: 5, text: 'medium' } }) queries a spin
  button with a specific current value and text representation.

extra info about aria-value : The term "aria-value" refers to a group of related
ARIA attributes that are used to describe the current value or state of an
element to assistive technologies. These attributes provide information about
the range, progress, or value-related properties of interactive controls.

Here are the main ARIA attributes related to "aria-value":

- aria-valuemin: It specifies the minimum allowed value for a range widget, such
  as a slider or progress bar.

- aria-valuemax: It specifies the maximum allowed value for a range widget.

- aria-valuenow: It represents the current value of a range widget. It indicates
  the widget's current position within the allowed range.

- aria-valuetext: It provides an alternative textual representation of the
  current value. This attribute can be used to convey additional information or
  describe the value in a more meaningful way than the numeric value provided by
  aria-valuenow.

These attributes are typically used in conjunction with roles like "slider,"
"progressbar," "spinbutton," or other interactive controls where a value or
range is involved. By setting and updating these attributes, developers can
ensure that the state and value of these controls are communicated to assistive
technologies accurately.

- why to use aria-valuetext?

- Customized Representation: Some controls, such as sliders or progress bars,
  may have numeric values that are not easily understandable or meaningful to
  all users. For example, a slider might have numeric values ranging from 0 to
  100, but these values may not convey much information to a visually impaired
  user. By using aria-valuetext, developers can provide a more descriptive
  representation of the value, such as "low," "medium," or "high." This
  customized representation can enhance the user's understanding of the
  control's current state.

- Localization and Language Adaptation: For international applications, numeric
  values may need to be adapted to different languages or cultural contexts.
  aria-valuetext allows developers to provide translated or adapted value
  descriptions based on the user's locale or language preferences. This ensures
  that disabled users, regardless of their language or location, can understand
  and interact with the control effectively.

- Auditory Feedback: Some assistive technologies, such as screen readers, can
  announce the aria-valuetext attribute to users. This auditory feedback
  provides an additional means of conveying the value to disabled users who rely
  on non-visual interactions. By hearing the descriptive representation of the
  value, users can understand the control's current state without relying solely
  on visual cues.

12. description: The description option allows you to filter returned elements
    based on their accessible description when you have multiple elements with
    the same role but different descriptions.

- It is useful in cases where elements, such as those with the alertdialog role,
  use the aria-describedby attribute to describe their content.
- For example, getByRole('alertdialog', { description: 'Your session is about to
  expire' }) queries a specific alertdialog element based on its description.

## TextMatch

1. String Matching:

- To find an element with an exact string match, use screen.getByText('Hello
  World').
- To match a substring within the text, set exact to false, like
  screen.getByText('llo Worl', {exact: false}).
- To perform a case-insensitive match, use screen.getByText('hello world',
  {exact: false}).

ex:

```
// Matching a string:
screen.getByText('Hello World') // full string match
screen.getByText('llo Worl', {exact: false}) // substring match
screen.getByText('hello world', {exact: false}) // ignore case
```

2. Regex Matching:

- To match a substring using a regular expression, use
  screen.getByText(/World/).
- To perform a case-insensitive regex match, use the /i flag, like
  screen.getByText(/world/i).
- To match the entire string with a regex, use ^ and $ anchors, like
  screen.getByText(/^hello world$/i).
- You can use regex patterns with optional characters, like W?, to match
  variations, as in screen.getByText(/Hello W?oRlD/i).

ex:

```
// Matching a regex:
screen.getByText(/World/) // substring match
screen.getByText(/world/i) // substring match, ignore case
screen.getByText(/^hello world$/i) // full string match, ignore case
screen.getByText(/Hello W?oRlD/i) // substring match, ignore case, searches for "hello world" or "hello orld"
```

3. Custom Function Matching:

- You can provide a custom function that accepts content (the text to match) and
  element (the element being tested) as arguments.
- The function should return true if there is a match and false if there is a
  mismatch.

ex:

```
// Matching with a custom function:
screen.getByText((content, element) => content.startsWith('Hello'))
```

4. Precision Options:

- The query APIs that accept TextMatch also accept an options object for
  controlling the precision of string matching.
- The exact option, which defaults to true, matches full strings with case
  sensitivity. When set to false, it matches substrings and is not
  case-sensitive.
- The exact option does not affect regex or function arguments.

5. Default Normalization:

- By default, DOM Testing Library automatically normalizes text before matching.
- The default normalization includes trimming whitespace from the start and end
  of the text.
- It also collapses multiple adjacent whitespace characters (such as newlines,
  tabs, and repeated spaces) within the string into a single space.

if you don't want to use then you can edit few setting in Default Normalization
or provide your custom Normalization function.

Default Normalization called (getDefaultNormalizer) with edit:

You can use the getDefaultNormalizer function to obtain the built-in normalizer
or adjust its behavior. getDefaultNormalizer takes an options object that allows
you to customize the normalization behavior.

The available options are:

- trim (default: true): Controls whether leading and trailing whitespace is
  trimmed.
- collapseWhitespace (default: true): Controls whether inner whitespace is
  collapsed into a single space.

ex:

```
screen.getByText('text', {
  normalizer: getDefaultNormalizer({ trim: false }),
})
```

custom Normalization function: By providing a custom normalizer function, you
can control how the text is normalized before matching.

ex:

```
screen.getByText('text', {
  normalizer: (str) =>
    getDefaultNormalizer({ trim: false })(str).replace(/[\u200E-\u200F]*/g, ''),
})
```

## ByLabelText

```
getByLabelText(
  // If you're using `screen`, then skip the container argument:
  container: HTMLElement,
  text: TextMatch,
  options?: {
    selector?: string = '*',
    exact?: boolean = true,
    normalizer?: NormalizerFn,
  }): HTMLElement
```

1. selector: HTML selector

ex:

```
const inputNode = screen.getByLabelText('Username', {selector: 'input'})

```

2. exact: used in textmatch where exect mean same to same text (same captail
   case and sansitivity)

## byplaceholder

```
getByPlaceholderText(
  // If you're using `screen`, then skip the container argument:
  container: HTMLElement,
  text: TextMatch,
  options?: {
    exact?: boolean = true,
    normalizer?: NormalizerFn,
  }): HTMLElement
```

## bytext

```
getByText(
  // If you're using `screen`, then skip the container argument:
  container: HTMLElement,
  text: TextMatch,
  options?: {
    selector?: string = '*',
    exact?: boolean = true,
    ignore?: string|boolean = 'script, style',
    normalizer?: NormalizerFn,
  }): HTMLElement
```

ignore: it take HTML selector as value which convay that there html node should
be ignored when selecting by text.

ex:

```
const element = screen.getByText('Valid Text', { ignore: 'style, script' });

```

## ByDisplayValue

```
getByDisplayValue(
  // If you're using `screen`, then skip the container argument:
  container: HTMLElement,
  value: TextMatch,
  options?: {
    exact?: boolean = true,
    normalizer?: NormalizerFn,
  }): HTMLElement
```

## ByAltText

```
getByAltText(
  // If you're using `screen`, then skip the container argument:
  container: HTMLElement,
  text: TextMatch,
  options?: {
    exact?: boolean = true,
    normalizer?: NormalizerFn,
  }): HTMLElement
```

## ByTitle

```
getByTitle(
  // If you're using `screen`, then skip the container argument:
  container: HTMLElement,
  title: TextMatch,
  options?: {
    exact?: boolean = true,
    normalizer?: NormalizerFn,
  }): HTMLElement
```

## ByTestId

```
getByTestId(
  // If you're using `screen`, then skip the container argument:
  container: HTMLElement,
  text: TextMatch,
  options?: {
    exact?: boolean = true,
    normalizer?: NormalizerFn,
  }): HTMLElement
```

# SEARCH VARIANTS:

In contrast to search types, there exist search variants as well. One of the
search variants in React Testing Library is getBy which is used for getByText or
getByRole. This is also the search variant which is used by default when testing
React components.

Two other search variants are queryBy and findBy; which both can get extended by
the same search types that getBy has access to. For example, queryBy

1. getBy : the getBy prefix is used when you expect the element to exist (throws
   an error if not found).

2. queryBy : The queryBy prefix is used when you expect the element to
   potentially not exist (returns null if not found)

3. findBy : the findBy search variants are asynchronous query methods that allow
   you to search for elements that may not be immediately present in the DOM. it
   wait for default timeout of 1000ms. These methods return promises that
   resolve when the desired element is found or reject after a specified
   timeout.

The findBy methods are useful when you are dealing with asynchronous operations,
such as data fetching or component updates, where the desired element may not be
immediately available in the DOM. They allow you to wait for the element to
appear asynchronously, ensuring that your tests account for delays in rendering
or state changes.

It's important to note that the findBy methods require your test environment to
support promises or be integrated with a testing framework that supports
asynchronous operations (e.g., Jest with async/await or done()).

```
import { render, screen } from '@testing-library/react';
import MyComponent from './MyComponent';

test('waits for an element with specific text to appear', async () => {
  render(<MyComponent />);

  const element = await screen.findByText('Hello, World');
  // Perform assertions or interact with the element...
});

```

as you can see we are using async fn() in our test case which allow your test to
be asyncronus and then you can use findByText method which currently doesn't
exist in the DOM but will be in the future. the value that we get from
screen.findByText('Hello, World') will be a promise that either be successful or
rejected. if it is successfull then you get the data otherwise it will be error.

note: getBy,queryBy,findBy will thow an error when find more then one element.  
checkout Summary Table to get the full picture:
https://testing-library.com/docs/queries/about

# multiple elements?

You have learned about the three search variants getBy, queryBy and findBy;
which all can be associated with the search types (e.g. Text, Role,
PlaceholderText, DisplayValue). If all of these search functions return only one
element, how to assert if there are multiple elements (e.g. a list in a React
component). All search variants can be extended with the All word:

getAllBy queryAllBy findAllBy

Whereas all of them return an array of elements and can be associated with the
search types again.

# Assertive Functions

Assertive functions are used in testing to verify certain conditions and make
assertions about the expected behavior of the code being tested. These functions
help ensure that the tested code behaves as expected and meets the desired
criteria.

In the context of testing with React Testing Library, assertive functions are
typically provided by the testing framework or assertion libraries used in
conjunction with React Testing Library. Some popular assertion libraries include
Jest, Chai, and Jasmine.

Assertive functions typically take one or more arguments and compare them to an
expected value or condition. If the assertion fails, an error or failure message
is raised, indicating that the test did not meet the expected criteria.

ex: expected().AssertiveFn()

# List of Assertive Functions from Jest Docs

1. toBeDisabled()

This allows you to check whether an element is disabled from the user's
perspective. According to the specification, the following elements can be
disabled: button, input, select, textarea, optgroup, option, fieldset, and
custom elements.

This custom matcher considers an element as disabled if the element is among the
types of elements that can be disabled (listed above), and the disabled
attribute is present. It will also consider the element as disabled if it's
inside a parent form element that supports being disabled and has the disabled
attribute present.

ex:

```
expect(getByTestId('button')).toBeDisabled() // Disabled
expect(getByTestId('input')).toBeDisabled()  // Disabled
expect(getByText('link')).not.toBeDisabled() // NOT Disabled
```

Note: This custom matcher does not take into account the presence or absence of
the aria-disabled attribute.

2. toBeEnabled()

This allows you to check whether an element is not disabled from the user's
perspective. It works like not.toBeDisabled(). Use this matcher to avoid double
negation in your tests.

3. toBeEmptyDOMElement()

it check if a selected element is empty, meaning it does not contain any child
elements or text content.If the element is empty the assertion will pass. If the
element has any content, the assertion will fail and display an error message.

This assertion is helpful when you want to verify that an element is
intentionally empty, such as a container that should not contain any content.

It's important to note that the .toBeEmptyDOMElement() assertion does not check
for the absence of attributes or specific properties. It focuses solely on the
absence of child elements or text content within the selected element.

4. toBeInTheDocument()

it check if a selected element is present in the document, meaning it exists in
the rendered DOM.

This assertion is commonly used to verify that specific elements are rendered
and accessible within the rendered component or page.

It's worth noting that the .toBeInTheDocument() assertion not only checks if the
element exists in the DOM but also ensures that it is visible (not hidden) and
not detached from the document. It provides a comprehensive check for the
presence and visibility of an element.

ex:

```
expect(getByTestId(document.documentElement, 'svg-element')).toBeInTheDocument() // presence check
expect(
  queryByTestId(document.documentElement, 'does-not-exist'),
).not.toBeInTheDocument() // not present check
```

5. .toBeInvalid()

it chack for

- If the element has an aria-invalid attribute with no value or a value of
  "true", it is considered invalid.
- If the result of the checkValidity() method on the element is false,
  indicating that the element fails its built-in validity checks, it is
  considered invalid.

side knowlage: checkValidity()

The checkValidity() method is a built-in method available on form elements in
JavaScript. It is used to check if the current value of the form element meets
its validity constraints based on HTML5 validation rules.

When called on an input, select, or textarea element, the checkValidity() method
returns a Boolean value indicating whether the element's value is valid (true)
or invalid (false) based on the defined validation rules.

The checkValidity() method takes into account the element's attributes such as
required, pattern, min, max, and others to perform validation checks.

6. toBeRequired()

This allows you to check if a form element is currently required.

An element is required if it is having a required or aria-required="true"
attribute.

This assertion is useful when testing form validation to ensure that specific
input fields are marked as required, indicating that the user must provide a
value before submitting the form.

It's important to note that the .toBeRequired() assertion focuses specifically
on the presence of the required attribute and does not validate the content or
behavior of the element. It verifies whether the required attribute is present
or not.

7. toBeValid()

This allows you to check if the value of an element, is currently valid.

An element is valid if it has no aria-invalid attributes or an attribute value
of "false". The result of checkValidity() must also be true if it's a form
element.

This assertion is useful when testing form validation to ensure that the
expected validation rules are applied correctly and that the input elements
behave as expected when valid data is entered.

It's important to note that the .toBeValid() assertion relies on the underlying
validation mechanisms in the browser, including HTML5 validation attributes or
custom validation logic. Therefore, it's essential to set up the necessary
validation conditions in your component or form for this assertion to work as
expected.

8. toBeVisible() This allows you to check if an element is currently visible to
   the user.

An element is visible if all the following conditions are met:

- it is present in the document
- it does not have its css property display set to none
- it does not have its css property visibility set to either hidden or collapse
- it does not have its css property opacity set to 0
- its parent element is also visible (and so on up to the top of the DOM tree)
- it does not have the hidden attribute
- if <details /> it has the open attribute

This assertion is useful when testing the visibility of elements, such as
checking if an element is displayed or hidden based on certain conditions or
user interactions.

It's important to note that the .toBeVisible() assertion verifies the visibility
of the element based on CSS styles, such as display: none, visibility: hidden,
or being outside the viewport. It does not consider the element's position or
the visibility of its parent elements. If an element is partially vis

9. toContainElement(element: HTMLElement | SVGElement | null)

This allows you to assert whether an element contains another element as a
descendant or not.

This assertion is useful when testing component compositions and ensuring that
the desired child elements are present within their parent components.

It's important to note that the .toContainElement() assertion only checks for
the presence of the child element within the parent element. It does not verify
the order or position of the child element, nor does it check for any specific
relationship between the parent and child elements.

ex:

```
  const parentElement = screen.getByTestId('parent');
  const childElement = screen.getByTestId('child');

  expect(parentElement).toContainElement(childElement);
```

10. .toContainHTML(valid HTML && htmlText: string)

it check if a selected element contains a specific HTML structure or markup.

This assertion is useful when testing component rendering and ensuring that the
desired HTML structure or markup is present within a particular element.

It's important to note that the .toContainHTML() assertion performs a strict
comparison of the HTML markup. It checks for an exact match, including the
element structure, attributes, and content. If there are any differences, such
as additional attributes or whitespace variations, the assertion will fail.

ex:

```
  const parentElement = screen.getByTestId('parent');
  expect(parentElement).toContainHTML('<div class="child">Hello, World!</div>');
```

11. toHaveAccessibleDescription(expectedAccessibleDescription?: string | RegExp)

it checks for the presence of the accessible description. It verifies if the
element has exect description value.

Describing:

- Describing by referencing content with aria-describedby.
- Describing tables and figures with captions.
- Descriptions derived from titles.

you can also partially matches a accessible description.

- FOR exect Matching: toHaveAccessibleDescription("Home page")
- FOR Partial Matching: toHaveAccessibleDescription(/Home/)
- FOR Partial Matching With Case insesitivity:
  toHaveAccessibleDescription(/home/i)

12. toHaveAccessibleName(expectedAccessibleName?: string | RegExp)

This allows you to assert that an element has the expected accessible name. It
is useful, for instance, to assert that form elements and buttons are properly
labelled.

Naming:

- Naming with child content.
- Naming with a string attribute via aria-label.
- Naming by referencing content with aria-labelledby.
- Naming form controls with the label element.
- Naming fieldsets with the legend element.
- Naming tables and figures with captions.
- Fallback names derived from titles and placeholders.
- alt text in case of Image

You can pass the exact string of the expected accessible name, or you can make a
partial match

- FOR exect Matching: toHaveAccessibleName("Home page")
- FOR Partial Matching: toHaveAccessibleName(/Home/)
- FOR Partial Matching With Case insesitivity: toHaveAccessibleName(/home/i)

13. toHaveAttribute(attr: string, value?: any)

This allows you to check whether the given element has an attribute or not. You
can also optionally check that the attribute has a specific expected value or
partial match.

ex:

```
<button data-testid="ok-button" type="submit" disabled>ok</button>

const button = getByTestId('ok-button')
expect(button).toHaveAttribute('disabled')
expect(button).toHaveAttribute('type', 'submit')
expect(button).not.toHaveAttribute('type', 'button')
```

14. toHaveClass(...classNames: string[], options?: {exact: boolean})

This allows you to check whether the given element has certain classes within
its class attribute.

You must provide at least one class, unless you are asserting that an element
does not have any classes.

ex:

```
<button data-testid="delete-button" class="btn extra btn-danger">
  Delete item
</button>
const deleteButton = getByTestId('delete-button')

expect(deleteButton).toHaveClass('extra')
expect(deleteButton).toHaveClass('btn-danger', 'btn')
```

15. toHaveFocus() This allows you to assert whether an element has focus or not.

ex:

```
<div><input type="text" data-testid="element-to-focus" /></div>
const input = getByTestId('element-to-focus')

input.focus()
expect(input).toHaveFocus()
```

16. toHaveFormValues(expectedValues: { [name: string]: any })

to check value of form control(input). which input you are accessing is based on
input name attribute and you can validate it's value. different input has
different value type so here's the list:

- <input type="number"> elements return the value as a number, instead of a
  string.
- <input type="checkbox"> elements:
  - if there's a single one with the given name attribute, it is treated as a
    boolean, returning true if the checkbox is checked, false if unchecked.
  - if there's more than one checkbox with the same name attribute, they are all
    treated collectively as a single form control, which returns the value as an
    array containing all the values of the selected checkboxes in the
    collection.
- <input type="radio"> elements are all grouped by the name attribute, and such
  a group treated as a single form control. This form control returns the value
  as a string corresponding to the value attribute of the selected radio button
  within the group.
- <input type="text"> elements return the value as a string. This also applies
  to <input> elements having any other possible type attribute that's not
  explicitly covered in different rules above (e.g. search, email, date,
  password, hidden, etc.)
- <select> elements without the multiple attribute return the value as a string
  corresponding to the value attribute of the selected option, or undefined if
  there's no selected option.
- <select multiple> elements return the value as an array containing all the
  values of the selected options.
- <textarea> elements return their value as a string. The value corresponds to
  their node content.

The above rules make it easy, for instance, to switch from using a single select
control to using a group of radio buttons. Or to switch from a multi select
control, to using a group of checkboxes. The resulting set of form values used
by this matcher to compare against would be the same.

# Note:

It is important to stress that this matcher can only be invoked on a form or a
fieldset element.

This allows it to take advantage of the .elements property in form and fieldset
to reliably fetch all form controls within them.

This also avoids the possibility that users provide a container that contains
more than one form, thereby intermixing form controls that are not related, and
could even conflict with one another.

ex:

```
<form data-testid="login-form">
  <input type="text" name="username" value="jane.doe" />
  <input type="password" name="password" value="12345678" />
  <input type="checkbox" name="rememberMe" checked />
  <button type="submit">Sign in</button>
</form>

expect(getByTestId('login-form')).toHaveFormValues({
  username: 'jane.doe',
  rememberMe: true,
})
```

here we are checking:

- input that has a name attribute username should have a value of jane.deo.
  (input type text)
- input that has a name attribute rememberMe should have a value of True. (input
  type checkbox)

17. toHaveStyle(css: string | object)

This allows you to check if a certain element has some specific css properties
with specific values applied. It matches only if the element has all the
expected properties applied, not just some of them.

This also works with rules that are applied to the element via a class name for
which some rules are defined in a stylesheet currently active in the document.
The usual rules of css precedence apply.

ex:

```
<button
  data-testid="delete-button"
  style="display: none; background-color: red"
>
  Delete item
</button>


const button = getByTestId('delete-button')

expect(button).toHaveStyle('display: none')
expect(button).toHaveStyle({
  backgroundColor: 'red',
  display: 'none',
})

expect(button).not.toHaveStyle({
  backgroundColor: 'blue',
  display: 'none',
})
```

18. toHaveTextContent(text: string | RegExp, options?: {normalizeWhitespace:
    boolean})

it checks if an element has the expected text content. It verifies that the text
content of the element matches the provided string or regular expression. This
supports elements, but also text nodes and fragments.

When a string argument is passed through, it will perform a partial
case-sensitive match to the node content.(if text content contain same text
(with the same sensitivity) text can be complete text content of the element or
a portition)

To perform a case-insensitive match, you can use a RegExp with the /i modifier.

If you want to match the whole content, you can use a RegExp to do it.

ex:

```
test('renders a message containing a specific word', () => {
  render(<Message>Welcome to the website</Message>);
  const messageElement = screen.getByTestId('message');

  expect(messageElement).toHaveTextContent(/welcome/i);
});
```

In this case, the regular expression /welcome/i is used to match the text
content of the messageElement. The i flag makes the match case-insensitive,
allowing it to match both "Welcome" and "welcome".

The toHaveTextContent() matcher is useful for verifying the expected text
content of elements such as headings, paragraphs, buttons, or any other element
that contains visible text.

19. toHaveValue(value: string | string[] | number)

toHaveValue() checks the actual value attribute of the element

toHaveValue check value of form inputs like (<input>, <select> and <textarea>)
this is same as toHaveFormValues the only differece here is in toHaveFormValues
we select the form element in toHaveValue we select each individual input and
check there values.

This allows you to check whether the given form element has the specified value.
It accepts <input>, <select> and <textarea> elements with the exception of
<input type="checkbox"> and <input type="radio">, which can be meaningfully
matched only using toBeChecked or toHaveFormValues.

For all other form elements, the value is matched using the same algorithm as in
toHaveFormValues does.

ex:

```
<input type="text" value="text" data-testid="input-text" />

const textInput = getByTestId('input-text')
expect(textInput).toHaveValue('text')
```

20. toHaveDisplayValue(value: string | RegExp | (string|RegExp)[])

this is similer to toHaveValue here also we check value but toHaveDisplayValue()
considers the displayed value as perceived by the user.

For instance, if a user has typed in an input field, the displayed value may
differ from the initial value or the value attribute until the input is
committed. In such cases, toHaveDisplayValue() can be used to verify the
displayed value, while toHaveValue() checks the underlying value attribute.

toHaveDisplayValue() takes into account the displayed value as perceived by the
user, considering user interactions and updates.

ex:

```
<label for="input-example">First name</label>
<input type="text" id="input-example" value="Luca" />

const input = screen.getByLabelText('First name')
expect(input).toHaveDisplayValue('Luca')

```

21. toBeChecked()

This allows you to check whether the given element is checked. It accepts an
input of type checkbox or radio and elements with a role of checkbox, radio or
switch with a valid aria-checked attribute of "true" or "false".

ex:

```
<input type="checkbox" checked data-testid="input-checkbox-checked" />

const inputCheckboxChecked = getByTestId('input-checkbox-checked')
expect(inputCheckboxChecked).toBeChecked()
```

22. toBePartiallyChecked()

This allows you to check whether the given element is partially checked. It
accepts an input of type checkbox and elements with a role of checkbox with a
aria-checked="mixed", or input of type checkbox with indeterminate set to true.

The toBePartiallyChecked() matcher is specifically designed for scenarios where
checkboxes have an indeterminate state, where neither the checked nor unchecked
state is explicitly set. It is useful when you want to verify the indeterminate
state of a checkbox in your tests.

Please note that the toBePartiallyChecked() matcher is not applicable to radio
buttons or other form elements. It is meant specifically for checkboxes that
have an indeterminate state.

23. toHaveErrorMessage(text: string | RegExp)

This allows you to check whether the given element has an ARIA error message or
not.

Use the aria-errormessage attribute to reference another element that contains
custom error message text (ex: span that contain an id to connext the Aria error
and the span that contain error text). Multiple ids is NOT allowed. Authors MUST
use aria-invalid in conjunction with aria-errormessage. Learn more from
aria-errormessage spec.

Whitespace is normalized.

When a string argument is passed through, it will perform a whole case-sensitive
match to the error message text.

To perform a case-insensitive match, you can use a RegExp with the /i modifier.

To perform a partial match, you can pass a RegExp or use
expect.stringContaining("partial string").

ex:

```
<label for="startTime"> Please enter a start time for the meeting: </label>
<input
  id="startTime"
  type="text"
  aria-errormessage="msgID"
  aria-invalid="true"
  value="11:30 PM"
/>
<span id="msgID" aria-live="assertive" style="visibility:visible">
  Invalid time: the time must be between 9:00 AM and 5:00 PM
</span>

const timeInput = getByLabel('startTime')

expect(timeInput).toHaveErrorMessage(
  'Invalid time: the time must be between 9:00 AM and 5:00 PM',
)
expect(timeInput).toHaveErrorMessage(/invalid time/i) // to partially match
expect(timeInput).toHaveErrorMessage(expect.stringContaining('Invalid time')) // to partially match
expect(timeInput).not.toHaveErrorMessage('Pikachu!')
```

here you can see first we selected the input then checked that it contain the
aria-errormessage="msgID" which link to <span id="msgID"> and we confirm the
error message.

### user action

# fireEvent

use userEvent over fireEvent is recommended but you should know about fireEvent
in case you need it.

Firing events refers to the process of simulating user interactions with the
components being tested. This allows you to test how the components respond to
different user actions, such as clicking buttons, typing in input fields, or
submitting forms.

React Testing Library provides various methods to fire events, depending on the
type of interaction you want to simulate. Here are some commonly used methods:

1. fireEvent.click(element): This method simulates a user clicking on the
   specified element, such as a button or a link.

2. fireEvent.change(element, { target: { value: 'new value' } }): This method
   simulates a user changing the value of an input element. It is often used to
   test input fields.

3. fireEvent.submit(element): This method simulates a user submitting a form,
   typically triggered by clicking a submit button or pressing the Enter key.

4. fireEvent.keyDown(element, { key: 'Enter', code: 'Enter' }): This method
   simulates a user pressing a specific keyboard key on the specified element.
   In this example, it simulates pressing the Enter key.

5. fireEvent.mouseOver(element): This method simulates a user moving the mouse
   pointer over the specified element.

6. fireEvent.focus(element): This method simulates a user giving focus to the
   specified element, such as clicking on an input field.

These are just a few examples of the event-firing methods provided by React
Testing Library. The library offers many more methods to simulate various user
interactions.

# userEvent

userEvent provides a more high-level and intuitive API compared to fireEvent,
making it easier to write tests that simulate various user interactions.

# Note: all useEvent are async in nature

# Differences from fireEvent

fireEvent dispatches DOM events, whereas user-event simulates full interactions,
which may fire multiple events and do additional checks along the way.

Testing Library's built-in fireEvent is a lightweight wrapper around the
browser's low-level dispatchEvent API, which allows developers to trigger any
event on any element. The problem is that the browser usually does more than
just trigger one event for one interaction. For example, when a user types into
a text box, the element has to be focused, and then keyboard and input events
are fired and the selection and value on the element are manipulated as they
type.

user-event allows you to describe a user interaction instead of a concrete
event. It adds visibility and interactability checks along the way and
manipulates the DOM just like a user interaction in the browser would. It
factors in that the browser e.g. wouldn't let a user click a hidden element or
type in a disabled text box. This is why you should use user-event to test
interaction with your components.

There are, however, some user interactions or aspects of these that aren't yet
implemented and thus can't yet be described with user-event. In these cases you
can use fireEvent to dispatch the concrete events that your software relies on.

summary: (userEvent also take browser behaviour into acount with the user
behaviour but fireEvent only take acount the user behaviour and fireEvent only
trigger one event for click but userEvent trigger multiple event for click like
mouseDown,mouseUp,click so useEvent make sure all the possible event get taken
care of when we choose a specific event).

# you have to install userEvent externaly

userEvent is a high level api that work in conjuction with rtl and it can work
with any library that have a dom to access.

npm install --save-dev @testing-library/user-event

# Writing tests with userEvent

We recommend invoking userEvent.setup() before the component is rendered. This
can be done in the test itself, or by using a setup function.

```
// inlining
test('trigger some awesome feature when clicking the button', async () => {
  const user = userEvent.setup()
  render(<MyComponent />)

  await user.click(screen.getByRole('button', {name: /click me!/i}))

  // ...assertions...
})
```

# Setup

When users interact in the browser by e.g. pressing keyboard keys, they interact
with a UI layer the browser shows to them. The browser then interprets this
input, possibly alters the underlying DOM accordingly and dispatches trusted
events. The UI layer and trusted events are not programmatically available.
Therefore user-event has to apply workarounds and mock the UI layer to simulate
user interactions like they would happen in the browser.

-# Starting a session per setup()

setup(options?: Options): UserEvent

The userEvent.setup() API applies these workarounds to the document and allows
you to configure an "instance" of user-event. Methods on this instance share one
input device state, e.g. which keys are pressed.

This allows to write multiple consecutive interactions that behave just like the
described interactions by a real user.

```
import userEvent from '@testing-library/user-event'

const user = userEvent.setup()

await user.keyboard('[ShiftLeft>]') // Press Shift (without releasing it)
await user.click(element) // Perform a click with `shiftKey: true`
```

# userEvent.setup(options?: Options)

Note: i'm currently not able to understand the syntex so here i have only
explained the concept of the option (which option do what)

The following options allow to adjust the behavior of user-event APIs. They can
be applied per setup(). all the instances of that user.setup will able to
utilize the changes. made by options

1. advanceTimers

- advanceTimers is used in conjection with usefaketime.
- advanceTimers is generally used for those event that are time dependent like
  click, async request, setTimeout etc.
- the use of advanceTimers is to simulate that certain time has been passed
  (advanced) that you define in advanceTimers fn in milli-second. code that run
  after the advanceTimers fn line will act as time as actually passed and now
  you can access that event like click which have changed after time.

- The value should be a function that returns a promise or void.
- Default: () => Promise.resolve() ex:

```
import userEvent from '@testing-library/user-event'
import { advanceTimersByTime } from 'jest-preset-angular'

jest.useFakeTimers()

const user = userEvent.setup({
  advanceTimers: advanceTimersByTime,
})

test('test with timers', async () => {
  const element = screen.getByRole('button')

  user.click(element)

  // Advance timers by 1000 milliseconds
  await user.advanceTimers(1000)
- from here all code will work in asumption of that 1000 milisecond has been passed

  // Assertions or further interactions
})

```

2. applyAccept

When using the upload() function to simulate file uploads, it can automatically
discard files that don't match the accept property if it exists on the target
element.

By default, the applyAccept option is set to true, which means that if the
element has an accept property specified, user-event will automatically filter
and discard files that do not match the specified file types when performing a
file upload simulation.

ex

```
import userEvent from '@testing-library/user-event'

const user = userEvent.setup({
  applyAccept: true,
})

test('test file upload', async () => {
  const input = screen.getByLabelText('Upload File')

  await user.upload(input, [
    { name: 'file1.txt', type: 'text/plain', contents: 'File 1 contents' },
    { name: 'image.png', type: 'image/png', contents: 'image data' },
    { name: 'file2.txt', type: 'text/plain', contents: 'File 2 contents' },
  ])

  // Assertions or further interactions
})

```

In this example, the applyAccept option is set to true. If the input element has
an accept attribute, user-event will automatically filter out any files in the
upload simulation that do not match the specified file types. This ensures that
only files of the allowed types will be included in the upload action.

If the applyAccept option is set to false, user-event will not filter the files
based on the accept property, and all specified files will be included in the
upload action, regardless of their file types.

It's worth noting that the applyAccept option only affects the behavior of the
upload() function. Other user-event functions, such as click(), type(), or
keyboard(), are not affected by this option.

3. autoModify

- In the future, userEvent intends to automatically apply modifier keys for
  printable characters. For example, typing 'A' might imply pressing Shift and
  'a' if caps lock is not active.
- This option allows you to opt out of this feature.
- Default: true (meaning changes made by shift + or capslock behaviour will
  change the output)
- Example: userEvent.setup({ autoModify: true })

"if you have provided a text that contain some capital latter and some small
later and you have set automodify to be true then userEvent will act as you have
used either hold shift key to capitalize the latter or caps lock key when
simulating typing actions. which simulate the user behavior more accurately "
but automodify is currently in development check docs in future for more info.

4. delay

By default, the delay option is set to 0, indicating that there is no delay
between subsequent inputs. This means that when you simulate user interactions,
such as typing a series of characters, the events are dispatched without any
delay in between.

However, you can customize the delay between subsequent inputs by setting the
delay option to a specific value in milliseconds. For example, setting delay to
100 would introduce a 100-millisecond delay between each input event during a
user interaction.

The purpose of introducing a delay is to mimic more realistic user behavior and
allow other asynchronous code to run between events. It simulates the natural
pauses or intervals that may occur when a user interacts with an application.

Ex:

```
import userEvent from '@testing-library/user-event'

const user = userEvent.setup({
  delay: 100,
})

test('test typing with delay', async () => {
  const input = screen.getByRole('textbox')

  await user.type(input, 'Hello')

  // There will be a 100-millisecond delay between each character typed
})
```

In this example, the delay option is set to 100. When typing the text "Hello"
using user.type(), there will be a 100-millisecond delay between typing each
character. This delay allows other asynchronous code to run between the events,
simulating a more realistic user interaction.

It's important to note that the delay option can be useful in certain testing
scenarios where you want to account for timing-related behaviors or when dealing
with code that relies on specific timing conditions. However, in most cases, the
default value of 0 is sufficient for simulating user interactions.

5. Document

- Specifies the document to use for user events. If an API is called directly
  with an element and without using setup(), it defaults to the owner document
  of that element. Otherwise, it falls back to the global document.
- Default: element.ownerDocument ?? globalThis.document
- Example: userEvent.setup({ document: customDocument })

```
import userEvent from '@testing-library/user-event'

 render (<globalDocument/>)
const customDocument = document.implementation.createHTMLDocument()
const input = customDocument.createElement('input')
customDocument.body.appendChild(input)

userEvent.type(input, 'Hello', { document: customDocument })
```

document option is given if you want to provide a custom document to test your
userEvent in isolation to that document not to the global document . if not then
default document is the render global object that we use to render the document.

6. keyboardMap

The keyboardMap array should contain the keys that make up the keyboard device.
Each key is represented as a string, and you can define the keys based on your
requirements. The order of the keys in the array defines the layout of the
keyboard device.

Default: A "standard" US-104-QWERTY keyboard.

This can be useful when working with non-standard keyboard layouts or when you
want to simulate interactions specific to a particular language or region.

ex:

```
import userEvent from '@testing-library/user-event'

const customKeyboardMap = ['A', 'B', 'C'] // Custom keyboard layout with only three keys: A, B, C

userEvent.type(input, 'ABC', { keyboardMap: customKeyboardMap })

// The 'input' element will receive the value 'ABC' based on the custom keyboard layout

```

7. pointerEventsCheck

The pointerEventsCheck option is used to control the frequency of checking for
the pointer-events: none CSS property on elements during user interactions
simulated by the user-event library.

The pointer-events CSS property controls whether an element can be the target of
pointer events such as click, hover, or drag. When the pointer-events property
is set to none on an element or any of its ancestors, it effectively disables
interaction with that element.

By default, the user-event library performs checks for the pointer-events: none
property on elements to ensure that simulated user interactions only occur on
elements that are actually interactable. However, these checks can be
computationally expensive, especially when dealing with deeply nested or complex
DOM structures.

The pointerEventsCheck option allows you to configure the frequency of these
checks during user interactions. It accepts the following values:

- PointerEventsCheckLevel.Never: Disables all pointer-events checks. No checks
  for the pointer-events: none property will be performed during user
  interactions. This can provide a performance boost but may result in
  interactions with elements that are actually not interactable.

- PointerEventsCheckLevel.EachTarget: This option performs the pointer-events
  check once for each event target during user interactions. It checks if the
  target element or any of its ancestors have pointer-events: none applied. This
  option ensures accurate handling of pointer-events but may impact performance,
  especially when dealing with deeply nested elements.

- PointerEventsCheckLevel.EachApiCall: This option performs the pointer-events
  check once per API call in user-event. It checks for the pointer-events: none
  property before triggering each user interaction event, such as click, hover,
  or focus. This option strikes a balance between accuracy and performance,
  reducing the number of checks compared to PointerEventsCheckLevel.EachTarget.
  (Default)

(it means that the user-event library performs a check for the pointer-events:
none CSS property before triggering each user interaction API call.

In other words, before simulating a user interaction such as a click, hover, or
focus using user-event, it checks if the target element or any of its ancestors
have the pointer-events: none property applied. If the property is found,
indicating that the element or its ancestors are set to ignore pointer events,
user-event will skip triggering the interaction on that element.)

- PointerEventsCheckLevel.EachTrigger: Checks the pointer-events: none property
  on every user interaction that triggers a set of events (e.g., releasing a
  mouse button triggering pointerup, mouseup, click, etc.). This option provides
  the highest level of accuracy but can significantly impact performance.

ex:

```
import userEvent from '@testing-library/user-event'

userEvent.click(element, { pointerEventsCheck: PointerEventsCheckLevel.Never })
// Simulates a click on the 'element' without performing any 'pointer-events' checks
```

In this example, the click interaction is performed on the element, but the
pointer-events checks are disabled by setting the pointerEventsCheck option to
PointerEventsCheckLevel.Never. This means the click interaction will occur
regardless of the pointer-events property on the element or its ancestors.

It's important to note that disabling the pointer-events checks can lead to
interactions with elements that may not be intended to be interacted with,
potentially resulting in inaccurate test results. The choice of the
pointerEventsCheck option should be based on the specific testing requirements
and the trade-off between performance and accuracy.

By default, the PointerEventsCheckLevel.EachTarget option is recommended as it
strikes a balance between performance and accuracy for most testing scenarios.

# Side note on pointer-events: none CSS property

The CSS property pointer-events: none is used to disable pointer events on an
element and its descendants. When applied to an element, it prevents the element
from being the target of any mouse events, such as clicks, hovers, or drags.

When an element has pointer-events: none, it effectively becomes "transparent"
to pointer events. Any pointer events that occur on the element will "pass
through" to the element below it in the DOM hierarchy, as if the element doesn't
exist.

8. pointerMap

An array of available pointer keys. It allows you to simulate different pointer
devices. Default: A simple mouse and a touchscreen. Example: userEvent.setup({
pointerMap: myCustomPointerMap })

ex:

```
import userEvent from '@testing-library/user-event';

const customPointerMap = ['mouse', 'touch']; // Custom pointer map with two devices

const user = userEvent.setup({ pointerMap: customPointerMap });

// Simulate a click using the "mouse" pointer device
await user.click(element, { pointer: 'mouse' });

// Simulate a touch using the "touch" pointer device
await user.click(element, { pointer: 'touch' });

```

9. skipAutoClose

The skipAutoClose option allows you to control the behavior of releasing any
keys that are still pressed at the end of the type() call.

By default, when you use userEvent.type() to simulate typing text into an input
field, any keys that were pressed during the typing process are automatically
released at the end of the call. This behavior replicates the typical behavior
of a user releasing keys after typing.(Default = skipAutoClose: false)

However, with the skipAutoClose option, you can choose to opt out of this
automatic key release feature. By setting skipAutoClose to true, you prevent
userEvent.type() from releasing any keys that were pressed during the typing.

Ex:

```
import userEvent from '@testing-library/user-event';

// Typing with automatic key release
userEvent.type(inputElement, 'Hello, World!');

// Typing without automatic key release
userEvent.type(inputElement, 'Hello, World!', { skipAutoClose: true });
```

In the first userEvent.type() call, without specifying the skipAutoClose option,
the keys pressed during the typing ('Hello, World!') will be automatically
released at the end of the call.

In the second userEvent.type() call, with skipAutoClose set to true, the keys
will not be automatically released. This can be useful in scenarios where you
want to retain the pressed state of certain keys after the typing is complete.(h
hold + e hold + l hold + l hold)

By providing the skipAutoClose option, you have fine-grained control over
whether the keys should be automatically released or not, allowing you to
simulate specific typing behaviors more accurately in your tests.

Here are a few situations where the skipAutoClose option might be useful:

- Testing key combinations: If you need to test a specific key combination, such
  as Ctrl+A or Shift+Enter, and you want to assert the behavior when those keys
  are held down together, you can use skipAutoClose to prevent the automatic
  release of the keys after typing. This allows you to verify how the
  application handles the combination of pressed keys.

- Testing keyboard shortcuts: In some applications, keyboard shortcuts may
  require multiple keys to be pressed simultaneously. By using skipAutoClose,
  you can simulate the pressing of multiple keys and keep them held down to test
  the expected behavior of the keyboard shortcut.

- Testing complex typing scenarios: In certain cases, you may have complex
  typing scenarios where you want to simulate a sequence of key presses and
  releases without any automatic key release in between. By using skipAutoClose,
  you can ensure that all the keys remain pressed until explicitly released,
  allowing you to test specific typing sequences accurately.

- Custom key interaction testing: If you are testing a specific interaction that
  involves pressing and holding down a key for an extended period, you can
  utilize skipAutoClose to keep the key pressed for the desired duration and
  observe the resulting behavior of the application.

10. skipClick

type() implies a click on the element (to focus on input before start typing).
This option allows to opt out of this feature.

default: false

when you set skipClick to true, it skips the initial click event used for
focusing the element. This means that the element will not be clicked before
typing, and the focus will not be explicitly set through a click event.

So, setting skipClick to true means opting out of the initial click event used
to focus the element before typing, allowing you to simulate typing without
triggering the click event for focusing.

11. skipHover

click() implies moving the cursor to the target element first. This option
allows to opt out of this feature.

default: false

when we click the curser first hoverover the element before clicking it. this
feature allow you to click on the element without hovering over the element.

12. writeToClipboard

the "writeToClipboard" option is related to the behavior of the cut() and copy()
functions in user-event.

By default, the Clipboard API is usually not available in test environments.
When you call the cut() or copy() functions in user-event, the library replaces
the navigator.clipboard property with a stub implementation to simulate the
behavior of writing selected data to the Clipboard API.

The "writeToClipboard" option controls this behavior, and its default value
depends on how you invoke the user-event APIs:

- When calling the APIs directly (without using userEvent.setup()), the default
  value is false. This means that by default, the selected data is not written
  to the Clipboard API when using cut() or copy().

- When calling the APIs on an instance returned by userEvent.setup(), the
  default value is true. This means that by default, the selected data is
  written to the Clipboard API when using cut() or copy().

So, depending on how you use the user-event APIs and the value of the
"writeToClipboard" option, you can control whether the selected data is written
to the Clipboard API or not.

# Pointer

This Pointer function allows you to simulate interactions with pointer devices,
such as mouse or touch input.

The pointer() function accepts a single pointer action or an array of pointer
actions. A pointer action can be either a press action or a move action.

1. Press Action: A press action is performed by pressing a button or touching
   the screen. It is defined by specifying the keys to be pressed, released, or
   both.

For example:

- pointer({ keys: '[MouseLeft]' }): Presses the left mouse button.
- pointer('[MouseLeft][mouseright]'): Presses the left and right mouse buttons
  at the same time.
- pointer('[MouseLeft>]'): Presses and holds the left mouse button.
- pointer('[/MouseLeft]'): Releases the left mouse button.

Multiple press actions can be declared at once, and they will be resolved to
multiple actions internally.

Which buttons are available depends on the pointerMap.

2. Move Action:

A move action describes a pointer movement. It is used to move the pointer to a
specific location.

```
pointer([
  { keys: '[TouchA>]', target: element1 },  // Touch the screen at element1
  { pointerName: 'TouchA', target: element2 },  // Move the touch pointer to element2
  { keys: '[/TouchA]' },  // Release the touch pointer at the last position (element2)
])
```

Note: TouchA is not a reserved key it is decleared by the auther

The pointerName property is used to specify which pointer is being moved. By
default, it is set to "mouse" for the mouse pointer. For touch pointers, the
name corresponds to the button used in the press action.

- PointerTaret

```
interface PointerTarget {
  target: Element
  coords?: PointerCoords
}
```

The PointerTarget interface is used to describe the position of the pointer on
the document. It consists of the target property, which should be the element
From the DOM (your pointer will be on the target directly and then you can
perform some action), and the optional coords property to specify the pointer
coordinates (use coord to specify location where you want your pointer to be).

- SelectionTarget

```
interface SelectionTarget {
  node?: Node
  offset?: number
}
```

- node: This property represents the DOM node that you want to interact with. It
  could be an element, text node, or any other valid node in the DOM tree.

- offset: The offset property is used to specify the displacement or position
  within the node where the pointer action should occur. It represents the
  character offset or position within the text content of the node.(positon with
  in node and offset in that from that offset you can perform actions)

Ex:

```
pointer({ node: element, offset: 1, keys: '[MouseLeft]' })
```

In this example, element represents the DOM node you want to interact with, and
the offset of 1 indicates that the action should occur one character position
after the specified node. This can be useful when simulating pointer actions
that involve text selection within a specific DOM node.

# Keyboard

read the docs ease of understand :
https://testing-library.com/docs/user-event/keyboard

# Clipboard

read the docs ease of understand :
https://testing-library.com/docs/user-event/clipboard

# Convenience APIs

you should use convenience API over keyboad and mouse API described above;

read docs for ease of understanding :
https://testing-library.com/docs/user-event/convenience

# Utility APIs

The following APIs don't have one-to-one equivalents in a real user interaction.
Their behavior is therefore an interpretation how the "perceived" user
interaction might be translated to actual events on the DOM.

1. upload()

```
upload(
    element: HTMLElement,
    fileOrFiles: File | File[],
): Promise<void>


- element: The HTML element representing the file input element that the user will interact with.
- fileOrFiles: The file or an array of files to be uploaded. It can be a File object or an array of File objects.

file can be an file object like this :
const file = new File(['hello'], 'hello.png', { type: 'image/png' });

file can be an array of file object like this :
const file = new file([{ name: 'file1.txt', type: 'text/plain', contents: 'File 1 contents' },
    { name: 'image.png', type: 'image/png', contents: 'image data' },
    { name: 'file2.txt', type: 'text/plain', contents: 'File 2 contents' }])

```

Change a file input as if a user clicked it and selected files in the resulting
file upload dialog.

Files that don't match an accept property (files that we declear) will be
automatically discarded, unless applyAccept is set to false.

2. Type()

The type() function provided by userEvent in React Testing Library allows you to
simulate typing text into an input element. It has the following signature:

```
type(
  element: Element,
  text: KeyboardInput,
  options?: {
    skipClick?: boolean,
    skipAutoClose?: boolean,
    initialSelectionStart?: number,
    initialSelectionEnd?: number
  }
): Promise<void>
```

Let's go through each parameter and option:

- element: The input element or textarea element that you want to type into.
- text: The text that you want to type into the element. It can be a string or a
  KeyboardInput type, which represents a sequence of keyboard inputs. For
  example, you can use userEvent.type(input, 'Hello, World!') to type the text
  "Hello, World!" into the input element.
- options (optional): An object that allows you to customize the behavior of the
  type() function.

- skipClick (default: false): By default, type() clicks the element before
  typing into it. Setting skipClick to true skips the click action.
- skipAutoClose (default: false): By default, type() releases all pressed keys
  at the end of the call. Setting skipAutoClose to true skips this automatic
  release of keys.
- initialSelectionStart (default: undefined): If set, this specifies the
  starting position of the text selection within the input element before
  typing.
- initialSelectionEnd (default: undefined): If set, this specifies the ending
  position of the text selection within the input element before typing. If
  initialSelectionEnd is not set, a collapsed selection will be set at the
  initialSelectionStart position.

# Note

By default, the function will click the element before typing into it, so you
don't need to manually focus the element first. It will also release all pressed
keys after typing unless you set skipAutoClose to true.

3. clear

```
clear(element: Element): Promise<void>
```

clear(): This API allows you to easily clear the contents of an editable
element, such as an <input> or <textarea>.

When called, it performs the following actions:

- Sets the focus on the specified element.
- Selects all the contents of the element using the browser's menu.
- Deletes the selected contents using the browser's menu.

Here's an example usage of clear():

```
render(<textarea defaultValue="Hello, World!" />)
await userEvent.clear(screen.getByRole('textbox'))
expect(screen.getByRole('textbox')).toHaveValue('')
```

In this example, the clear() function is used to clear the contents of a
<textarea> element. After calling clear(), the element should have an empty
value.

4. selectOptions(), deselectOptions()

```
selectOptions(
    element: Element,
    values: HTMLElement | HTMLElement[] | string[] | string,
): Promise<void>
deselectOptions(
    element: Element,
    values: HTMLElement | HTMLElement[] | string[] | string,
): Promise<void>
```

selectOptions(): This API is used to select one or multiple options in an
<select> element or listbox. It accepts the following parameters:

- element: The target <select> element or listbox.
- values: The options to be selected. It can be specified using the option's
  value, HTML content, or the option element itself. You can pass a single value
  or an array of values.

```
----- Before render ------------------------
render(
  <select multiple>
    <option value="1">A</option>
    <option value="2">B</option>
    <option value="3">C</option>
  </select>
)

-------------------------------------------

await userEvent.selectOptions(screen.getByRole('listbox'), ['1', 'C'])

----- after render ---------------------------(not part of code, just for understanding )
render(
  <select multiple>
    <option value="1">A</option>
    <option value="2" selected>B</option>
    <option value="3">C</option>
  </select>
)
-------------------------------------------
expect(screen.getByRole('option', { name: 'A' }).selected).toBe(true)
expect(screen.getByRole('option', { name: 'B' }).selected).toBe(false)
expect(screen.getByRole('option', { name: 'C' }).selected).toBe(true)
```

similarly with deselectOptions() to deselect option from list

### useing Fake timer

Using fake timers in testing allows you to control and manipulate time-related
operations, such as timers, timeouts, intervals, and animations, within your
test environment. Instead of waiting for real time to elapse, you can manually
control the progression of time, making your tests more deterministic and
efficient.

```
- inside jest you can do this to allow faketime to run before every test.

// Fake timers using Jest
beforeEach(() => {
  jest.useFakeTimers()
})


- inside jest you can do this to allow faketime to stop and it is more like a cleanup fn.
  to make you testing app sink with real time after test is over.

// Running all pending timers and switching to real timers using Jest
afterEach(() => {
  jest.runOnlyPendingTimers()
  jest.useRealTimers()
})
```

# Async Methods

Several utilities are provided for dealing with asynchronous code. These can be
useful to wait for an element to appear or disappear in response to an event,
user action, timeout, or Promise.

These methods are helpful when you need to wait for certain conditions or
changes to occur in the DOM before proceeding with your tests.

The async methods return Promises, so be sure to use await or .then when calling
them.

1. findBy

findBy methods are a combination of getBy queries and waitFor. They accept the
waitFor options as the last argument (e.g. await screen.findByText('text',
queryOptions, waitForOptions)).

Note: query option there are different type of query like
byRole,byLabelText,byPlaceholderText etc and each query can take some aditional
options. if we are using findByRole then we can use byRole option on findBy
menthod. similarly there are waitForOption that are used with waitFor method
those can also be used with findBy method.

findBy queries work when you expect an element to appear but the change to the
DOM might not happen immediately.

findBy is specifically designed for waiting for elements to appear in the DOM
and is often used in conjunction with asynchronous actions or events that
trigger changes in the DOM. It is a convenience method that combines the getBy
queries with the waitFor functionality. It is particularly useful when you
expect an element to appear asynchronously and want to perform assertions or
further actions on that element once the element has appeard.

2. waitFor

```
function waitFor<T>(
  callback: () => T | Promise<T>,
  options?: {
    container?: HTMLElement
    timeout?: number
    interval?: number
    onTimeout?: (error: Error) => Error
    mutationObserverOptions?: MutationObserverInit
  },
): Promise<T>
```

- callback: A function that is called repeatedly until it returns a truthy value
  or throws an error. It can also return a promise that resolves to a truthy
  value or rejects with an error.

- options (optional): An object containing additional options for the waitFor
  method: -- container (default: document): The DOM element within which the
  waitFor method should search for changes. The elements you are waiting for
  should be descendants of the container element. -- timeout (default: 1000ms):
  The maximum amount of time to wait for the condition to be met, in
  milliseconds. -- interval (default: 50ms): The time interval between each
  retry of the callback function. -- onTimeout (default: appends container's
  printed state to the error message): A callback function that is called when
  the timeout is reached. It receives an error object and can be used to
  customize the error handling. -- mutationObserverOptions (default: {subtree:
  true, childList: true, attributes: true, characterData: true}): Options for
  the MutationObserver used by the waitFor method to detect changes in the DOM.

Return value: The waitFor method returns a promise that resolves when the
condition specified in the callback function is met or rejects if the timeout is
reached.

Explanation: The waitFor method allows you to wait for a certain condition to be
met in an asynchronous and non-blocking manner. It repeatedly calls the callback
function until the condition is satisfied or the timeout is reached.

Here's how it works:

- The waitFor method starts by immediately executing the callback function.
- If the callback function returns a truthy value, the waitFor method resolves
  the promise and the execution continues.
- If the callback function throws an error, the waitFor method rejects the
  promise, and the error can be  
  caught and handled in the surrounding code. If the callback function returns a
  promise, the waitFor method waits for the promise to resolve. If the promise
  resolves to a truthy value, the waitFor method resolves the promise and
  continues the execution. If the promise rejects, the waitFor method rejects
  the promise as well.
- If the callback function doesn't satisfy the condition or throws an error, the
  waitFor method waits for the specified interval (default: 50ms) and then
  repeats the process by calling the callback function again.
- The process continues until the condition is met, the timeout is reached, or
  an error occurs.

The waitFor method uses a MutationObserver to detect changes in the DOM. By
default, it observes additions and removals of child elements (including text
nodes), as well as attribute changes within the specified container.

The waitFor method provides flexibility through the options object. You can
customize the container, timeout, interval, error handling, and mutation
observer options according to your specific testing needs.

3. waitForElementToBeRemoved

waitForElementToBeRemoved wait until an element or elements are removed from the
DOM. It is a wrapper around the waitFor utility and simplifies the process of
waiting for the removal of elements.

Here is the explanation of waitForElementToBeRemoved from the provided
documentation:

```
function waitForElementToBeRemoved<T>(
  callback: (() => T) | T,
  options?: {
    container?: HTMLElement
    timeout?: number
    interval?: number
    onTimeout?: (error: Error) => Error
    mutationObserverOptions?: MutationObserverInit
  },
): Promise<void>
```

The first argument of waitForElementToBeRemoved can be either an element, an
array of elements, or a callback function that returns an element or an array of
elements. This represents the element(s) that you want to wait for their removal
from the DOM.

The options parameter is an optional object that allows you to customize the
behavior of waitForElementToBeRemoved. It includes the following properties:

- container (optional): The container element in which to look for the
  element(s) being removed. The default is the global document.

- timeout (optional): The maximum amount of time (in milliseconds) to wait for
  the element(s) to be removed. If the timeout is reached and the element(s) are
  still present, the waitForElementToBeRemoved promise will be rejected. The
  default timeout is 1000ms.

- interval (optional): The interval (in milliseconds) at which to check for the
  presence of the element(s) being removed. The default interval is 50ms.

- onTimeout (optional): A callback function that allows you to customize the
  error thrown when the timeout is reached. It receives the timeout error as an
  argument and should return an error object. The default behavior appends the
  container's printed state to the error message.

- mutationObserverOptions (optional): Options to configure the MutationObserver
  used to detect changes in the DOM. By default, it detects additions and
  removals of child elements (including text nodes) in the container and its
  descendants, as well as attribute changes.

The waitForElementToBeRemoved function returns a Promise that resolves when the
element(s) specified in the callback or argument are no longer present in the
DOM. If the timeout is reached before the removal occurs, the promise will be
rejected.

Note: that waitForElementToBeRemoved throws an error if the first argument is
null or an empty array, indicating that the element(s) to be removed should
exist before waiting for their removal.

Overall, waitForElementToBeRemoved provides a convenient way to handle the
asynchronous removal of elements from the DOM and allows you to synchronize your
tests accordingly.

# Accessibility

1. getRoles This function allows iteration over the implicit ARIA roles
   represented in a given tree of DOM nodes.

It returns an object, indexed by role name, with each value being an array of
elements which have that implicit ARIA role.

See ARIA in HTML for more information about implicit ARIA roles.

2. logRoles

This helper function can be used to print out a list of all the implicit ARIA
roles within a tree of DOM nodes, each role containing a list of all of the
nodes which match that role. This can be helpful for finding ways to query the
DOM under test with getByRole.

# custom quries

1. getNodeText

```
getNodeText(node: HTMLElement)
```

Returns the complete text content of an HTML element, removing any extra
whitespace. The intention is to treat text in nodes exactly as how it is
perceived by users in a browser, where any extra whitespace within words in the
html code is not meaningful when the text is rendered.

```
// <div>
//   Hello
//     World  !
// </div>
const text = getNodeText(container.querySelector('div')) // "Hello World !"
```

This function is also used internally when querying nodes by their text content.
This enables functions like getByText and queryByText to work as expected,
finding elements in the DOM similarly to how users would do.

The function has a special behavior for some input elements:

```
// <input type="submit" value="Send data" />
// <input type="button" value="Push me" />
const submitText = getNodeText(container.querySelector('input[type=submit]')) // "Send data"
const buttonText = getNodeText(container.querySelector('input[type=button]')) // "Push me"
```

These elements use the attribute `value` and display its value to the user.

# debuging

1. prettyDOM

use console.log(prettyDOM(element)) it will return HTML tree of that element but
with the prettyformater:on on it.

use shorthand: screen.debug() === console.log(prettyDOM())

# Querying Within Elements

within (an alias to getQueriesForElement)

When you use within with a specific DOM element, it establishes a scope for
querying within that element. So, when you call a query function like
getByText('hello') within the within block, it will search for the text "hello"
only within that particular element.

Using within allows you to narrow down your queries to a specific part of the
DOM tree, making your tests more focused and targeted. It's particularly useful
when you want to query elements within a specific container or section of your
application.

ex:

```
const messages = document.getElementById('messages');
const helloMessage = within(messages).getByText('hello');
```

The within(messages) establishes the scope within the messages element. Then,
the getByText('hello') function is called within that scope to find the element
that contains the text "hello" within the messages section.

By using within, you can avoid potential conflicts or unintended matches with
elements outside of the specified container and ensure that your queries are
limited to the desired context.

### What is Mock function?

a mock function is a not real (Fake function) i doesn't do anything. it record
two things:

1. Numbers of time Mock fn has been called
2. argunmnet it has recived.

generaly we use mock function to pass it as a placed holder of the component
props becouse we want to test the component in isolation and need to know that
the prop is called and what values we are giving to the prop .

const mock = jest.fn()

render(<YourComponent props = {mock}>)

expect(mock).toHaveBeenCalled() // make sure fn has been called atlest one time
expect(mock).toHaveBeenCalledWith({argument provided}) // check for fn arg with
the provided arg

Note: even though mock fn don't return anything it can with some additional
properties like mockReturnValueOnce, mockReturnValue or simply providing return
inside jest.fn(()=> return abc)

### Mocking Request

mocking Request meaning creating a simulation (fake) request. where we completly
omit (not use the external API) and we just make a fake/Mock request with jest
which return some dummy data that looks like the real API data

why we mock request?

if we use real API to make a request then making a request will

1. cost money and we don't need to test API (server from the backend) we just
   need to test user browser behaviour (fronted). if there are 1000 test then
   making a server request will cost a ton of money.

2. requesting from a server is slow. making request take time and making 1000
   test will take forever to complete.

3. data that you recive from real API might change in future and coz your test
   to fail.

4. our test is now depended on something external. we just want to test the user
   behaviour on the frontend side not the server if you want to test the server
   do it sepratly in isolation not with the frontend.

how to mock a request?

1. you should know what you want to mock. you want to mock the fetch request.

2. inside src folder structure create a new folder by the name of `__mocks__`
   and inside mock folder make a file with the name of thing that you want to
   mock. in our case it is fetch so it will we fetch.js

3. inside fetch.js make a default export jest.fn().mockResolvedValue(object)
   that mimic the object send by the real API call so that it looks like the
   real API call.

4. now import fetch.js in the testing environment and create a mock function
   "const fetch =jest.mock(fetch)" and use it as a fake API.

Mocking modules is a powerful technique in testing because it allows you to
replace dependencies with controlled, predictable behavior. by mocking the fetch
module, the test can be executed without relying on the actual API, making the
tests faster, more reliable, and independent of external services.

## jest Object

1. jest.mock(moduleName, factory, options) : It allows you to automatically mock
   modules or dependencies used in your code during the test execution.

- moduleName is the name of the module or dependency that you want to mock. It
  can be either a relative path to a local module or the name of an installed
  package.
- factory (optional) is a factory function or module that generates the mock
  implementation for the module being mocked. This allows you to provide custom
  mock behavior. If not provided, Jest will automatically generate a mock
  implementation based on the module's interface.
- options (optional) is an object that can be used to configure the mock
  behavior. It allows you to specify options such as whether to reset the mock
  before each test or to restore the original implementation.

ex:

```
import { getValue } from './myModule'

jest.mock('./myModule', () => ({
  getValue: "customMock",
}),)

test("ABC", ()=> here getValue = customMock )
```

now anyone try to acess anything from "./myModule" address they will get an
object in return { getValue:"customMock"}

2. jest.unmock(moduleName)

The jest.unmock(moduleName) function is used to unmock a module that has been
previously mocked using jest.mock().

When you mock a module using jest.mock(), Jest replaces the actual
implementation of the module with a mocked version. This is useful in situations
where you want to control the behavior of the module during testing.

However, there may be cases where you want to revert back to using the original,
unmocked implementation of a module. This is where jest.unmock(moduleName) comes
into play.

3.

### .mock property

every mock function has access to .mock object which contain info about how the
function has been called and what the function returned is kept. The .mock
property also tracks the value of this for each call, so it is possible to
inspect this as well:

1. mockfn.mock.call

The .calls property is an array that contains inner arrays, where each inner
array represents a call made to the mock function. Each inner array contains the
arguments passed to the mock function during that specific call.

ex:

```
mockFunction('apple', 'banana');
mockFunction('orange', 'grape');
mockFunction(1, 2, 3);
------------------------------------------
mockFunction.mock.calls = [
  ['apple', 'banana'],
  ['orange', 'grape'],
  [1, 2, 3],
];
```

2. mockfn.mock.result

The .results property is an array that contains objects representing the results
of the mock function for each call. Each object in the array corresponds to a
specific call and contains properties such as value, error, and type.

ex:

```
mockFunction.mockReturnValueOnce('apple');
mockFunction.mockReturnValueOnce('orange');
mockFunction.mockImplementation(() => 'banana');

mockFunction(); // Returns 'apple'
mockFunction(); // Returns 'orange'
mockFunction(); // Returns 'banana'
------------------------------------------------------------
mockFunction.mock.results = [
  { type: 'return', value: 'apple' },
  { type: 'return', value: 'orange' },
  { type: 'return', value: 'banana' },
];
```

3. instance

The .instances property is an array that contains the instances name of mock
function that are created with the new keyword.

ex:

```
const instance1 = new MockConstructor();
const instance2 = new MockConstructor();
---------------------------------------------
MockConstructor.mock.instances = [
  instance1,
  instance2,
];

```

4. contexts:

- This property contains an array of objects representing the value of this
  within the mock function for each call.
- Each object corresponds to a call to the mock function and represents the this
  value used during that call.
- Example: mockFunction.mock.contexts would give you an array of objects
  representing the this value for each call to mockFunction.

5. getMockName():

- This method returns the name of the mock function as a string.
- It can be useful when you have multiple mock functions and want to identify
  them in test error output.
- Example: mockFunction.getMockName() would return the name of the mock
  function.

## Mocking returned value

Mocking return values refers to the process of simulating or controlling the
return value of a function during testing or development using mock functions.
Mock functions are special functions that can be created and configured to mimic
the behavior of real functions. They allow you to define specific return values
for function calls without actually writting fn logic.

# Creating a Mock Function:

First, you need to create a mock function:

```
const myMock = jest.fn();
```

# Configuring Return Values:

Once you have the mock function, you can configure its return values using
specific methods provided by the mocking library. In the case of Jest, you can
use the mockReturnValue or mockReturnValueOnce methods.

1. mockReturnValue: Sets a default return value for the mock function for every
   call.

```
myMock.mockReturnValue('mocked value');
```

2. mockReturnValueOnce: Sets a specific return value for the mock function for
   the next call(s). You can chain multiple mockReturnValueOnce calls to define
   different return values for consecutive calls.

```
myMock.mockReturnValueOnce(10).mockReturnValueOnce('x').mockReturnValue(true);
```

# Utilizing the Mock Function:

Now, you can use the mock function in your test or development code as you would
use a regular function. When the mocked function is called, it will return the
configured return values instead of executing the original logic.

```
console.log(myMock()); // Output: 'mocked value'
console.log(myMock()); // Output: 10
console.log(myMock()); // Output: 'x'
console.log(myMock()); // Output: true
```

In this example, the first call to myMock() returns the default value 'mocked
value' set by mockReturnValue. The subsequent calls return the values configured
by mockReturnValueOnce in the order they were defined.

Mocking return values is particularly useful when you want to isolate specific
parts of your code for testing, simulate different scenarios, or control the
behavior of dependent functions or components. By mocking return values, you can
ensure consistent and predictable behavior during testing without relying on the
actual implementation of the functions being called.

### Jest matchers

# expect

The expect function is typically used in conjunction with a "matcher" function
to make assertions about values and test whether they meet the expected
conditions. Instead of calling expect alone, you use it with a matcher function
to specify what you expect from a particular value.

It's important to note that when using expect, the value produced by your code
is passed as the argument to the expect function, and the expected value (the
correct value) is provided as the argument to the matcher function. If you
accidentally mix them up, your tests may still run, but the error messages for
failing tests might appear strange or misleading.

# Modifier

1. .not: This modifier allows you to test the opposite of a condition.

```
test('the best flavor is not coconut', () => {
  expect(bestLaCroixFlavor()).not.toBe('coconut');
});
```

bestLaCroixFlavor() != "coconut"

# Macher

1. .toBeDefined(): Use .toBeDefined to check that a variable is not undefined.
2. .toBeFalsy(): this matches anything that an if statement treats as false.
3. .toBeGreaterThan(number): it compare received > expected for number values.
4. .toBeGreaterThanOrEqual(number): it compare received >= expected for number
   values.
5. .toBeLessThan(number): it compare received < expected for number values.
6. .toBeLessThanOrEqual(number): it compare received <= expected for number
   values.
7. toBeTruthy(): matches anything that an if statement treats as true
8. .toBeNull(): matches only null,is the same as .toBe(null) but the error
   messages are a bit nicer.
9. .toBeUndefined(): check that a variable is undefined.
10. .toBeNaN(): when checking a value is NaN.

11. .toBe(Value)

Use .toBe to compare primitive values or to check referential identity of object
instances. It calls Object.is to compare values, which is even better for
testing than === strict equality operator.

use tobe to compare number strings (only primitve values) it is similer to === .

```
const can = {
  name: 'pamplemousse',
  ounces: 12,
};

test('has 12 ounces', () => {
    expect(can.ounces).toBe(12); //True
});
```

Note: Don't use .toBe with floating-point numbers. For example, due to rounding,
in JavaScript 0.2 + 0.1 is not strictly equal to 0.3. If you have floating point
numbers, try .toBeCloseTo instead.

---12--16 are toHaveBeenCalled() varients ---------

12. .toHaveBeenCalled() also called .toBeCalled(): This matcher function is used
    to ensure that a mock function has been called. It verifies that the
    specified function has been invoked at least once.

ex:

```
test('drinks something lemon-flavoured', () => {
  const drink = jest.fn();
  drinkAll(drink, 'lemon');
  expect(drink).toHaveBeenCalled();
});
```

13. .toHaveBeenCalledTimes(number) also called .toBeCalledTimes(number): This
    matcher function is used to ensure that a mock function has been called a
    specific number of times. The number argument specifies the expected number
    of times the function should have been called

```
test('drinkEach drinks each drink', () => {
  const drink = jest.fn();
  drinkEach(drink, ['lemon', 'octopus']);
  expect(drink).toHaveBeenCalledTimes(2);
});
```

14. .toHaveBeenCalledWith(arg1, arg2, ...) also called .toBeCalledWith(): This
    matcher function is used to ensure that a mock function has been called with
    specific arguments. It verifies that the mock function has been invoked with
    the provided arguments.

```
// A simple function that adds two numbers
function add(a, b) {
  return a + b;
}

// Testing the add function
test('add function is called with correct arguments', () => {
  const mockAdd = jest.fn(); // Create a mock function

  mockAdd(2, 3); // Call the mock function with arguments

  expect(mockAdd).toHaveBeenCalledWith(2, 3);
});
```

15. .toHaveBeenLastCalledWith(arg1, arg2, ...) also called .lastCalledWith(arg1,
    arg2, ...): This matcher function is used to test the arguments of the last
    invocation of a mock function. It verifies that the last call to the mock
    function was made with the provided arguments.

```
test('applying to all flavors does mango last', () => {
  const drink = jest.fn();
  drink(mango)
  expect(drink).toHaveBeenLastCalledWith('mango');
});
```

16. .toHaveBeenNthCalledWith: this matcher function is used to test the
    arguments of the nth invocation of a mock function. It allows you to verify
    the arguments passed to a mock function when it is called at a specific
    index or nth call.

```
// A simple function that logs messages
function logMessage(message) {
  console.log(message);
}

// Testing the logMessage function
test('logMessage function is called with correct arguments', () => {
  const mockLog = jest.fn(); // Create a mock function

  mockLog('Hello');
  mockLog('World');
  mockLog('!');

  expect(mockLog).toHaveBeenNthCalledWith(2, 'World');
});

```

---17-21 are toHaveReturned varient-------------------

17. .toHaveReturned() (also .toReturn): Use .toHaveReturned to test that a mock
    function has returned (i.e., did not throw an error) at least once.

18. .toHaveReturnedTimes(number) (also .toReturnTimes): Use .toHaveReturnedTimes
    to ensure that a mock function has returned (i.e., did not throw an error)
    an exact number of times. Any calls to the mock function that throw an error
    are not counted toward the number of times the function has returned.

19. .toHaveReturnedWith(value) (also .toReturnWith): Use .toHaveReturnedWith to
    ensure that a mock function has returned a specific value.

20. .toHaveLastReturnedWith(value) (also .lastReturnedWith): Use
    .toHaveLastReturnedWith to test the specific value that a mock function last
    returned. If the last call to the mock function threw an error, this matcher
    will fail regardless of the expected return value.

21. .toHaveNthReturnedWith(number, value) (also .nthReturnedWith): Use
    .toHaveNthReturnedWith to test the specific value that a mock function
    returned for the nth call. If the nth call to the mock function threw an
    error, this matcher will fail regardless of the expected return value.

22. .toHaveLength(number): Use .toHaveLength to check that expected element has
    a .length property on it or not and if it has then is it set to a certain
    numeric value.

This is especially useful for checking arrays or strings size. ex:

```
expect([1, 2, 3]).toHaveLength(3);
expect('abc').toHaveLength(3);
```

23. .toHaveProperty(keyPath, value?):

- Use .toHaveProperty to check if a property exists at the provided reference
  keyPath for an object.
- The keyPath can be specified using dot notation for nested properties or an
  array containing the key path for deep references.
- You can provide an optional value argument to compare the received property
  value against the expected value (using deep equality, like the toEqual
  matcher).

```
const houseForSale = {
  bath: true,
  bedrooms: 4,
  kitchen: {
    area: 20,
  },
};

test('this house has my desired features', () => {
  expect(houseForSale).toHaveProperty('bath');
  expect(houseForSale).toHaveProperty('bedrooms', 4);
  expect(houseForSale).toHaveProperty('kitchen.area', 20);
})
```

24. .toBeCloseTo(number, numDigits?):

- Use .toBeCloseTo to compare floating-point numbers for approximate equality.
- The optional numDigits argument limits the number of digits to check after the
  decimal point.
- By default (numDigits not specified or null)

```
test('adding works sanely with decimals', () => {
  expect(0.2 + 0.1).toBe(0.3); // Fails due to floating-point precision error
  expect(0.2 + 0.1).toBeCloseTo(0.3, 5); // Passes with a precision of 5 digits
});

expect(0.2 + 0.1).toBe(0.3) fails because in JavaScript, 0.2 + 0.1 is actually 0.30000000000000004.

```

25. .toBeInstanceOf(Class): to check that an object is an instance of a class.
    an instance is created with new keyword.

26. .toContain(item): is used to check if a specific item is present in an array
    or other iterable objects like strings, sets, node lists, or HTML
    collections. It performs a strict equality check (===) to determine if the
    item is included.

ex:

```
// Example 1: Checking an item in an array
const flavors = ['apple', 'orange', 'lime'];

test('the flavor list contains lime', () => {
  expect(flavors).toContain('lime');
});

// Example 2: Checking a substring in a string
const sentence = 'The quick brown fox jumps over the lazy dog';

test('the sentence contains "brown"', () => {
  expect(sentence).toContain('brown');
});
```

In the first example, we have an array flavors, and we use .toContain('lime') to
verify that the array contains the string 'lime'. If the item is present in the
array, the test will pass.

In the second example, we have a string sentence, and we use .toContain('brown')
to check if the string contains the substring 'brown'. If the substring is found
in the string, the test will pass.

You can use the .toContain matcher with various iterable objects to check for
the presence of specific items or substrings.

27. .toContainEqual(item): is used to check if an item with a specific structure
    and values is contained in an array. It performs a deep equality check,
    recursively comparing all fields of the objects within the array, rather
    than checking for object identity.

ex:

```
describe('my beverage', () => {
  test('is delicious and not sour', () => {
    const myBeverage = { delicious: true, sour: false };
    expect(myBeverages()).toContainEqual(myBeverage);
  });
});

```

If the array contains an object that has the same all field values as
myBeverage, the test will pass. Otherwise, if there is no matching object in the
array, the test will fail.

The .toContainEqual matcher is particularly useful when working with arrays of
objects and you want to check if a specific object with a particular structure
and values exists within the array.

28. .toEqual(value): is used to compare the equality of two values, including
    objects and arrays, by recursively checking all their properties or
    elements. It performs a deep equality check, ensuring that all properties or
    elements match.

ex:

```
const can1 = {
  flavor: 'grapefruit',
  ounces: 12,
};
const can2 = {
  flavor: 'grapefruit',
  ounces: 12,
};

describe('the La Croix cans on my desk', () => {
  test('have all the same properties', () => {
    expect(can1).toEqual(can2);// we are checking obj1 each key:value within obj2 key: value
  });
  test('are not the exact same can', () => {
    expect(can1).not.toBe(can2); // here we are check obj1 === obj2
  });
});
```

Note: .toEqual ignores certain discrepancies, such as undefined properties in
objects, undefined items in arrays, array sparseness, or object type mismatches.
If you want to consider these differences in your comparison, you can use the
.toStrictEqual matcher instead.

29. .toMatch(regexp | string): is used to check whether a string matches a
    regular expression or substring. It is commonly used in test cases to verify
    specific patterns or substrings within strings.

ex:

```
test('grapefruits are a fruit', () => {
    expect('grapefruits').toMatch('fruit');
  });
```

30. .toMatchObject(object): is used to compare if a JavaScript object matches a
    subset of the properties of another object.
    expected(superset-Object).toMatchObject(subset-Object)

ex:

```
const houseForSale = {
  bath: true,
  bedrooms: 4,
  kitchen: {
    amenities: ['oven', 'stove', 'washer'],
    area: 20,
    wallColor: 'white',
  },
};

const desiredHouse = {
  bath: true,
  kitchen: {
    amenities: ['oven', 'stove', 'washer'],
    wallColor: expect.stringMatching(/white|yellow/),
  },
};

test('the house has my desired features', () => {
  expect(houseForSale).toMatchObject(desiredHouse);
});

```

31. .toStrictEqual(value): use to test that objects have the same structure and
    type.

Differences from .toEqual:

- keys with undefined properties are checked, e.g. {a: undefined, b: 2} will not
  equal {b: 2};
- undefined items are taken into account, e.g. [2] will not equal [2,
  undefined];
- array sparseness is checked, e.g. [, 1] will not equal [undefined, 1];
- object types are checked

32. .toThrow(error): is used to test whether a particular function throws an
    error when it is called. It allows you to write assertions to verify that
    the expected error is thrown, ensuring the code behaves as intended in error
    scenarios.

expected(function that throw an error).toThrow(check error message match)

in check error message match you can provide: string message (strict match),
/subString/ (partical match),new Error('strict match').

----------- Asymmetric Matchers -------------------------------------

33. expect.anything(): is used to matches anything but null or undefined. you
    can use this as a argunment in assersion matcher. ex:
    equlto(expect.anything()) means can be equal to anything but not null or
    undefined.

You can use it inside toEqual or toBeCalledWith

34. expect.any(constructor): to match any value that was created with the given
    constructor or is a primitive of the specified type. It allows you to check
    the type of a value in your assertions.

expect.any(constructor) is used to check expect(element) it will check the type
of element and then you can assert it with equalTo(expect.any(constructor)).

constructor can be = string,number,object.

You can use it inside toEqual or toBeCalledWith

35. expect.arrayContaining(array): This matcher matches a received array that
    contains all elements in the expected array. It checks if the expected array
    is a subset of the received array.

expected(superSet of array).toEqual(expect.arrayContaining(subset of array))

You can use it inside toEqual or toBeCalledWith to match a property in
objectContaining or toMatchObject

35. expect.not.arrayContaining(array): This matcher matches a received array
    that does not contain all elements in the expected array.

expected(superSet of array).toEqual(expect.arrayContaining( does not contain
subset of array))

You can use it inside toEqual or toBeCalledWith

36. expect.closeTo(number, numDigits?): is useful when comparing floating point
    numbers in object properties or array item. If you need to compare a number,
    please use .toBeCloseTo instead.

ex:

```
test('compare float in object properties', () => {
  expect({
    title: '0.1 + 0.2',
    sum: 0.1 + 0.2,
  }).toEqual({
    title: '0.1 + 0.2',
    sum: expect.closeTo(0.3, 5),
  });
});
```

37. expect.objectContaining(object): matches any received object that
    recursively matches the expected properties. That is, the expected object is
    a subset of the received object. Therefore, it matches a received object
    which contains properties that are present in the expected object.

expected(superSet of object).toEqual(expect.arrayContaining(contain subset of
object))

38. expect.not.objectContaining(object): matches any received object that does
    not recursively match the expected properties. That is, the expected object
    is not a subset of the received object. Therefore, it matches a received
    object which contains properties that are not in the expected object.

expected(superSet of object).toEqual(expect.arrayContaining( does not contain
subset of object))

39. expect.stringContaining(string): matches the received value if it is a
    string that contains the exact expected string.

40. expect.not.stringContaining(string): matches the received value if it is not
    a string or if it is a string that does not contain the exact expected
    string.

ex:

```
expect('How are you?').toEqual(expect.not.stringContaining("hello world"))
```

41. expect.stringMatching(string | regexp): matches the received value if it is
    a string that matches the expected string or regular expression. It allows
    you to assert that a string value meets a certain pattern or format.

You can use it instead of a literal value:

- in toEqual or toBeCalledWith
- to match an element in arrayContaining
- to match a property in objectContaining or toMatchObject

```
const expected = [
    expect.stringMatching(/^Alic/),
    expect.stringMatching(/^[BR]ob/),
  ];

it('matches even if received contains additional elements', () => {
    expect(['Alicia', 'Roberto', 'Evelina']).toEqual(
      expect.arrayContaining(expected),
    )
});
```

42. expect.not.stringMatching(string | regexp): matches the received value if it
    is not a string or if it is a string that does not match the expected string
    or regular expression.

### jest snapshot testing.

when we or create a snapshot it copys the HTML DOM for that perticular

jest snapshot allow you to make a copy of the provided HTML structure and
compaire it as a refrence. It helps to ensure that the output remains consistent
over time and does not unintentionally change.

When you run a snapshot test for the first time, Jest captures the output of the
component or function and saves it as a snapshot file. On subsequent test runs,
Jest compares the new output with the saved snapshot to check for any
differences. If the output matches the snapshot, the test passes. If there are
differences, Jest provides a diff of the changes and asks you to review and
confirm whether the changes are intentional. if the changes were intentional
then then you can also update the snapshot by prassing "u" in the command line.
This approach simplifies the process of verifying the correctness of your code
output and makes it easier to catch unintended changes.

# how to take a snapshot

ex: lets say you are currently testing a module component in a file name
module.test.js

```
test('renders correctly', () => {
  const component = renderMyComponent();
  expect(component).toMatchSnapshot();
});
```

- onthe first render of the test jest will take a snapshot of the component HTML
  and create a file with the same name of the file in the same directry with
  different extension in our case (module.test.js.snapshot)

- in (module.test.js.snapshot) there will be HTML of the component in text
  format.

- in consiquent render of the test toMatchSnapshot() will compare the expected
  component HTML with the snaphot it has if the HTML matches then test will pass
  otherwise If there are differences, Jest provides a diff of the changes and
  asks you to review and confirm whether the changes are intentional. if the
  changes were intentional then then you can also update the snapshot by
  prassing "u" in the command line.

# difference between toMatchSnapshot (snapshot) and toMatchInlineSnapshot (inline snapshot)

1. toMatchSnapshot (snapshot): take a snapshot and store it into a seprate file
   with .snapshot extension.

2. toMatchInlineSnapshot (inline snapshot):take a snapshort and store it
   directly in the same argunment.

ex:

```
----------------Before inital render ----------------------------
it('renders correctly', () => {
  const tree = renderer
    .create(<Link page="https://example.com">Example Site</Link>)
    .toJSON();

  expect(tree).toMatchInlineSnapshot();
});

-----------------after inital render ----------------------------
it('renders correctly', () => {
  const tree = renderer
    .create(<Link page="https://example.com">Example Site</Link>)
    .toJSON();

  expect(tree).toMatchInlineSnapshot(`
<a
  className="normal"
  href="https://example.com"
  onMouseEnter={[Function]}
  onMouseLeave={[Function]}
>
  Example Site
</a>
`);
});
```

# how to handle dynamic data in snapshot

The provided documentation explains the usage of property matchers in Jest's
snapshot testing. When you take a snapshot of an object that contains
dynamically generated fields, such as IDs or Dates, the snapshot will fail on
every test run because the values of these fields change.

To address this issue, Jest allows you to provide asymmetric matchers for
specific properties. These matchers are checked before the snapshot is written
or tested, and the matched values are saved to the snapshot file instead of the
received values.

ex:

```
it('will check the matchers and pass', () => {
  const user = {
    createdAt: new Date(),
    id: Math.floor(Math.random() * 20),
    name: 'LeBron James',
  };

  expect(user).toMatchSnapshot({
    createdAt: expect.any(Date),
    id: expect.any(Number),
  });
});
```

it means any value that you place inside the toMatchSnapshot({..here..}) and
match with the HTML/data of expect(data) then those value will be overWritien
and replaced in our case we use any method to date and id it means that any
valur of date will be pass the case. rest of the data will be checked as
comparision that is not defined in toMatchSnapshot({..here..}) to the previous
stored snapshot.

# what is toThrowErrorMatchingSnapshot() and .toThrowErrorMatchingInlineSnapshot()

1. toThrowErrorMatchingSnapshot(): is used to take a snapshot of an error and
   compare it with exsisting error snapshot (from the stored file). if the error
   matches then the test passses.

ex:

```
it('throws an error with a matching snapshot', () => {
  const myFunction = () => {
    throw new Error('This is an error message');
  };

  expect(myFunction).toThrowErrorMatchingSnapshot();
});

```

By using toThrowErrorMatchingSnapshot(), you can easily capture and maintain
snapshots of error messages, ensuring that any changes to the error messages are
intentionally made and reviewed.

2. toThrowErrorMatchingInlineSnapshot(): same but for the inline.

### Global methods describe, expect, test

1. afterAll(fn, timeout)

Runs a function after all the tests in this file have completed. If the function
returns a promise or is a generator, Jest waits for that promise to resolve
before continuing.

Optionally, you can provide a timeout (in milliseconds) for specifying how long
to wait before aborting. The default timeout is 5 seconds.(it will wait 5 sec
afterAll the test has completed before exiting)

This is often useful if you want to clean up some global setup state that is
shared across tests.

Note: If afterAll is inside a describe block, it runs at the end of the describe
block.

2. afterEach(fn, timeout)

Runs a function after each one of the tests in this file completes. If the
function returns a promise or is a generator, Jest waits for that promise to
resolve before continuing.

Optionally, you can provide a timeout (in milliseconds) for specifying how long
to wait before aborting. The default timeout is 5 seconds.

This is often useful if you want to clean up some temporary state that is
created by each test.

NOte: If afterEach is inside a describe block, it only runs after the tests that
are inside this describe block.

3. beforeAll(fn, timeout)

Runs a function before any of the tests in this file run. If the function
returns a promise or is a generator, Jest waits for that promise to resolve
before running tests.

Optionally, you can provide a timeout (in milliseconds) for specifying how long
to wait before aborting. The default timeout is 5 seconds.

This is often useful if you want to set up some global state that will be used
by many tests.

Note: If beforeAll is inside a describe block, it runs at the beginning of the
describe block.

4. beforeEach(fn, timeout)

Runs a function before each of the tests in this file runs. If the function
returns a promise or is a generator, Jest waits for that promise to resolve
before running the test.

Optionally, you can provide a timeout (in milliseconds) for specifying how long
to wait before aborting. The default timeout is 5 seconds.

This is often useful if you want to reset some global state that will be used by
many tests.

Note:If beforeEach is inside a describe block, it runs for each test in the
describe block.

5. describe(name, fn)

- describe(name, fn) creates a block that groups together several related tests.

- This isn't required - you can write the test blocks directly at the top level.
  But this can be handy if you prefer your tests to be organized into groups.

- You can also nest describe blocks if you have a hierarchy of tests:

ex:

```
describe('binaryStringToNumber', () => {
  describe('given an invalid binary string', () => {
    test('composed of non-numbers throws CustomError', () => {
      expect(() => binaryStringToNumber('abc')).toThrow(CustomError);
    });

    test('with extra whitespace throws CustomError', () => {
      expect(() => binaryStringToNumber('  100')).toThrow(CustomError);
    });
  });
})
```

6. describe.only(name, fn): You can use describe.only if you want to run only
   one describe block

7. describe.skip(name, fn): You can use describe.skip if you do not want to run
   the tests of a particular describe block

8. test(name, fn, timeout) also called it(name, fn, timeout)

All you need in a test file is the test method which runs a test.

The first argument is the test name; the second argument is a function that
contains the expectations to test. The third argument (optional) is timeout (in
milliseconds) for specifying how long to wait before aborting. The default
timeout is 5 seconds.

If a promise is returned from test, Jest will wait for the promise to resolve
before letting the test complete.

9. test.only(name, fn, timeout) also called it.only(name, fn, timeout)

When you are debugging a large test file, you will often only want to run a
subset of tests. You can use .only to specify which tests are the only ones you
want to run in that test file.

Optionally, you can provide a timeout (in milliseconds) for specifying how long
to wait before aborting. The default timeout is 5 seconds.

10. test.skip(name, fn) also called it.skip(name, fn), xit(name, fn), and
    xtest(name, fn)

When you are maintaining a large codebase, you may sometimes find a test that is
temporarily broken for some reason. If you want to skip running this test, but
you don't want to delete this code, you can use test.skip to specify some tests
to skip.

11. test.todo function in Jest is used to mark a test as a "to-do" item or a
    placeholder for a test that needs to be implemented in the future. It allows
    you to quickly add reminders for tests that you plan to write later.

```
test.todo('should calculate total price of items');
```

When you run your test suite, the test marked as "to-do" will be reported as a
pending test. It serves as a reminder for you to come back later and fill in the
test body with the actual implementation.

## there are some global method that require the use of typeScript to use them(so i skipped over them)

### React testing library DOM Method

1. render

```
function render(
  ui: React.ReactElement<any>,
  options?: {
    /* You won't often use this, expand below for docs on options */
  },
): RenderResult
```

render fn is used to render a React component into a container, which is then
appended to the document.body.

The render function takes two parameters: ui (React element) and options
(optional configuration object).

byDefault container is = <div><div>

so if you render(<div/>) then your created element is automatically appended to
the container document.body.appendChild(<div/>) the output will be:

```
<body>
    <div>
        <div>
        </div>
    </div>
</body>
```

The render function returns a RenderResult object containing utility functions
for interacting with the rendered component during testing.

## render options

1. container
2. baseElement
3. hydrate
4. legacyRoot
5. wrapper
6. queries

7. container : By default, React Testing Library will create a div and append
   that div to the document.body and this is where your React component will be
   rendered. If you provide your own HTMLElement container via this option, it
   will not be appended to the document.body automatically.

ex:

```
const table = document.createElement('table')

const {container} = render(<TableBody {...props} />, {
  container: document.body.appendChild(table),
})
```

you have to create HTML and Append it too.

2. baseElement : default BaseElement is document.body but if you have specified
   the container option then your container option will become the baseElement.

baseElement is the top level element which query/varient/element finder starts
to look from for dom Element. and this is what you can see in screen.Debug()
starts from the baseElement.

3. hydrate: If hydrate is set to true, then it will render with
   ReactDOM.hydrate. This may be useful if you are using server-side rendering
   and use ReactDOM.hydrate to mount your components.

4. legacyRoot: By default we'll render with support for concurrent features
   (i.e. ReactDOMClient.createRoot). However, if you're dealing with a legacy
   app that requires rendering like in React 17 (i.e. ReactDOM.render) then you
   should enable this option by setting legacyRoot: true.

5. wrapper: the wrapper option allows you to pass a React component as a wrapper
   around the inner element during rendering. This feature is particularly
   useful when you want to extract out complex component functionality that is
   highy depended on some context to function. help to isolating complex
   component

ex:

```
import React from 'react'
import {render} from '@testing-library/react'
import {ThemeProvider} from 'my-ui-lib'
import {TranslationProvider} from 'my-i18n-lib'
import defaultStrings from 'i18n/en-x-default'

const AllTheProviders = ({children}) => {
  return (
    <ThemeProvider theme="light">
      <TranslationProvider messages={defaultStrings}>
        {children}
      </TranslationProvider>
    </ThemeProvider>
  )
}

const customRender = (ui, options) =>
  render(ui, {wrapper: AllTheProviders, ...options})

// re-export everything
export * from '@testing-library/react'

// override render method
export {customRender as render}
```

in this ex: your ui element from render will become a chlid of AlltheProvider
component that wrap it's children with muliple context value.

6. queries: this option allow you to use custom created queries with the render
   element.

## render fn return an object which also contain some fn that can be usedful

1. rerender

It'd probably be better if you test the component that's doing the prop updating
to ensure that the props are being updated correctly. That said, if you'd prefer
to update the props of a rendered component in your test, this function can be
used to "upDate props" of the rendered component.

```
import {render} from '@testing-library/react'

const {rerender} = render(<NumberDisplay number={1} />)

// re-render the same component with different props
rerender(<NumberDisplay number={2} />)
```

2. unmount

This will cause the rendered component to be unmounted. This is useful for
testing what happens when your component is removed from the page

ex:

```
import {render} from '@testing-library/react'

const {container, unmount} = render(<Login />)
unmount()
// your component has been unmounted and now: container.innerHTML === ''
```

### overWrite default React-testing-library methods with your custom methods

to do that create a file and import the method you want to overWrite from
`@testing-library/react`

create your own implimentation with the same name as the original method and
export it as

- from "@testing-library/react"

now where ever you want to use your implimentation of that overWritten method

import that method from `test/test-utils` and you are done use your own
implimentation of that method.

ex:

```
---------------------create your own implimentation---------------
//test-utils.js

import * as React from 'react'
import {render as rtlRender} from '@testing-library/react'
import {ThemeProvider} from 'components/theme'

function render(ui, {theme = 'light', ...options} = {}) {
  const Wrapper = ({children}) => (
    <ThemeProvider initialTheme={theme}>{children}</ThemeProvider>
  )
  return rtlRender(ui, {wrapper: Wrapper, ...options})
}

export * from '@testing-library/react'
// override React Testing Library's render with our own
export {render}
-----------------------import it from `test/test-utils`--------------
//07.extra-3.js
import * as React from 'react'
import {render, screen} from 'test/test-utils'
import EasyButton from '../../components/easy-button'

test('renders with the light styles for the light theme', () => {
  render(<EasyButton>Easy</EasyButton>, {theme: 'light'})
  const button = screen.getByRole('button', {name: /easy/i})
  expect(button).toHaveStyle(`
    background-color: white;
    color: black;
  `)
})

```

NOTE if you want to import your custom render from 'test/test-utils' then you
need to configure your jest.config to easily find the 'test/test-utils' file
other wise you need to use './../test/test-utils'dependening on the location of
your file.

```
in jest.config add
moduleDirectories:["node_modules",path.join(__dirname,"src")]
```

## some additional method that comes from react testing library other then render

1. cleanup

Unmounts React trees that were mounted with render.

Please note that this is done automatically if the testing framework you're
using supports the afterEach global and it is injected to your testing
environment (like mocha, Jest, and Jasmine). If not, you will need to do manual
cleanups after each test.

ex:

```
test.afterEach(cleanup)
```

2. act

The purpose of the act function is to ensure that all updates to the rendered
components are properly processed and applied before performing any assertions
or checks on the component's state or the DOM.

Please note that this is done automatically if the testing framework you're
using supports the afterEach global and it is injected to your testing
environment (like mocha, Jest, and Jasmine). If not, you will need to do manual
cleanups after each test.

Here are some key points about the act function:

- It ensures that all pending state updates, effects, and other asynchronous
  tasks are completed and applied before proceeding.
- It helps simulate the behavior of React when rendering and updating
  components, including handling asynchronous operations.
- It ensures that the component's state and the DOM are in a stable and
  consistent state before making any assertions or performing any further
  actions.
- It is typically used when interacting with the rendered component, such as
  triggering events, updating state, or making asynchronous requests, to ensure
  that the component is properly updated and reflects the expected behavior.
- It is recommended to use the act function when working with asynchronous
  operations or when making updates that could trigger re-renders or other side
  effects.

ex:

```
test('increments count on button click', () => {
  act(() => {
    render(<Counter />);
  });

  const countText = screen.getByText(/Count:/);
  const incrementButton = screen.getByText('Increment');

  expect(countText.textContent).toBe('Count: 0');

  act(() => {
    fireEvent.click(incrementButton);
  });

  expect(countText.textContent).toBe('Count: 1');
});
```

use act fn when asyncronus actiom might take place mabe due to re-render coz by
sertain action so act will make sure that wrapped compoent become stable before
moving forward with the test.

3. renderHook: this method is designed for testing your custom hooks in testing
   environment. in react you can't place a hook outside a component so this is a
   easy fix to that.

how to use it to test your coustom hook.

- first import {renderHook} from '@testing-library/react'

- then const {result} = renderHook(()=> customHook(),options) or
- then const {result} = renderHook(customHook,options)

result contain all the methods that are returned by customHook.

3. 1. options of the renderHook

- initialProps: you can define props that your customHook take to provide
  methods

ex:

```
  const {result} = renderHook(useCounter, {initialProps: {initialCount: 3}})
  expect(result.current.count).toBe(3)
```

3. 2. return methods that you can use on renderHook

- result: Holds the value of the most recently committed return value of the
  render-customHook.

```
import {renderHook} from '@testing-library/react'

const customHook = () => {
  const [name, setName] = useState('')
  React.useEffect(() => {
    setName('Alice')
  }, [])

  return name
}

const {result} = renderHook(customHook)

expect(result.current).toBe('Alice')
```

NOTE: result.current is just like a ref where we also use ref to access the
value.

- rerender: to re-render your customHook with different props.

```
import {renderHook} from '@testing-library/react'

const {rerender} = renderHook(({name = 'Alice'} = {}) => name)

// re-render the same hook with different props
rerender({name: 'Bob'})
```

### what if you are having problem with figuring out wich role to choose to select Element.

there are the steps that i recommend you take to find which role you need to
select element

1. screen.logTestingPlaygroundURL()

this is a helper method when executed your testing render HTML DOM will be
copied and when you see your terminal a link is provided to you when you open
that link you will find TestingPlayground website with a test environment open
and there are 4 panel there to help you out which role you need to select a
pertical element from your render HTML.

1. 1. first panel is a code editer which has your render HTML on it
      automatically that was copied from your code.

1. 2. second panel is the web representation of your HTML Code to select which
      element you want to know role of. select that element.

Note: if you are having problem selecting element then add css in your first
panel like border-size: 4rem. will make the element more clickable with extra
space.

1. 3. third panel show a 100% gurenty way to select the element which you should
      use when nothing works out.

1. 4. fourth panel show you role suggestion you should use/ (this is the most
      helpful part of the site)

1. Fallback query

sometimes finding element by role just doesn't work. and you should try to find
role but don't fuzz over it to much maybe try to find it for 3-4 min and for
some reson things don't work out use there fallback queries.

2. 1. data-testid : in your code just place data-testid = "anything" and give
      any id you want you can then select that element by using
      screen.getByTestId tesing selector that is avilible to us by react testing
      library.

Note: if you are worried about that unnessesery size will be added to production
build of that app which is not cunserning to begain this will not make much of a
difference but still you feel cuncern than use
"babel-plugin-react-remove-properties" library to remove it when your code
compile for production.

you can combine data-testid with within property provided by the react testing
library. ex: within(getBydataId("user")).getByrole("row")

if there are multiple role on the page but you want to select few of them but
not all then you can reducer the scope of search for finding the elements with
within property.

2. 3. container.querySelector(): this is the last resort. if nothing works.
      querySelector is same as css querySelector to select HTML element.

ex:

```
const {container} = render(<Componet/>)

const user = container.querySelectorAll("row");
```

every render retun an object which contain multiple property one of them is
container. container is outer level HTML element. which is by default a <div>
element and when you render a component your component gets appended to that div
and now you have access to enter HTML tree due to the acess of container. you
can use anykind of javascript method to select your element. like
querySelcector, getelementbyid etc.

### create a custom matcher?

ex:

```
function containElement(container,role,quantity = 1){
    const element = within(container).queryAllByRole(role);

    if(element.length === quantity){
        return{
            pass:true
        }
    }

    return {
        pass: false,
        message: `provided ${role} quantity is ${element.length} and expected quantity was ${quantity} `
    }
}

expect.extend({containElement})

------------------------------------------------------
 test("form contain two button",()=> {
    render(<FormComponent>)

    const form = screen.queryByRole("form")

    expect(form).containElement("button",2)

 })

```

1. use expect.extend({...here..}) to add a custom matcher.
2. your custom matcher fn should do the testing but return 2 cases

3. 1. pass: true when expected result is returned
4. 2. pass: false when expected result not match with that also provide a
      message for screen.debug to show message why expected value fail.

5. in your custom component your first argunment is the element that you get in
   your expect(element) test.
6. use custom matcher when you are finding a lot of repeated test cases you need
   to repeat on every test. just like why component are used for.

### regEx

1. you can use regular expression as a string /string/i (not dynamic)

2. you can use it as constructor as new RegEx(pattern,flag)

3. 1. pattern can be regular expression as a string (also can be dynamic)
4. 2. flag (optional) there are some filter condion that help to manupulated
      provided pattern. flag are also string by nature with quatation on them.

ex: new RegEx(/string/,"i")

there are ton of flags to choose from checkout mdn page :
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/RegExp

### javascript debugger keyword

In JavaScript, there is a debugger keyword that you can use to pause the
execution of your code and invoke the debugger.

When the debugger statement is encountered in your JavaScript code, it triggers
a breakpoint, and if a debugger is available, it will pause the execution at
that point. This allows you to inspect variables, step through the code, and
debug any issues.

Here's an example of how you can use the debugger keyword in JavaScript:

```
function multiply(a, b) {
  debugger; // Pause execution here
  return a * b;
}

const result = multiply(5, 10);
console.log(result);
```

When you run this code in a browser with developer tools open, the execution
will pause at the line with the debugger statement. At that point, you can use
the debugging features provided by your browser's developer tools to inspect
variables, step through the code, and continue execution.

It's important to note that the debugger statement is intended for debugging
purposes and should be used during development. It doesn't have any effect when
your code is not being debugged or when there is no debugger available.

#### common testing error and safeguard (from course-1)

### when checking a value from a inputfield

before checking a value from a inputfield that you are going to change to check if the value changes during assersion makesure to userEvent.Clear() first . becouse you don't know what's the inital value is . it is a safeguard practice/

ex:

```
const textBox = screen.getByRole('textbox')
await userEvent.clear(textBox)
userEvent.type(textBox, 'Hello')
expected(textBox).toHaveDisplayValue("hello")

```

### spinbutton role from aria

A spinbutton typically allows users to change its displayed value by activating increment and decrement buttons that step through a set of allowed values. Some implementations display the value in an text field that allows editing and typing but typically limits input in ways that help prevent invalid values.

### most common error

1. unable to find role="role" => Either role (ex: Button) doesn't exist, or no element with that role also matches "name" option.

2. Warning: An update to component inside a test was not wrapped in act{...} => There was an update to the component after the test completed use await findBy\*

3. Warning: Can't perform a React state update on an unmounted component. this is a no-op, but it indicates a memory leak in your application => There was an update to the component state after the test completed. use awaut findBy\*

4. Error: coonect ECONNREFUSED 127.0.0.1 => There is no Mock server Worker handler associalted with the route and method..

### Learn about act keyword from kent C Dodds blog (Not required latest version don't need act, just for refrence)

### when to use act

So the act warning from React is there to tell us that something happened to our
component when we weren't expecting anything to happen. So you're supposed to
wrap every interaction you make with your component in act to let React know
that we expect our component to perform some updates and when you don't do that
and there are updates, React will warn us that unexpected updates happened. This
helps us avoid bugs.

Luckily for you and me, React automatically handles this for any of your code
that's running within the React callstack (like click events where React calls
into your event handler code which updates the component)

but it cannot handle this for any code running outside of it's own callstack
(like asynchronous code that runs as a result of a resolved promise you are
managing or if you're using jest fake timers). With those kinds of situations
you typically need to wrap that in act(...) or async act(...) yourself. BUT,
React Testing Library has async utilities that are wrapped in act automatically!

If you're still experiencing the act warning, then the most likely reason is
something is happening after your test completes for which you should be
waiting. (component re-render)

when you are triggering state update which coz the component to re-render then
you need act to make sure your app stablize before you test it. when you are
testing and doing some action which coz your react component to re-render . that
action need to wrapped by act function so that action first stablize. act
function will wait to re-render to complete then move forward with the code
test.

### error

# error name : provide router component

external library don't like to be run in the testing environment.

solution:

1. import the context provider(with context i mean the library that the element
   is thowing context error for).
2. provide the context they want wrap your render component with the context
   component.

# error name: provide act()

generaly coz by data fetching inside useEffect becouse useEffect update the DOM
after the render and data fetching a async task with by default take some time
to complete and testing environment warn you that the component has updated and
we were not looking for any kind of update. maybe after the update the changes
may break the current exsisting test cases.

testing environment run there code syncronusly one line at a time if it
encounter an a code that trigger async task it does not wait a to finish the
async task before moving forward to the next line. and there is the entire
problem.

1. so unexpected state update in test is a bad thing.

act function is a wrapper to wrap your async task with it so that it can provide
a waiting window for async task to finish first before moving to the next sync
task.

Note: act function is implimented by react DOM and it is a testing utility
directly provided by the react.

so when we are not using react tesing environment and making any async request
that action must be wrapped with act function call

in test when we wrap act fn on an event that coz the state update. we tell our
test that we expect state to be changed becouse of this act fn. and when test
reach that line and enter the act fn then react will make sure that all the
state update + useEffect finishes before exiting the act wrapper function.

react testing library uses act fn for you automatically. when you are using any
selecter that include async nature into count (they also provide a window to
finish the async task on the react before moving forward in the test) like
findBy, findAllBy, waitFor, userEvent

Note: so as there are selecter with automaticlly use act fn behind the seen.
when you see a warining saying use act you don't need to use act. and use your
react tesing library async selector (findBy, findAllBy, waitFor, userEvent)
which will fix that error for you automatically

## Module Mock (helps to skip over component)

module mock is used when your render componet or children of the render
component is dependent on some external dependency js file and you want to mock
it.

```
jest.fn("import adress",()=>{
    return ()=> {
        return "XYZ"
    }
})
```

now what will happen is the external commononet will not be imported from the
real import adress, do not actually import the real file insted if anyone try to
import that file from this import address just give them this code that i have
decleared here. this module mock will overwrite the real data with the provided
fn data.

what will endup happening is that where the external compoent is written will be
replaced by the XYZ text that we have decleared here.

we do module mocking when we don't want to includer that external file or our
test does not depend on that external file we just wnat to make the render
component isolated so we can test our test on it.
