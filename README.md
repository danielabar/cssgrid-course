<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [CSS Grid](#css-grid)
  - [Setup](#setup)
  - [CSS Grid Fundamentals](#css-grid-fundamentals)
  - [CSS Grid Dev Tools](#css-grid-dev-tools)
  - [Implicit vs Explicit Tracks](#implicit-vs-explicit-tracks)
  - [Grid auto-flow Explained](#grid-auto-flow-explained)
  - [Sizing Tracks](#sizing-tracks)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# CSS Grid

> My notes from CSS Grid [course](https://cssgrid.io/) with Wes Bos.

[#griddyup](https://twitter.com/hashtag/griddyup?src=hash)

Course will use Firefox - best CSS Grid debugging tools. Recommend Developer Edition.

## Setup

[Starter Files + Solutions](https://github.com/wesbos/css-grid)

Will use browsersync to auto refresh browser.

Run `npm install`, then `npm start` to open FirefoxDeveloperEdition at [localhost:7777](http://localhost:7777/)

Note base stylesheet uses css variables:

```css
code:root {
  --yellow: #ffc600;
  --black: #272727;
}
```

## CSS Grid Fundamentals

[Example](03 - CSS Grid Fundamentals/css-grid-fundamentals-START.html)

Take any element such as `<div>` and add `display: grid` in css. Then you can slice/dice grid into a grid of rows and columns, where items can be placed *anywhere* in grid. Can also span multiple rows/cols.

Rows/cols are *tracks*. Content can be laid out within tracks without css positioning.

Emmet trick to get 10 divs with values 1 through 10 inside of them:

```
.item{$}*10
```

```html
<div class="item">1</div>
<div class="item">2</div>
<div class="item">3</div>
<div class="item">4</div>
<div class="item">5</div>
<div class="item">6</div>
<div class="item">7</div>
<div class="item">8</div>
<div class="item">9</div>
<div class="item">10</div>
```

When putting `display: grid` on a div (container), all its direct children become css grid item (no need to add display property on the direct children).

Define columns, for example to specify 3 columns each of width 100px:

```css
.container{
  display: grid;
  grid-template-columns: 100px 100px 100px;
}
```

Rows are automatically created as child items fill up the columns (more on *implicit grid* later).

`grid-gap: 20px` will add space between each column and row. Similar to margin, adds space between each *track*.

If column width is defined as wider than content inside it, by default, items inside will stretch to column width.

Can mix and match units on column widths, for example, `rem`, percentages (but don't use, will have `fr` unit to replace that).

Can also use `auto` for column widths:

```css
.container {
  display: grid;
  grid-template-columns: 100px auto 500px 50px;
}
```

Second column will automatically take up whatever leftover space after the fixed width columns are laid out. Can use for fluid/responsive layout. Good for common layout such as fixed sidebar (eg: 300px), then main content area, "the rest" of the view.

Can use `repeat` function, for eg: 5 columns of 100px each:

```css
.container {
  display: grid;
  grid-template-columns: repeat(5, 100px);
}
```

Can also specify rows:

```css
.container {
  display: grid;
  grid-template-rows: 200px 100px 400px;
}
```

Note this will make items go all the way across because there are no more columns specified. If you don't specify number of columns, then only get one.

## CSS Grid Dev Tools

[Example](04%20-%20CSS%20Grid%20Dev%20Tools/dev-tools-START.html)

Difficult to visualize because grid parts such as columns, rows, tracks, gaps etc are not DOM elements so they can't be inspected like regular DOM elements.

Right-click to inspect grid element. In Rules tab where `display: grid` is shown, can click on grid icon to toggle CSS Grid Highlighter.

Also on Layout tab, shows all the grids. Clicking the checkbox beside the grid in Layout tab will also toggle CSS Grid Highlighter.
Displays lines overlapping on grid. Displays where tracks are.

In Grid Display Settings:
- always check `Display line numbers` - shows row and column numbers as tags,
- `Display area names` - we don't have any yet, will get to that later.
- `Extend lines infinitely` - extends track lines to end of viewport, can be difficult to see on top of what you already have, but can modify the line colors by clicking on color circle beside grid item in Layout tab. Easiest to see in most layouts is black.

To distinguish various patterns in the lines (can be subtle), use Mac zoom accessibility (Cmd Option +/-) to zoom entire display in/out.

Note if create a grid with two columns, numbered labels from dev tools will show: 1, 2, 3. Tracks are numbered not by column/row itself, but by lines that start and stop them, so will always be one more number displayed than actual number of columns or rows.

Notice there are different kinds of lines:

- Solid line: Start and stop of *explicit* grid.
- Dashed diagonal line: Gaps
- Dashed line: Explicit track (eg: dark dashed lines going vertical are explicitly created columns)
- Dotted lines: Implicit track (eg: if only have columns but enough items that it creates new rows, those are implicit)

## Implicit vs Explicit Tracks

In the example below, there are *explicitly* defined columns, but rows have not, therefore the rows are *implicit*:

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 200px 400px;
}
```

So if html has more than two grid items to be placed in the grid container, for eg:

```html
<div class="container">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
</div>
```

Browser will place:
- first item in first column (explicit)
- second item in second column (explicit)
- third item gets wrapped, creating a second row (implicit)
- fourth item gets wrapped, creating second row, (implicit)

Note how the dotted lines in dev tools for implicit change to dashed lines for explicit if css is modified to also explicitly define rows:

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 200px 400px;
  grid-template-rows: 100px 200px;
}
```

If more items are added than can fill the explicit grid, will expand grid beyond the explict end point (solid line), and create an implicit row to place the additional items.

To size implicity created rows:

```css
.container {
  ...
  grid-auto-rows: 500px;
}
```

Note: Open FF issue - unable to define size of multiple implicit rows, this will get crossed out, but it does work in Chrome:

```css
.container {
  ...
  grid-auto-rows: 500px 200px;
}
```

Also have `grid-auto-columns: 100px;`

But just adding that doesn't do anything in this example. By default, define explicit columns, and any extra items are turned into rows. `grid-auto-flow` can change this behaviour (more in next section).

## Grid auto-flow Explained

[Example](06%20-%20CSS%20grid-auto-flow%20Explained/autoflow-START.html)

Set whether another row or column should be added after explicit items have been laid out and getting into implicit items. Default is `grid-auto-flow: row`. Then additional items create new implicit rows. But `grid-auto-flow: column` will place additional items in new columns (enough new columns will scroll horizontally).

Given `grid-auto-flow: column`, then can also use `grid-auto-columns: 200px;` to specify how wide implicitly created columns should be.

Similar to `flex-direction` in Flexbox.

## Sizing Tracks

[Example](07%20-%20Sizing%20tracks%20in%20CSS%20Grid/sizing-tracks-START.html)

Trying to use percentage sizing on columns will create some horizontal scroll, due to gaps, eg:

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 25% 25% 25% 25%;
}
```

A better approach is `fr` (fractional) unit. fr represents the amount of space left *after* all elements are laid out. eg,
below will use 100% of remaining width for 3rd column, after first two columns of 200px each are laid out.

```css
.container {
  ...
  grid-template-columns: 200px 200px 1fr;
}
```

Below will first lay out first column of 200px, then divide remaining space equally between 2nd and 3rd columns.

```css
.container {
  ...
  grid-template-columns: 200px 1fr 1fr;
}
```

`fr` units work similarly to `flex-grow` and `flex-shrink` in Flexbox. i.e. `fr` units are in *proportion* to how much free space is left.

Below, 2nd column will get twice as much of the free space left as first column.

```css
.container {
  ...
  grid-template-columns: 200px 2fr 1fr;
}
```

If want all columns evenly distributed across free space, don't use pixels at all, use `fr` units:

```css
.container {
  ...
  grid-template-columns: 1fr 1fr 1fr 1fr;
}
```

Note adding `grid-template-rows: 1fr 1fr 1fr 1fr;` won't immediately change anything. Because default `height` of grid is however high the content is, but default width is viewport width.

To see effect of `grid-template-rows`, can add explicit height to grid, in this case 2nd row will take up 10x more free space than the others, distributed across a height of 600px, minus grid gap:

```css
.container {
  height: 600px;
  border: 10px solid var(--yellow);
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 10fr 1fr 1fr;
}
```

Can also use `auto` keyword combined with `fr`. `auto` will adjust column to max size of the content (i.e. ALL items in the column will get wider), then `1fr` will use all of the remaining free space:

```css
.container {
  ...
  grid-template-columns: auto 1fr;
}
```