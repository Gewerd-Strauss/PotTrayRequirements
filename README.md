
<!-- README.md is generated from README.Rmd. Please edit that file -->
<!-- In that case, don't forget to commit and push the resulting figure files, so they display on GitHub and CRAN. -->

# PTR

<!-- badges: start -->
<!-- badges: end -->

This package includes all functions required for a simplistic
determination of the required board-space for a given number of pots.
This is by no means exhaustive, and by no means the best algorithm for
this kind of packing-problem.

## Installation

You can install the development and release versions of PTR like so:

``` r
# release
devtools::install_github("https://github.com/Gewerd-Strauss/PTR@master")
# development
devtools::install_github("https://github.com/Gewerd-Strauss/PTR@dev")
```

# Documentation

A Tray-Layout can be defined given the required number of pots, their
dimensions, as well as the dimensions of a board.

## Structure of a shelf

The functions in this package *technically* work on an abstract board of
any dimension. However, it is designed for the assumption that a `board`
constitutes the area marked in green (denoted by `(3)`). Obviously, you
can just decide that a `(2)` is a board, or even `(1)`. Up to you.

<img src="man/figures/shelf_overview_complete.png" alt="An empty shelf. An entire level is marked as a '1', a side as '2' and a board as '3'." width="100%" />

## Defining boards for circular & square pots

``` r
library(PTR)
## Circular pots
circle_pots <- PTR_generateBoardLayouts2(
  pots = 64,
  board_width = 30,
  board_height = 60,
  pot_radius = 5,
  distance = 0,
  lbls = paste0("circle_", 1:64),
  pot_type = "circle"
);
## Square pots
square_pots <- PTR_generateBoardLayouts2(
  pots = 64,
  board_width = 30,
  board_height = 60,
  pot_radius = 5,
  pot_diameter = 5,
  distance = 0,
  lbls = paste0("square_", 1:64),
  pot_type = "square"
)
```

The results of `PTR_generateBoardLayouts2()` has the principal structure

``` r
ret
  $board_{n}
           $points      ## a list denoting the x/y-coordinates of the center point of a pot-graphic
           $board_plot  ## the plot which is the main result of the function
           $input       ## a list denoting the input data
           $version     ## an internal versioning number to be used in various function processing
```

## Defining boards for rectangular pots

Rectangular pots are defined similarly. However, they require additional
arguments:

``` r
## Rectangular Pots
rectangle_pots <- PTR_generateBoardLayouts2(
  pots = 17,
  board_width = 30,
  board_height = 60,
  pot_radius = NA,
  pot_diameter = NA,
  pot_rectangle_width = 10,
  pot_rectangle_height = 12,
  distance = 0,
  lbls = paste0("rectangle10x12_", 1:17),
  pot_type = "rectangle"
)
## Flipping the pot-dimensions 
rectangle_pots_flipped <- PTR_generateBoardLayouts2(
  pots = 17,
  board_width = 30,
  board_height = 60,
  pot_radius = NA,
  pot_diameter = NA,
  pot_rectangle_width = 12,
  pot_rectangle_height = 10,
  distance = 0,
  lbls = paste0("rectangle12x10_", 1:17),
  pot_type = "rectangle"
)
rectangle_pots_not_flipped <- PTR_generateBoardLayouts2(
  pots = 17,
  board_width = 30,
  board_height = 60,
  pot_radius = NA,
  pot_diameter = NA,
  pot_rectangle_width = 12,
  pot_rectangle_height = 10,
  distance = 0,
  lbls = paste0("rectangle12x10_", 1:17),
  pot_type = "rectangle",
  rectangle_enforce_given_orientation = TRUE
)
```

## Example Boards

The following figures showcase the first board for the example-codes
above:

<div class="figure">

<img src="man/figures/README-circle_plot-1.png" alt="The first board for the circle-example above. Note that the x-&amp;y-axis are not equally scaled." width="100%" />
<p class="caption">
The first board for the circle-example above. Note that the x-&y-axis
are not equally scaled.
</p>

</div>

<div class="figure">

<img src="man/figures/README-square_plot-1.png" alt="The first board for the square-example above. Note that the x-&amp;y-axis are not equally scaled." width="100%" />
<p class="caption">
The first board for the square-example above. Note that the x-&y-axis
are not equally scaled.
</p>

</div>

<div class="figure">

<img src="man/figures/README-rectangle_plot-1.png" alt="The first board for the rectangle-example above." width="100%" />
<p class="caption">
The first board for the rectangle-example above.
</p>

</div>

<div class="figure">

<img src="man/figures/README-rectangle_flipped_plot-1.png" alt="The first board for the second rectangle-example above, where the pot-dimensions are flipped relative to the first one." width="100%" />
<p class="caption">
The first board for the second rectangle-example above, where the
pot-dimensions are flipped relative to the first one.
</p>

</div>

<div class="figure">

<img src="man/figures/README-rectangle_enforced_plot-1.png" alt="The first board for the second rectangle-example above, while enforcing the dimensions of 'board_width' &amp; 'pot_width' to be locked to each other via setting 'rectangle_enforce_given_orientation=TRUE'. This option is intended to be used when rectangle-post _must_ be placed in a specific orientation, even if it doesn't fit the most pots onto a board. Notice that less pots could be fit onto the first board compared to the previous figures." width="100%" />
<p class="caption">
The first board for the second rectangle-example above, while enforcing
the dimensions of ‘board_width’ & ‘pot_width’ to be locked to each other
via setting ‘rectangle_enforce_given_orientation=TRUE’. This option is
intended to be used when rectangle-post *must* be placed in a specific
orientation, even if it doesn’t fit the most pots onto a board. Notice
that less pots could be fit onto the first board compared to the
previous figures.
</p>

</div>

## Rotating Boards

Boards can be rotated via the function `PTR::PTR_rotateBoards2()`:

``` r
circle_pots_rotated <- PTR::PTR_rotateBoards2(circle_pots,-1)
```

<div class="figure">

<img src="man/figures/README-circle_plot_rotated_forwards-1.png" alt="The first board now contains the pots of the previously-last board of 'circle_pots'." width="100%" />
<p class="caption">
The first board now contains the pots of the previously-last board of
‘circle_pots’.
</p>

</div>

<div class="figure">

<img src="man/figures/README-circle_plot_rotated_forwards2-1.png" alt="The second board now contains the pots of the previously-first board of 'circle_pots'." width="100%" />
<p class="caption">
The second board now contains the pots of the previously-first board of
‘circle_pots’.
</p>

</div>

## Rotating Pots

``` r
circle_pots_pots_rotated <- PTR::PTR_rotatePots(circle_pots,shifts = 2)
```

Pots can be rotated within a board by using `PTR::PTR_rotatePots()`:

<div class="figure">

<img src="man/figures/README-pot_rotation_plot-1.png" alt="Pots are rotated left to right, bottom to top on a per-board-basis" width="100%" />
<p class="caption">
Pots are rotated left to right, bottom to top on a per-board-basis
</p>

</div>

<div class="figure">

<img src="man/figures/README-pot_rotation_plot2-1.png" alt="Pots are rotated left to right, bottom to top on a per-board-basis" width="100%" />
<p class="caption">
Pots are rotated left to right, bottom to top on a per-board-basis
</p>

</div>

Note that a circular shift is not possible on a general board for `N`
pots, and therefore currently an index-based shift is used .
