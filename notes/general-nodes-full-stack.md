# HTML and CSS

- [HTML and CSS](#html-and-css)
  - [Emmet](#emmet)
  - [SVG](#svg)
  - [Tables in HTML](#tables-in-html)
  - [CSS Reset](#css-reset)
  - [CSS Units](#css-units)
  - [Font Face](#font-face)
  - [Text Style](#text-style)
  - [More CSS Properties](#more-css-properties)
  - [CSS Selectors extended](#css-selectors-extended)
  - [Pseudo Selectors (classes and elements)](#pseudo-selectors-classes-and-elements)
  - [Specificity Revisit](#specificity-revisit)
  - [Position](#position)
  - [CSS Variables (or Custom Properties)](#css-variables-or-custom-properties)
  - [Compatibility with different browsers](#compatibility-with-different-browsers)
  - [Forms](#forms)
    - [MDN Forms](#mdn-forms)
    - [Some extra widgets](#some-extra-widgets)
    - [Styling forms](#styling-forms)

## Emmet

- Cheatsheet: [Emmet Cheatsheet](https://docs.emmet.io/cheat-sheet/)

- `Remove Tag` action removes tag
- Directly typing out `.class` or `#id` or anything makes `div` as default tag
- `input[type="button"]` creates **input** with attribute **type**
- `ul>li*3{List Item $$}` will create a 3 list items with **List Item 01/02/03** as text
- `+` is used for creating siblings, `>` for direct children and `^` for climbing up (see example 3)
  - Note: You can use grouping operator `()` in-place of `^` as well

---

- Example 1: `ul>li*>a.link$|t`
  - `|t` filter: removes list markers

```html
* Alpha * Beta * Gamma * Delta

<!-- After -->
<ul>
  <li><a href="">Alpha</a></li>
  <li><a href="">Beta</a></li>
  <li><a href="">Gamma</a></li>
  <li><a href="">Delta</a></li>
</ul>
```

- Example 2: `ul>li*>a[href='google.com/$#']+img[alt="$#"]`
  - `$#` placeholder for output position
  - `+` allows to add multiple elements inside same parent

```html
About Contact Home

<!-- After -->
<ul>
  <li>
    <a href="google.com/About"></a>
    <img src="" alt="About" />
  </li>
  <li>
    <a href="google.com/Contact"></a>
    <img src="" alt="Contact" />
  </li>
  <li>
    <a href="google.com/Home"></a>
    <img src="" alt="Home" />
  </li>
</ul>
```

- Example 3:

```html
<!-- header>nav+footer -->

<header>
  <nav></nav>
  <footer></footer>
</header>
<progress id="file" max="100" value="70">70%</progress>

<!-- header>nav^footer or (header>nav)+footer-->

<header>
  <nav></nav>
</header>
<footer></footer>
```

## SVG

- SVG resources:
  - [Material Icons](https://fonts.google.com/icons)
- Useful for simple images
- `xmlns` tells the user-agent which xml dialect it is using like `svg`, `html`, `android layout` and so on. It allows us to mix together different dialects of xml
  - `html` has implied namespace name `http://www.w3.org/1999/xhtml` and so does `svg` and `MathMl`
  - Notice that namespace name is just string even though they look like URIs. They are just unique strings to identify the dialect of xml you are using for browsers or other things to correctly parse them
- `use` element allows you to clone nodes
- `svg` element:
  - `viewport`: **portion** of svg you can **view**. It is the total area of image.
    - It can be controlled via `width` and `height` attribute of svg element
  - `viewBox min-x min-y width height`: It is actually like a window to viewport which you can move around or use to zoom-in and zoom-out.
    - **min-x** and **min-y** represent the top-left corner of the viewBox
    - **width** and **height** represent height of the viewBox
    - In the example below, we have image of 100\*100 but we will be viewing only the top-right quarter of the total image
    - Making **viewBox** bigger than **viewport** => zooming-out
    - Making **viewBox** smaller than **viewport** => zooming-in

```svg
<svg viewBox="50 0 50 50" width='100' height='100'>
  <circle cx="50" cy="50" r="50" fill="yellow"/>
</svg>
```

## Tables in HTML

- `td` (or `th` for headers) represents a cell in table
  - Both accept `colspan` and `rowspan` attribute which accepts a number of rows or columns cell to spanned
  - You can have `th` as header for both rows and cols. For screen readers to make this distinguishing, use `scope='col|row'` attribute
- Styling can be easily applied to each column by using `col` and `colgroup` elements
- NOTE: Styles applied to the cells are painted on top of the table. So cell's style will always override columns style
- `caption` is like title of table
- `thead` (generally first row), `tbody` and `tfoot` (generally last row providing summary) do not add enhancement and aren't useful for screen readers as well. They are used mostly as hooks for styling.
  - `tbody` is always included either implicitly or explicitly. Keep it mind as it messes up while using `nth-child` selector on `table`
  - `tfoot` will always be at bottom and `thead` always at top no matter where you declare them in html
- The `border-collapse` css property sets whether table borders should collapse into a single border or be separated.
- You can make any element behave as table element using `display: table|table-row|table-cell|table-column and many more`
- TIPS:
  1. Keep the text **left-aligned** (or **right-aligned** aligned for arabic-like languages) for both `td` and `th`.
  2. Keep the numbers **right-aligned**. Try to align decimal point by making same amount of decimal points
  3. Keep borders to minimum (1px lightgray at max)
  4. For more tips: [Pencil and Paper's article on tables](https://www.pencilandpaper.io/articles/ux-pattern-analysis-enterprise-data-tables)

```html
<table style="border-collapse: collapse">
  <caption>
    My Beautiful Table
  </caption>
  <colgroup>
    <col />
    <!-- span attribute specifies number of columns to apply style to -->
    <col style="background-color: yellow" span="2" />
  </colgroup>
  <thead>
    <tr>
      <th>&nbsp;</th>
      <th>Column 1</th>
      <th>Column 2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="2">Cell 1</td>
      <td rowspan="2">Cell 2</td>
    </tr>
    <tr>
      <td>Cell 1</td>
      <td>Cell 2</td>
    </tr>
  </tbody>
</table>
```

- `HTMLTableElement` API makes it much easier to create tables using JS. Here is an example of 20\*10 grid

```js
const table = document.createElement("table");
const tbody = document.createElement("tbody");
const body = document.querySelector("body");
body.appendChild(table);
table.appendChild(tbody);
for (let r = 0; r < 20; r++) {
  tbody.insertRow(r);
  for (let c = 0; c < 10; c++) {
    const cell = tbody.rows[r].insertCell(c);
    cell.appendChild(document.createTextNode(`(${r}, ${c})`));
  }
}
```

## CSS Reset

- To deal with styling inconsistencies across browsers, we have three main approaches:
  1. **Normalize**: Applying a normalize stylesheet means fixing the inconsistencies between browsers while still broadly retaining their default set of styles
     - e.g [modern-normalize](https://github.com/sindresorhus/modern-normalize)
  2. **Reset**: A reset stylesheet is the nuclear option: it removes margins, paddings, and text styles across the board. HTML elements completely lose their visual differentiation: `<p>` no longer looks like a paragraph, `<h1>` no longer looks like heading, `<ul>` is no longer a bulleted list, and so on.
  3. **Hybrid**: Combination of normalize and reset
- [Explanation of following custom reset:](https://www.joshwcomeau.com/css/custom-css-reset/)

```css
*,
*::before,
*::after {
  box-sizing: border-box; /*default: content-box i.e width = width of content*/
}

* {
  margin: 0;
}

body {
  line-height: 1.5; /* Means each line be 50% larger than the element's font-size*/
  -webkit-font-smoothing: antialiased; /* MacOS Specific*/
}

img,
picture,
video,
canvas,
svg {
  display: block;
  max-width: 100%;
}

/* By default, buttons and input don't inherit styles but apply their own */
input,
button,
textarea,
select {
  font: inherit;
}

/* In case no soft-wrap is possible, use hard-wrap rather than overflowing*/
p,
h1,
h2,
h3,
h4,
h5,
h6 {
  overflow-wrap: break-word;
  hyphens: auto; /* adds a hyphen when a word is broken in middle*/
}
```

## CSS Units

- **Absolute Units:** Units that are same in all contexts
  - e.g `px` (recommended, 0.27 mm), `in` (96px), `cm` (37.8px), `mm`, `Q` (1/4 of mm) and so on
  - Except `px` other values are only used for print
- **Relative Units:** Units that depend on the context
  - `em` and `rem` (prefer) both refer to font size
    - `1em` is the `font-size` of element (or element's parent if you are using it to set `font-size` itself). Thus `4em` means **4 \* font-size**.
    - `1rm` is the `font-size` of root element (`:root` or `html`)
  - `vh` is the `1%` of viewport height and `1vw` is the `1%` of the viewport width
  - `vmin` is the `1%` of smaller dimension of viewport and `vmax` that of larger dimension
  - Similar to **em** and **rem** are `lh` and `rlh`, the first is relative to **line height** of the element itself and the second to the **line height** of root element
    - This is especially useful for text decoration
  - `%` is also a relative unit
- TIPS:
  - Use something like `font-size: calc(16px + 0.5vw)` (and `line-height`) for responsive typography
  - Use `rem` for font-size
  - For width prefer `%` together with max-width
  - For height (do you really need to set a height?), prefer min-height
  - For padding and margin, `rem`/`em` if you want padding/margin to grow with text (like in buttons)
  - 75 characters per line and be happy

> - PSST: `rgb(r g b / a)` where **a** is opacity
> - Using `opacity: a` controls opacity of element and everything in it while as `rgb(r g b / a)` only controls the opacity of color
> - `background-image: url(https://google.com/someimage)` can be used for images

## Font Face

- Web Fonts are fonts which are not available on user's device. They can be imported vai `link` tag, `@import` tag or by locally hosting them and defining a custom font face using `@font-face` rule
- For a natural look, use system font stack like this one `system-ui, "Segoe UI", Roboto, Helvetica, Arial, sans-serif`
- **OpenType** (.OTF) and **TrueType** (.TTF) are the most common file formats for fonts

```css
@font-face {
  /* used when font-weight is normal*/
  font-family: sick-font;
  font-weight: normal;
  src: url(.../cool-font-normal.woff2) format(woff2);
}

@font-face {
  /* used when font-weight is bold*/
  font-family: sick-font;
  font-weight: bold;
  src: url(.../cool-font-bold.woff) format(woff2);
}
```

- For smaller devices, prefer small text and for larger devices prefer large text
  - Use something like `clamp(1rem, 0.75rem + 1.5vw, 2rem)` to achieve it
- Use `max-inline-size: 66ch` for a container to force container to only get as wide as 66 characters take
  - `max-inline-size` is basically `max-width` if writing-mode is horizontal and it is `max-height` otherwise

## Text Style

- Use `em` when you want to give text semantic meaning, for styling only use css `font-style: italic`
- `line-height`: adds space b/w wrapped lines
- `letter-spacing`, `text-transform: capitalize|uppercase|lowercase|none`
- `text-indent: 10px` indents the first line of text
- `text-shadow: offset-x | offset-y | blur-radius | color` (color can be in beginning as well)
- Ellipses trick:

```css
.overflowing {
  white-space: nowrap; /* do not wrap */
  overflow: hidden; /* hide overflown part */
  text-overflow: ellipsis; /* show ellipses if text overflows */
}
```

## More CSS Properties

- `background`:
  - Shorthand for 8 different background-related properties
    - Each layer can have **0 or 1** occurances of:
    - `<attachment>`: Position of background w.r.t viewport
    - `<bg-image>`
    - `<box>`
    - `<position>`:
      - **10% 50%** means 10% to the right and 50% to the bottom
      - **bottom 10% right 50%** means 10% from bottom and 50% to the right
      - **left|right|center|top|bottom**
    - `<bg-size>`
    - `<repeat-style>`: **no-repeat | repeat-x | repeat-y | space | round | space repeat**
  - You can have multiple background layers
  - Background color can be only specified in last layer
  - Anything you don't specify will be set to its default value

```css
body {
  background: url(sweettexture.jpg) /* image */ top center / 200px 200px /* position / size */
    no-repeat /* repeat */ fixed /* attachment */ padding-box /* origin */ content-box /* clip */
    red; /* color */
}
```

- `border`:
  - `outline` does not take space and that is how they differ from border
  - Border style can be: `none | hidden | dotted | dashed | solid | double | groove | ridge | inset | outset`
- `border-radius`:
  - Use this amazing tool to generate organic shapes: [Fancy-Border-Radius](https://9elements.github.io/fancy-border-radius/)
  - Read their [this](https://9elements.com/blog/css-border-radius-can-do-that/) article for little more insight

```css
/* right and scale down image to fit container */
background: right / contain no-repeat url("image.svg");
/* scaled down, repeating image */
background: center / 10% url("image.svg");
/* blue-bg => gradient layer => repeating image => centered image */
/* prettier-ignore*/
background: center / 10% no-repeat url("images/settings.svg"),
            center / 50% repeat-x url("images/touch.svg"), 
            linear-gradient(red, transparent, yellow),
            rgb(0 0 255 / 0.4);
```

- `box-shadow: inset offset-x offset-y blur-radius spread-radius color`:
  - Only 2 values => x and y and color
  - Only 3 values => x and y and blur-radius and color
  - Only 4 values => x and y and blur-radius and spread-radius
  - color and inset keywords are optional. inset makes shadow from drop-shadow to inner shadow
  - Multiple shadows can be provided by comma separated list
- `overflow: hidden | visible (def) | clip | scroll | auto`: controls how element should behave when content does not fit
  - If only one keyword is passed then both `overflow-x` and `overflow-y` are set to that value
  - If two keywords are given then they are distributed
  - `auto` is exactly `scroll` if content is overflowing otherwise scroll bars are hidden in `auto` while shown in `scroll`

```css
/* Content can overflow up to 20px after which it will be hidden*/
.box {
  overflow: clip;
  overflow-clip-margin: 20px;
}
```

## CSS Selectors extended

- Following two examples are equivalent
- `&` is kind of placeholder

```css
p {
  /* style paragraphs */
  & img {
    /* & is optional*/
    /* styles images inside paragraphs */
  }

  & ~ img {
    /* style images which are siblings of paragraphs */
  }

  & > img {
    /* style images which are direct children of paragraphs */
  }
}
```

```css
p {
  /* style paragraphs */
}
p img {
  /* styles images inside paragraphs */
}
p ~ img {
  /* style images which are siblings of paragraphs */
}
p > img {
  /* style images which are direct children of paragraphs */
}
```

## Pseudo Selectors (classes and elements)

- A pseudo-class is a selector that selects elements that are in a specific state, e.g. they are the first element of their type, or they are being hovered over by the mouse pointer.
  - They tend to act as if you had applied a class to some part of your document
- Pseudo-elements act as if you had added a whole new HTML element into the markup, rather than applying a class to existing elements.
  - The ::before and ::after pseudo-elements enable you to insert content into the document using CSS
- In nutshell, pseudo-classes represents just another state of an element while as pseudo-element represents an actual element in ODM
  - e.g `input::placeholder` represents actual element (placeholder text of input) while as `input:hover` represents state of input when hovered
- RegEx can be used with Attribute selectors:
  - `^=` for matching start of attribute value
  - `$=` for matching end of attribute value
  - `*=` for matching substring of attribute value
  - `~=` for making space as decimeter
  - `|=` for making hyphen as delimiter
- A pseudo-element is a ‘fake’ element, it isn’t really in the document with the ‘real’ ones. Pseudo-classes are like ‘fake’ classes that are applied to elements under certain conditions

```css
/* star before div content */
div::before {
  content: "* ";
}

/* small rectangle at the end of div content*/
div::after {
  content: "";
  width: 5px;
  height: 20px;
  background-color: red;
  display: inline-block;
}

/* manipulate selected text */
::selection {
  background: black;
  color: red;
}

/* matches the root of document, usually <html>. It is the only element with no parent */
:root {
}

/* select all divs which are (2n+1)th children of their parent */
div:nth-child(2n + 1) {
}

/*  equivalent to `p[type="button"]:nth-child(2)`*/
:nth-child(2): is(input[type= "button"]);

/* select first div */
div:nth-of-type(1) {
}

/* match divs which are only child of their parent */
div:only-child {
}

/* applies to all links */
a {
}

/* applies to unvisited links */
a:link {
}

/* applies to visited links */
a:visited {
}

/* style element's bullets or numbers */
li::marker {
}
```

## Specificity Revisit

- `style` > `id` > `class, pseudo-class, attribute` > `elements, pseudo-elements`
  - See examples of calculating it [here](https://css-tricks.com/specifics-on-css-specificity/)
  - `*` has no specificity
  - `:not(...)`, `:is(...)` etc. have no specificity of their own but what is passed to them
  - `!important` can be only overridden by its own kind (not even by inline-style)

## Position

- NOTE: `left`, `right`, `top` and `bottom` will be referred as offsets here

1. `static`: default
2. `relative`:

   - similar to **static** but now you can use **offsets**
   - **Offsets** are with respect to original position of element
   - Rest of document does not notice any change
   - `z-index` is unlocked

3. `absolute`:

   - Element is removed from normal flow of document
   - **Offsets** are with respect to **containing block**
     - If any ancestor has position other than static then it becomes the **containing block**
       - PS: `position: relative` is generally used for making a parent a containing block
     - `width: 50%` means 50% width of 'containing block'
     - `z-index` is unlocked
     - **margins** are added to the offset

4. `fixed`:

   - **Offsets** are relative to **viewport** so element is fixed even if you scroll the page
   - They act like normal elements until you scroll them
   - They can leave their container

5. `sticky`:

   - Similar to static but when you scroll it sticks
   - **Offsets** control when it will stick
     - e.g `nav {top: 100px; /* stick when element is 100px away from top of nearest scrolling ancestor*/}`
   - Items won't get out of their parent
   - Item sticks to it nearest scrolling ancestor
   - Really useful for navigation bars

- The main advantage of **fixed** over **sticky** is that a fixed element stays always visible while as sticky element stays visible only until its parent is visible
- The main advantage of **sticky** over **fixed** is that a sticky element does not get out of normal flow so it is hide content beneath it which is not the case with fixed

## CSS Variables (or Custom Properties)

- `var(--custom-property, fallback-value)`: You can do something like `var(prop1, var(prop2, fallback))`
- Scope of custom property is the selector it was declared into and any descendant of that selector. In other words, custom properties (by default) cascade
- You can use theme to create themes as follows

```css
:root {
  /* vars for light theme*/
}

/* applied when os's theme is dark */
@media (prefers-color-scheme: dark) {
  :root {
    /* vars for dark theme */
  }
}
```

- You can control inheritance of custom properties using `@property` at-rule

```css
@property --bg-color {
  syntax: "<color>";
  inherits: false; /* do not cascade */
  initial-value: coral; /* acts like a fallback value */
}

.parent {
  --bg-color: red; /* Will be available only to elements with container class */
  background-color: var(--bg-color); /* red */
}

.child {
  background-color: var(--bg-color); /* coral as inheritance is disabled*/
}
```

- If invalid values of css properties are provided, browser behaves as if the declaration did not exist. However when browser encounters an invalid `var()` substitution, then `intial` or `inherited` value of the property is used

```css
:root {
  --bg-color: red;
  --font-color: 16px;
}

div {
  background-color: red;
  color: green;
}

div {
  background-color: 18px; /* will be red */
  color: var(--font-color); /* will be black (and not green)*/
}
```

- You can access css variables declared in element (or its parent, if inheritable) in js via following methods
  - `element.style.getPropertyValue('--bg-color')`
  - `element.style.setPropertyValue('--bg-color', 'red')`

## Compatibility with different browsers

- [caniuse.com](caniuse.com) is an excellent website to check which browsers support newly added features

## Forms

- In the form below, `label` is for accessibility and `name` attribute is for code know what the data of `input` element represents
- `textarea` accepts text that spans multiple lines
- Use `select` for 6 or more elements
- Use `radio` for 5 or fewer elements
- Default type of `button` in form is **submit**.
- `fieldset` along with `caption` can be used to separate form into logical units

- Examples of different form elements

```html
<form action="https://httpbin.org/post" method="POST">
  <!-- Input element -->
  <div>
    <label for="first_name">First Name</label>
    <input type="text" name="first-name" id="first_name" placeholder="Junaid..." />
  </div>

  <!-- Select -->
  <select name="Car" id="car">
    <optgroup label="Expensive">
      <option value="tesal">tesala</option>
      <option value="bmw">BMW</option>
    </optgroup>
    <option selected value="alto">Alto-800</option>
    <!-- Selected -->
  </select>

  <!-- Radio button -->
  <fieldset>
    <legend>Select OS You want to use</legend>
    <div>
      <label for="mac">Mac OS</label>
      <input type="radio" name="os_type" id="mac" />
    </div>
    <div>
      <label for="win">Windows</label>
      <input type="radio" name="os_type" id="win" />
    </div>
    <div>
      <label for="linux">Linux</label>
      <input checked type="radio" name="os_type" id="linux" />
      <!-- CHECKED -->
    </div>
  </fieldset>

  <!-- Checkbox -->
  <div>
    <label for="realme">Realme</label>
    <input checked type="checkbox" name="phone" id="realme" />
  </div>
  <div>
    <label for="mi">Xiomi</label>
    <input type="checkbox" name="phone" id="mi" />
  </div>
  <div>
    <label for="iphone">IPhone X</label>
    <input type="checkbox" name="phone" id="iphone" />
  </div>

  <!-- Buttons -->

  <button type="submit">Submit Form</button>
  <button type="reset">Reset Form</button>
</form>
<textarea name="comment" id="comment-box" rows="20" cols="60"></textarea>
```

### MDN Forms

- Form controls are also called widgets
- UX (user experience)
- `input[type='button']` will allow only plain text as it label while as `button` allows full HTML content
- Form controls outside the form can be linked with a form using `form` attribute
- You can nest a form element inside `label`. Use of `for` attribute is till recommended
- Lists are often used to wrap form controls and their labels. They are recommended for radio buttons and checkboxes
- You cannot style text within `input` elements. Create custom widgets for that
- Different attributes of input
  - `type` can be `tel` (for telephone)
  - `type` can be `number` (for floating point numbers)
    - You can use `min`, `max` and `step` as well
    - You can use (quite surprisingly) above attributes with `input[type='date']` as well
  - `pattern` with regex as value
  - `maxlength` number of characters of input
  - `spellcheck`
  - `readonly`: input will not be allowed to edit but its value will be sent
  - `disabled`: input will not be allowed to edit neither will be its value sent
  - `list` can used to add autocomplete (see example)
    - Adding list to `input[type='range']` will add tick marks at those points
    - You can even make labels from **datalist** see [this MDN paragraph](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/range#adding_labels) for more info
- For `input[type='email']`:
  - You can add `multiple` attribute to allow multiple emails to be entered
  - When wrong email is entered pseudo-class `:invalid` matches
- `input[type='hidden']` is hidden from user and is used to send some additional data to the server (e.g timestamp)
- Checkboxes and radio buttons with `checked` attribute match the `:default` pseudo-class
- `input[type='image']` is like a son of `img` and `input`. It supports all attributes of both elements
  - When this input is used to submit the form, coordinates of click are sent with data
  - Useful for creating hot maps
- `textarea`:
  - `resize: none|both|horizontal|vertical` can be used to risibility of text area

```html
<input type="text" list="my-sugg" />
<datalist id="my-sugg">
  <option>SickBoy</option>
  <option>ByteBolt</option>
</datalist>
```

> PS: You can make any elements content editable using `contenteditable='true'` property

### Some extra widgets

- `progress`:
  - without value, its in indeterminate state
  - no `min` attribute that is always 0
- `meter`: Kind of progress-bar with added benifit of color coding
  - e.g `<meter optimum="10" low="40" high="90" min="0" max="100"></meter>`
  - There are 3 ranges `min-low`, `low-high` and `high-max`
    - range containing `optimum` will be green
    - range immediately before and after `optimum` will be yellow
    - range other than above (if any (at most 1 range)) will be red
  - Here `0-40` (contains optimum) will be green, `40-90` will be yellow and `90-100` will be red

### Styling forms

```css
button,
input,
select,
textarea {
  font-family: inherit;
  font-size: 100%;
}
```

- `appearance: none`: stops os-level styling. Especially useful with checkboxes and radio buttons
- Elements on which css styling does not have any control are called **replaced elements.**
  - Like `img`, `iframe`, etc
  - Two main properties:
    - `object-fit:`
      - `contain`: Aspect-ratio maintained and fitted as in container as large as possible
      - `fit`: fitted in container but aspect-ratio lost
      - `cover`: fitted in container and aspect-ratio maintained but clipped
    - `object-position:`
      - left, right, top, bottom
- The `choose file` button of `input[type=file]` is completely unstylable. Even font cannot be changed. However, the label can be styled
