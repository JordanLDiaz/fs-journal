# Bootstrap
Easiest way to get - copy css cdn tag from site
- copy into head tag, place before or after your own css link depending on if you want yours to override it or not (reccomend putting mine AFTER cdn)
- rows = display flex, column = display block
- simplifies the architecture of page, makes more mobile friendly/responsive

****In index file, type bs5-$ for shortcut input with foundation, includes cdn links for everything you need!!

Grid system
1. container 
     2. row
          3. column

sibling to container = container-fluid that will take container all the way to edge of page
- columns share the entire length of the row they are in (must equal up to 6)
- change size of col by adding col-# (#=1-12)
- you don't have to use all 12 "sections" of row, but you should if you want content to span entire width of page
- using less than 12 allows more flex space
- if you don't label with numbers, they will space columns evenly across width of page (shared evenly)
- if you go over 12, automatically moves down but stays within same element (this can be done if you need to!)
- justify-content-... as rule for centering/spacing
- other rules to play with: rounded, shadow, bg-...,
- containers = header, main, footer OR entire page could be container... depends on preference
- rows....rows...
- columns = each section of the row

- style:debug will drop style tag in html 
- adding this to body class (debug) will outline body elements on page so we can better visualize columns.

Building Codeworks website tips
- map out basics of layout first. Think containers (how many), rows (how many), columns (how many, col size?, bg color, text color), add 
- header: container-fluid, use bg-'s for color
- button: btn-btn-outline-info or similar
- columns can have small amount of text, but if its much more, best practice is to break it up into more elements
- go back and add padding (need to clarify top,bottom,left,right)
- hero image = bg-image, which is not built into bootstrap (unsplash.com is great place to find imgs!). Need to link to css to style.
  - background images need content in html to work, plus styling in css (height, width, etc)
  - min-height 90vh (vh = viewheight)
  - built in class for changing height (h-#...multiples of 25 defaulted).
- align-items-... moves around page (e.g. align-items-end moves content to end of container)
  ** Must put on items that are flex
  -instead of just typing .row (enter), can also add classes all at once (.row.bg-black.p-5, etc)
  - explore other bs$-... for other cool components....lots to explore (look at bootstrap site)

  -img breaking page? add class img-fluid to img tag. This tells it to be as big as it can be but dont break out of current container.
  - Search for other lorem ipsums (metaphor, dj khaled, meet the ipsums, etc)
  - add span w/ class to add classes random places without affecting anything else
  - can embed youtube video

  - can add more rows inside other columns, with more columns.
  - can add onclick as class in html

  Mobile View
    - add class "breakpoints"/infixes for screen size (sm, md, lg, etc) to your col class
      - md stands for anything medium screen OR LARGER (sm = small or larger, lg = large or larger, etc)
      - bootstrap has details on exact screen size plus other options (layout --> breakpoints)
      - add multple col-md-# depending on screen size to make it extra responsive and mobile friendly.
      - can also adjust padding based on screen size (EX: p-2 p-md-5)
    - change positions of items within columns in mobile view by using order-1, order-2, etc.
      - need to specify on which kind of screen (ex: order-sm-1 order-md-2, etc)