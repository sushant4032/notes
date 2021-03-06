---
title: CSS
created: '2020-12-21T06:12:07.806Z'
modified: '2021-01-08T10:05:55.314Z'
---

# CSS
## Disable text wrapping
```css
white-space:nowrap; // or 'pre'
```

> Apr-23-2020
## grid basics   


We create a grid container by declaring `display : grid` or `display: inline-grid` on an element. As soon as we do this, all direct children of that element become grid items. 

We define rows and columns on our grid with the `grid-template-columns` and `grid-template-rows` properties. These define grid tracks. A grid track is the space between any two lines on the grid.

Tracks can be defined using any length unit. Grid also introduces an additional length unit to help us create flexible grid tracks. The new `fr` unit represents a fraction of the available space in the grid container. 

Large grids with many tracks can use the `repeat()` notation. `repeat(number, size)`.

Repeat notation can be used for a part of the track listing. 
```css

grid-template-columns: 20px repeat(6, 1fr) 20px;
 
grid-template-columns: repeat(5, 1fr 2fr);
```
## The implicit and explicit grid 


If you place something outside of the defined grid or due to the amount of content, more grid tracks are needed then the grid creates rows and columns in the implicit grid. These tracks will be auto-sized by default, resulting in their size being based on the content that is inside them.

You can also define a set size for tracks created in the implicit grid with the `grid-auto-rows` and `grid-auto-columns` properties. 
```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 200px;
}
```
## Track sizing and minmax

When setting up an explicit grid or defining the sizing for automatically created rows or columns we may want to give tracks a minimum size
```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: minmax(100px, auto);
}
```
/* Using auto means that the size will look at the content size and will stretch to give space for the tallest item in a cell, in this row. 


### Grid lines

It should be noted that when we define a grid we define the grid tracks, not the lines. Grid then gives us numbered lines to use when positioning items.

Lines are numbered according to the writing mode of the document. In a left-to-right language, line 1 is on the left-hand side of the grid. In a right-to-left language, it is on the right-hand side of the grid. Lines can also be named.

## Positioning items against lines 

When placing an item, we target the line rather than the track.

## Grid cells

A grid cell is the smallest unit on a grid. Conceptually it is like a table cell. As we saw in our earlier examples, once a grid is defined as a parent the child items will lay themselves out in one cell each of the defined grid.

## Grid areas

Items can span one or more cells both by row or by column, and this creates a grid area. Grid areas must be rectangular it. is not possible to create an L-shaped area for example. 

## Gutters

Gutters or alleys between grid cells can be created using the `column-gap` and `row-gap` properties, or the shorthand `gap`. 
 
 ```css 
.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  column-gap: 10px;
  row-gap: 1em;
}
```
/* Note: When grid first shipped in browsers the column-gap, row-gap and gap were prefixed with the grid- prefix as grid-column-gap, grid-row-gap and grid-gap respectively.

Any space used by gaps will be accounted for before space is assigned to the flexible length fr tracks, and gaps act for sizing purposes like a regular grid track, however you cannot place anything into a gap. In terms of line-based positioning, the gap acts like a fat line.

## Nesting grids

A grid item can become a grid container. nested grid has no relationship to the parent.

### Subgrid

In the working draft of the Level 2 Grid specification there is a feature called subgrid, which would let us create nested grids that use the track definition of the parent grid.

Note: This feature shipped in Firefox 71, which is currently the only browser to implement subgrid. */

```css 
.box1 {
  grid-column-start: 1;
  grid-column-end: 4;
  grid-row-start: 1;
  grid-row-end: 3;
  display: grid;
  grid-template-columns: subgrid;
}
```


## z-index

Grid items can occupy the same cell. The grid item which is defined later displays on top.
with z-index:number, it can be specified which item to be displayed on top. Highter z-index displays on top.

### CSS grid with other layour methods 

css grid is designed to work with other layout options

### grid and flexbox 

The basic difference between CSS Grid Layout and CSS Flexbox Layout is that flexbox was designed for layout in one dimension - either a row or a column. Grid was designed for two-dimensional layout - rows, and columns at the same time.

when you wrap flex items, each new row (or column when working by column) becomes a new flex container. 

The alignment properties from the flexbox specification have been added to a new specification called Box Alignment Level 3. This means that they can be used in other specifications, including Grid Layout. In the future, they may well apply to other layout methods as well.

## Alignment in CSS Grids
```html 

/* <div class="wrapper">
  <div class="box1">One</div>
  <div class="box2">Two</div>
  <div class="box3">Three</div>
</div> */
```
```css 
.wrapper {
  display: grid;
  grid-template-columns: repeat(3,1fr);
  align-items: end;
  grid-auto-rows: 200px;
}
.box1 {
  align-self: stretch;
}
.box2 {
  align-self: start;  
  /* flex-start -> start,  property name remains "align-self" */
}
```
### The fr unit and flex-basis

 if we drag our window wider and smaller, the flexbox does a nice job of adjusting the number of items in each row according to the available space. In comparison, the grid version always has three column tracks. The tracks themselves will grow and shrink, but there are always three since we asked for three when defining our grid.

 ### Auto-filling grid tracks

 We can create a similar effect to flexbox, while still keeping the content arranged in strict rows and columns, by creating our track listing using repeat notation and the auto-fill and auto-fit properties. */

```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(auto-fill, 200px);
}
 ```
/* This means that grid will create as many 200 pixels column tracks as will fit in the container.

### A flexible number of tracks

```css 
.wrapper {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}
```




