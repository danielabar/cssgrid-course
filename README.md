<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [CSS Grid](#css-grid)
  - [Setup](#setup)
  - [CSS Grid Fundamentals](#css-grid-fundamentals)
  - [CSS Grid Dev Tools](#css-grid-dev-tools)

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