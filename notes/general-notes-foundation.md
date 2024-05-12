# Notes

- [Notes](#notes)
  - [Definitions](#definitions)
  - [Git](#git)
  - [HTML](#html)
  - [CSS](#css)
  - [Box Model](#box-model)
    - [Types of boxes](#types-of-boxes)
    - [Divs and Spans](#divs-and-spans)
  - [Flexbox](#flexbox)
    - [Axes](#axes)
    - [Gap](#gap)
    - [Misc](#misc)

## Definitions

- **Router:** To connect 10 computers you need 45 cables. Thus to limit this we use routers and connect every computer to router. Whenever a computer sends message, router routes that message to the desired destination. Thus to connect 10 computers we need only 10 wires
  - We can connect routers with other routers (as they are computers as well) to make network of networks
- **Modem:** Modem turns the information from our network into information manageable by the telephone infrastructure and vice versa so we don't have to build everything from scratch
- **Domain Name:** Alias for ip addresses
- _Web_, _email_, _IRC_, etc are some technologies built on top of internet
  - Some computers (called **Web servers**) can send messages understandable by the web browsers. This is web
  - DNS (Domain Name System) is a system that uses special servers that match up 'domain names' to the websites real 'ip address'
  - Not exactly 100% sure but i guess you can configure some domain names on your computer to specific ips (like me.sickboy.com -> 127.0.0.1)
- ISP (Internet Service Provider) is company that provides access to the internet (vai fiber-optic or wireless connections, telephone lines, etc)

## Git

- [Interactive cheatsheet](https://ndpsoftware.com/git-cheatsheet.html)
- Seven rules of a great git commit message
  1. Separate subject from body with a blank line
  2. Limit the subject line to 50 characters
  3. Capitalize the first character of subject line
  4. Do not end the subject line with a period
  5. Use the imperative mood in the subject line (spoken as if giving a command or instruction)
     - Close the door
     - Remove deprecated methods
     - Fix typo in introduction
     - Notice that all 7 seven rules themselves are written in imperative mood
     - A properly formed git commit subject line should always be able to complete the following sentence: **If applied, this commit will {subject line}**
  6. Wrap the body at 72 characters
  7. Use the body to explain what and why (and not how, code does that)

## HTML

- `strong` element makes text bold as well as marks it important (helps screen readers)
- `em` makes text italic and also places emphasis on the text
- elements at same level of nesting -> siblings
- **Description lists** are often used in glossaries. `<dl>` stands for description list, `<dt>` stands for description title and `<dd>` stands for description. You can have multiple descriptions for a single description title
- `rel` attribute describes the relation b/w current page and the linked document. `noopener` value prevents opened link from gaining access of webpage from which it was opened. The `noreferrer` value prevents the opened link from knowing which webpage or resource has a reference to it. Note that `noreferrer` implies `noopener`
- Always provide `width` and `height` attributes to `img` tag even if image is of correct size and you use css to handle it (prevents screen from flashing)
  - There are 4 main image formats in use on the web:
    1. JPG: For complex phots with no text
    2. GIF: For simple animations (trade-off limited color palette)
    3. PNG: For anything that is not a photo or animation
    4. SVG: Vector based graphics format. Can be used anywhere. More text => more size
  - Pixel-based image formats should be twice as big as you want them to appear on retina displays
  - Setting only either `width` or `height` will scale the image proportionally

## CSS

- Selectors:
  - `*` -> Universal selection
  - `div`, `h1`, etc. -> Type or element selector
  - `.class-name` -> Class Selector
  - `#id` -> ID selectors. ID cannot be repeated
  - `.class1, .class2` -> Grouping selectors, selects all elements with either **class1** or **class2** or both
  - Chaining selectors:
    - `.class1.class2` -> Select elements which contain both **class1** and **class2**
    - `.class2#id1` -> Select elements with `class2` and `id1`
    - `h1.class1` -> Select all `h1` elements with `class1`
  - Combinators:
    - Descendant combinator:
      - `.classOuter .classInner` -> Select all elements with class **classInner** and whose ancestor (direct or distant) is an element with class **classOuter**
    - Child combinator:
      - `.classOuter > .classInner` -> Same as above but ancestor must be direct parent
    - Sibling combinator:
      - `#element1 ~ .element2` -> Select all elements with class **.element2** and sibling with id **#element1**
    - Adjacent sibling combinator:
      - `#element1 + .element2` -> Select element with class **.element2** and is adjacent to element with id **#element1**
  - Attribute Selectors:
    - `p[name]`: All **p** elements with attribute **name** defined
    - `p[name="cat"]`: All **p** elements with attribute **name** having value **cat**
    - `p[name~="cat"]`: All **p** elements with attribute **name** containing word **cat** (**cute cat** will match but won't match **cats**)
    - `p[name|="cat"]`: exactly **cat** or **cat** followed by hyphen (**cute-cat** will match but **cute cat** won't)
    - `p[name^="cat"]`: starts with **cat**
    - `p[name$="cat"]`: ends with **cat**
    - `p[name*="cat"]`: contains **cat** anywhere
- Properties:
  - `color` -> font color
  - `font-weight` -> boldness of font (if supports) b/w 1 to 1000
- Cascading:

  - Cascade is what determines which rules actually get applied to our HTML
  - Here is a great article on [Cascading in css](https://2019.wattenberger.com/blog/css-cascade)
  - [Specificity calculator](https://specificity.keegan.st/)

  1. Specificity: Inline styles > ID selectors > Class selectors > Type selectors. This means that if ID selector will always win no matter how many classes you have

     - Combinators (+ ~ > space) do not add any specificity
     - If a selector has 1 ID, 1 Class and 3 type selector while other has 1 ID, 2 classes and 0 type selectors then the latter has high specificity as number of classes is greater
     - `*` has no specificity

  2. Inheritance: Properties are inherited by descendants from parents
     - Most Typography-based properties are inherited while most other properties aren't
     - Directly targeting always beats inheritances
  3. Rule Order: Last one applied wins

## Box Model

- Everything on a webpage is rectangular box with content, padding, border and margin
- Standard box model: `width` and `height` prop value define the dimens of content of box. Any padding and borders are then added to those dimensions to get the total size taken up by the box (see **box one**)
- Alternative box model: `box-sizing: border-box` causes css to use padding and border from the given height and width (see **box two**).
- NOTE: margin is not counted towards the actual size of the box in any model
- In box model, margin b/w two adjacent elements collapses and takes the value of the element:
  - largest margin value if both have positive margins
  - smallest margin value if both have negative margins
  - positive - negative, if sign of margins vary
  - NOTE: Margin collapsing takes place only in vertical direction

```css
* {
  outline: 2px solid red; // visualize box model
}

/* Box one has total 180*180 dimensions ignoring margin */
box-one {
  height: 140px; /* content height */
  margin: 3000px; /* not used in height and width by default */
  padding: 20px;
  border: 20px solid red;
}

/* Box two has total 100*100 dimensions ignoring margin*/
box-two {
  box-sizing: border-box;
  height: 100px; /* content height + padding + border */
  margin: 3000px; /* not used in height and width by default */
  padding: 40px;
  border: 10px solid red;
}
```

### Types of boxes

- You can change type of a box using `display: block/inline/inline-block`
- Block boxes: break to a new line. width is generally not specified
- Inline boxes: don't break to a new line. height and width properties will not apply.
- Inline-block boxes: don't break to a new line but respect width and height properties
- none: It is commonly used with JavaScript to hide and show elements without really deleting and recreating them.

- Inline element cannot contain block-level element

> PS: block-size and inline-size are simply width and height which depend on writing mode

### Divs and Spans

- They give no particular meaning to the content
- by default, `div` is block level and is commonly used as container and span is an inline level element and is used to group text content and inlint elements.

## Flexbox

- [Flexbox visual cheatsheet](https://flexbox.malven.co/)
- [Flexbox interactive guide](https://www.joshwcomeau.com/css/interactive-guide-to-flexbox/)
- [Flexbox with more examples](https://css-tricks.com/snippets/css/a-guide-to-flexbox/#aa-examples)
- [Flex gamed](https://mastery.games/flexboxzombies/)
- Way to arrange items in rows and columns
- items flex = grow or shrink
- some properties belong to **flex-container** and some to **flex items**
- Flex container is an element with `display: flex` and flex items are any elements that live directly inside the flex container

- (def: horizontal) -> use justify-content for alignment
- cross-axis (def: vertical) -> use align-items for alignment
- Items:

  - `flex` is shorthand for 3 properties -- `flex-grow`, `flex-shrink`, `flex-basis`
  - `flex 1` is eq. to `flex 1 1 0` i.e `flex-grow: 1`, `flex-shrink: 1`, `flex-basis: 0`
  - `flex-grow: n`: n is the growth factor
  - `flex-shrink: n`: n is the shrink factor. It is applied only when size of flex items is larger than their parent
    - `flex-shrink: 1`: (def) all items will shrink evenly
    - `flex-shrink: 0`: don't shrink the item
  - when you specify **flex-grow/shrink**, width values may not be respected
  - `flex-basis: x`: initial size of a flex item. Any sort of flex-growing or flex-shrinking starts from that baseline size
    - `flex-basis: auto`: (def) set basis to same as width
    - if you set both width and flex-basis then width is ignored

```css
/* can you predict what should be width of flex container
for all boxes to be of same size
*/
.container {
  display: flex;
  width: ??px;
}

.box1 {
  flex-basis: 100px;
  flex-grow: 3;
}

.box2 {
  flex-basis: 200px;
  flex-grow: 2;
}

.box3 {
  flex-basis: 300px;
  flex-grow: 1;
}
/*ans is 1200px*/
```

- flex items have display flex-items and not block/inline/inline-block
- width: max-content until you can fit all items then turn to width: min-content
- `gap`: property of parent => space in-between items
- `flex-grow`: if there is space to grow they will grow according this factor
- `flex-wrap: wrap/nowrap`: whether to wrap items to new line if flex box runs out of room i.e when it is about to through min-content, whether it should through min-content or just wrap to a new line
- flex-basis is sort of width
  - flex-basis 0: tries to go as small as it can i.e min-content reached
- main-axis: horizontal def
- cross-axis: vertical def
- justify-content: center/space-between/space-around/space-evenly => works along main-axis. In effect only when there is left-over space
- align-items: stretch (def)/flex-end/flex-start/center => works along cross-axis. In effect only when there are items of different heights so that there is space in vertical direction
  - `align-self: (same)` can be used in individual flex-items for more refined control
- `flex: initial`/`flex: 0 1 auto`:
- `flex: auto`/`flex: 1 1 auto`:
- `flex: none`/`flex: 0 0 auto`:
- `flex: <positive-no>`/`flex: <positive-no> 1 auto`:

### Axes

- `flex-direction: row` (def) controls orientation
- Flexbox has two axes: main axis and cross axis
- If main axis is vertical then `flex-basis` refers to height and not width

> NOTE: block elements take full-width of their parent and for default height they rely on their content

### Gap

- `gap: 10px` adds a specified amount of gap b/w flex items

### Misc

- `align-self` is align items but for flex-items
- This can be confusing, but `align-content` determines the spacing between lines, while `align-items` (which is similar to `justify-content`) determines how the items as a whole are aligned within the container. When there is only one line, `align-content` has no effect.
