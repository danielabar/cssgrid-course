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
  - [Repeat Function](#repeat-function)
  - [Sizing Grid Items](#sizing-grid-items)
  - [Placing Grid items](#placing-grid-items)
  - [Spanning and Placing Cardio](#spanning-and-placing-cardio)
  - [auto-fit and auto-fill](#auto-fit-and-auto-fill)
  - [Using minmax() for Responsive Grids](#using-minmax-for-responsive-grids)
  - [Grid Template Areas](#grid-template-areas)

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
.container>.item{$}*10
```

```html
<div class="container">
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
</div>
```

When putting `display: grid` on a div (container), all its direct children become css grid item (no need to add display property on the direct children).

Define columns, for example to specify 3 columns each of width 100px:

```css
.container{
  display: grid;
  grid-template-columns: 100px 100px 100px;
}
```

![three cols](assets/images/three-cols.png)

Rows are automatically created as child items fill up the columns (more on *implicit grid* later).

`grid-gap: 20px` will add space between each column and row. Similar to margin, adds space between each *track*.

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 100px 100px 100px;
}
```

![gap](assets/images/gap.png "gap")

If column width is defined as wider than content inside it, by default, items inside will stretch to column width.

Can mix and match units on column widths, for example, `rem`, percentages (but don't use, will have `fr` unit to replace that).

Can also use `auto` for column widths:

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 100px auto 500px 50px;
}
```

![auto col](assets/images/auto-col.png "auto col")

Second column will automatically take up whatever leftover space after the fixed width columns are laid out. Can use for fluid/responsive layout. Good for common layout such as fixed sidebar (eg: 300px), then main content area, "the rest" of the view.

Can use `repeat` function, for eg: 5 columns of 100px each:

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: repeat(5, 100px);
}
```

![repeat 5 100](assets/images/repeat-5-100.png)

Can also specify rows:

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-rows: 100px 50px 200px;
}
```

![rows no cols](assets/images/rows-no-cols.png)

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
  .container {
    height: 600px;
    border: 10px solid var(--yellow);
    display: grid;
    grid-gap: 20px;
    grid-template-columns: 200px 200px 1fr;
  }
}
```

![one fr](assets/images/one-fr.png "one fr")

Below will first lay out first column of 200px, then divide remaining space equally between 2nd and 3rd columns.

```css
.container {
  height: 600px;
  border: 10px solid var(--yellow);
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 200px 1fr 1fr;
}
```

![one fr one fr](assets/images/one-fr-one-fr.png "one fr one fr")

`fr` units work similarly to `flex-grow` and `flex-shrink` in Flexbox. i.e. `fr` units are in *proportion* to how much free space is left.

Below, 2nd column will get twice as much of the free space left as first column.

```css
.container {
  height: 600px;
  border: 10px solid var(--yellow);
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 200px 2fr 1fr;
}
```

![two-fr-one-fr.png](assets/images/two-fr-one-fr.png "two fr one fr")

If want all columns evenly distributed across free space, don't use pixels at all, use `fr` units:

```css
.container {
  height: 600px;
  border: 10px solid var(--yellow);
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}
```

![cols even](assets/images/cols-even.png "cols even")

Note adding `grid-template-rows: 1fr 1fr 1fr 1fr;` won't immediately change anything. Because default `height` of grid is however high the content is, but default width is viewport width.

To see effect of `grid-template-rows`, can add explicit height to grid, in this case 2nd row will take up 10x more free space than the others, distributed across a height of 600px, minus grid gap:

```css
.container {
  height: 600px;
  border: 10px solid var(--yellow);
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr;
}
```

Can also use `auto` keyword combined with `fr`. `auto` will adjust column to max size of the content (i.e. ALL items in the column will get wider), then `1fr` will use all of the remaining free space:

```css
.container {
  .container {
    border: 10px solid var(--yellow);
    display: grid;
    grid-gap: 20px;
    grid-template-columns: auto 1fr;
  }
}
```

![col auto 1fr](assets/images/col-auto-1fr.png "col auto 1fr")

## Repeat Function

[Example](08%20-%20CSS%20Grid%20repeat%20function/repeat-START.html)

Instead of specifying `1fr` four times to get four columns:

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}
```

Use `repeat` function to specify how many times you want to repeat and what should be repeated, eg:

```css
.container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}
```

![repeat 4](assets/images/repeat-4.png "repeat 4")

To get 8 columns alternating between `1fr` and `2fr`:

```css
.container {
  display: grid;
  grid-template-columns: repeat(4, 1fr 2fr);
}
```

![repeat 8](assets/images/repeat-8.png "repeat 8")

Can mix and match regular columns with repeat function:

```css
.container {
  display: grid;
  grid-template-columns: 100px repeat(2, 1fr auto) 200px;
}
```

## Sizing Grid Items

[Example](09%20-%20Sizing%20Grid%20Items/sizing-items-START.html)

Setting an explicit width on an item within a grid (or placing longer content in it) will make the entire column that wide, not just that item:

```css
.item9 {
  background: mistyrose;
  width: 500px;
}
```

![width 500](assets/images/width-500.png "width 500")

To only affect a single item, use *spanning*, to tell an item to be a specific width. eg, for a single item in a grid to take up two columns: Will start where it naturally starts and then flow into next slot:

```css
.item9 {
  background: mistyrose;
  grid-column: span 2;
}
```

![span 2](assets/images/span-2.png "span 2")

If you set span to value greater than there is space on that row, it will bump item down to next row and leave a gap:

```css
.item9 {
  background: mistyrose;
  grid-column: span 2;
}
```

![span 3](assets/images/span-3.png "span 3")

Can also span rows:

```css
.item9 {
  background: mistyrose;
  grid-column: span 2;
  grid-row: span 2;
}
```

![span row col](assets/images/span-row-col.png)

If you span more columns than are available, will force grid to be larger. eg: 5 column grid, but span 10:

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: repeat(5, 1fr);
}

.item9 {
  background: mistyrose;
  grid-column: span 10;
}
```

![col 5 span 10](assets/images/col-5-span-10.png "col 5 span 10")

## Placing Grid items

[Example](10%20-%20Placing%20Grid%20Items/placing-START.html)

`grid-column: span 2` is actually shorthand of two values: `grid-column-start: span 2` and `grid-column-end: auto`.

To specify `grid-column-start` and end explicitly, can use *track values.* For example, to start a particular item at column 2, it will layout all the items before it, then start the item at 2:

```css
.poop {
  grid-column-start: 2;
  background: #BADA55;
}
```

![col start 2](assets/images/col-start-2.png "col start 2")

Can also specify end:

```css
.poop {
  background: #BADA55;
  grid-column-start: 2;
  grid-column-end: 5;
}
```

![col start 2 end 5](assets/images/col-start-2-end-5.png "col start 2 end 5")

Short hand version of start/end:

```css
.poop {
  background: #BADA55;
  grid-column: 2 / 5;
}
```

Can also combine start/end with span, eg: start at track 2 and then span for 2 columns:

```css
.poop {
  background: #BADA55;
  grid-column: 2 / span 2;
}
```

![col start 2 span 2](assets/images/col-start-2-span-2.png "col start 2 span 2")

If don't know how many tracks there will be, can specify to end at last track, eg: start at 1 and go to end (roughly like width: 100%)

```css
.poop {
  background: #BADA55;
  grid-column: 1 / -1;
}
```

![col start to end](assets/images/col-start-to-end.png "col start to end")

To go to second last track, use `-2`:

```css
.poop {
  background: #BADA55;
  grid-column: 1 / -2;
}
```

![col start to second last](assets/images/col-start-to-second-last.png "col start to second last")

Can mix and match this with rows:

```css
.poop {
  background: #BADA55;
  grid-column: 1 / -4;
  grid-row: 3 / span 3;
}
```

![place col and row](assets/images/place-col-and-row.png "place col and row")

Note that `-1` for `grid-row` will only go to end of *explicit* grid (in this example, 5 rows):

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(5, 1fr);
}

.poop {
  background: #BADA55;
  grid-column: span 2;
  grid-row: 1 / -1;
}
```

![row to end](assets/images/row-to-end.png "row to end")

## Spanning and Placing Cardio

[Example](11%20-%20Spanning%20and%20Placing%20Cardio/get-sweaty-START.html)

Given html:

```html
<div class="container">
  <div class="item item1">1</div>
  <div class="item item2">2</div>
  <div class="item item3">3</div>
  ...
  <div class="item poop">💩</div>
  <div class="item item9">9</div>
  <div class="item item10">10</div>
  <div class="item item11">11</div>
  ...
  <div class="item item30">30</div>
</div>
```

And css:

```css
.container {
  display: grid;
  grid-gap: 20px;
}
```

***NOTE: Exercises are cumulative!***

**Make the grid 10 columns wide, every other taking up twice the free space**

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: repeat(5, 1fr 2fr);
}
```

![10 wide every other 2x](assets/images/10-wide-every-other-2x.png "10 wide every other 2x")

**Make the grid have 10 explicit rows, 50px high each**

```css
.container {
  display: grid;
  grid-gap: 20px;
  /* Make the grid 10 columns wide, every other taking up twice the free space */
  grid-template-columns: repeat(5, 1fr 2fr);
  /* Make the grid have 10 explicit rows, 50px high each */
  grid-template-rows: repeat(10, 50px);
}
```

Notice lots of extra explicit rows created (depicted by solid lines in dev tools).

![10 rows 50 high](assets/images/10-rows-50-high.png "10 rows 50 high")

**With Item 1, start at col 3 and go until 5**

```css
.item1 {
  grid-column: 3 / 5;
}
```

![col 3 to 5](assets/images/col-3-5.png "col 3 to 5")

**With Item 2, start at col 5 and go until the end**

```css
.item2 {
  grid-column: 5 / -1;
}
```

![col 5 to end](assets/images/col-5-end.png "col 5 to end")

**Make Item 5 double span 2 cols and rows**

```css
.item5 {
  grid-column: span 2;
  grid-row: span 2;
}
```

![col row span 2](assets/images/col-row-span-2.png "col row span 2")

**Make Item 8 two rows high**

```css
.item8 {
  grid-row: span 2;
}
```

![row span 2](assets/images/row-span-2.png "row span 2")

**Make Item 15 span the entire grid width**

```css
.item15 {
  grid-column: 1 / -1;
}
```

![entire width](assets/images/entire-width.png "entire width")

**Make item 18 span 4 widths, but end 9**

```css
.item18 {
  grid-column: span 4 / 9;
}
```

![span 4 end 9](assets/images/span-4-end-9.png "span 4 end 9")

**Make item 20 start at row 4 and go for 3**

```css
.item20 {
  grid-row: 4 / span 3;
}
```

![row 4 span 3](assets/images/row-4-span-3.png "row 4 span 3")

## auto-fit and auto-fill

[Example](12%20-%20auto-fit%20and%20auto-fill/auto-fit-and-auto-fill-START.html)

With `auto-fill`, do not specify how many columns, just tell grid to "figure it out" based on however much content is in each item.

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  grid-template-columns: repeat(auto-fill, 150px);
}
```

![auto fill](assets/images/auto-fill.png "auto fill")

As viewport is resized, items will jump to next line if there's no more space for them on current line.

![auto fill narrow](assets/images/auto-fill-narrow.png "auto fill narrow")

At first glance, `auto-fit` doesn't seem to do anything different than `auto-fill`. Difference is when there are not enough items to fill width or height of grid. In this case, `auto-fit` will stop explicit grid at last item, whereas `auto-fill` will continue adding more tracks to end of viewport.

Eg, if there are only 4 child items in grid:

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  grid-template-columns: repeat(auto-fit, 150px);
}
```

Notice how explicit grid ends at 4th item, i.e. it makes the grid just wide enough to "fit" the content:

![auto fit](assets/images/auto-fit.png "auto fit")

Whereas with `auto-fill`, the grid tracks keep going to fit as much of the view port as possible:

![auto fill less items](assets/images/auto-fill-less-items.png "auto fill less items")

`auto-fill` is useful for example, have a few buttons on left side, then want to place some content at the right-most of grid:

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  grid-template-columns: repeat(auto-fill, 150px);
}

.item4 {
  grid-column-end: -1;
}
```

![auto fill item end](assets/images/auto-fill-item-end.png "auto fill item end")

Note this would NOT work with `auto-fit` because that does not tack on any extra columns:

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  grid-template-columns: repeat(auto-fit, 150px);
}

.item4 {
  grid-column-end: -1;
}
```

![auto fit item end](assets/images/auto-fit-item-end.png "auto fit item end")

## Using minmax() for Responsive Grids

[Example](13%20-%20Using%20minmax%20for%20Responsive%20Grids/minmax-START.html)

Combination of auto-fill/auto-fit/minmax replaces many media queries because it makes grid responsive by design.

Problem: Given this base layout:

```html
<div class="container">
  <div class="item item1">Item 01</div>
  <div class="item item2">Bonjour!</div>
  <div class="item item3">Item 03</div>
  <div class="item item4">Item 04</div>
</div>
```

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  grid-template-columns: repeat(auto-fill, 150px);
}
```

![min max base](assets/images/min-max-base.png "min max base")

Suppose columns were only 100px wide:

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  grid-template-columns: repeat(auto-fill, 100px);
}
```

Then any content wider than 100px starts to spill out of column:

![content spill](assets/images/content-spill.png "content spill")

Don't want to have to calculate how wide columns should be, want them to "flow" as wide as they need to be to fit content.

Solution, use `minmax` function to specify the min and max column widths for the grid. Max of `1fr` means max is entire width of grid. Note don't need to explicitly tell it how many columns.

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
}
```

Now as viewport gets narrower, items will wrap to next line when they no longer fit:

![min max](assets/images/min-max.png "min max")

Using `minmax` in combination with `auto-fit`, the 4 tracks will expand to fill all available space:

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  /* grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); */
  grid-template-columns: 150px 150px 150px 150px;
}
```

![min max auto fit](assets/images/min-max-auto-fit.png "min max auto fit")

AND as view port shrinks, will wrap, until eventually get a single stacked column:

![min max auto fit narrow](assets/images/min-max-auto-fit-narrow.png "min max auto fit narrow")

Given 4 columns all 150px in a wider viewport:

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  grid-template-columns: 150px 150px 150px 150px;
}
```

![four cols 150](assets/images/four-150-cols.png "four 150 cols")

Then changing first column width to `auto` makes it take up all leftover space:

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  grid-template-columns: auto 150px 150px 150px;
}
```

![first col auto](assets/images/first-col-auto.png "first col auto")

That `auto` width column also resizes as viewport width gets smaller:

![viewport first col auto](assets/images/viewport-first-col-auto.png "viewport first col auto")

Instead of using `auto`, use `fit-content` function, which accepts a `clamp` value, i.e. max width (for column) or max height (for row) that this auto grid item can grow to. eg:

```css
.container {
  display: grid;
  grid-gap: 20px;
  border: 10px solid var(--yellow);
  grid-template-columns: fit-content(100px) 150px 150px 150px;
}
```

![fit content](assets/images/fit-content.png "fit-content")

## Grid Template Areas