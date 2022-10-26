# CSS
Set-up
  -In body, use header, main, footer elements.
  -Helps to avoid using divs too much and with readability. 
  -Most content will go in main
  - Nav bar in header
  - Not all pages will have footer

  Margin vs. padding
  -margin is the invisible space on the outside of the box
  -padding is the invisible space on the inside of the box
  - don't use margin to center things if you can help it
  - add margin rules in css with m-1 m-2, etc

  -Don't forget line type with border/outline!
    (solid, dotted, double, etc)
  
  - Use box model in dev tools to modify/play with css
  - img tags can be links to somewhere on internet, local file (need to add to local file in vscode)

  d-flex (utility class)
  -has 1 thing inside (display: flex) among others
  - this creates flex space on page that can be modified
  - create .flex-end class to add additional modifiers to our d-flex (justify-content, etc)
  - d-flex can move things beside each other

  - ul = unordered list
  - ol = ordered list
  - li = list item

  - comments = ctrl and forward slash
  - shift alt and down takes code and replicates directly below
  - paragraphs should not span across entire page (bad practice), adjust by targeting element and adding max-width
  - add card class to elements for better styling
  - if you want multiple of same line, click in that spot and then alt??
  -adjust image without stretching using object-fit: cover or center
  -text align moves text content
  - justify moves entire element, not just the content within
  -object-fit resizes img without squishing/stretching
  - hyperlinks = <a href="">    place link in "" with full https://
  - if you include new link, add target attribute

  flex vs flex-box
  * flex box targets boxes whereas flex targets element

  Find images w/transparent backgrounds
  - google images --> tools --> transparent

Images
  -can add additional attributes (beyond src and alt) such as title which allows viewers to see a comment/title about what the image shows (not the same as alt, which is what shows if img doesn't work)
  - attributes can go in any order, but helpful to keep a similar order for readability/preference
  -.pos-relative: this 
  - and .pos-absolute: position will be ignored by everything but still displayed (can tie top,left,etc to that to place at specific spot on page (think x,y cooordinates)
  - transform allows you to do a lot including translate which moves image around (need to learn more about this...confused)

  @media (screen) {} tag allows you to target how large screen is and behavior of certain elements.
  - add min or max width 
  - helps make things more mobile friendly
  @media is from react, which is built to focus on responsitivity 

  - transition is a class that goes with any changes (ease is one), helps make it more smooth
  -cursor can change what cursor looks like when it hovers over element, shows things that can be clicked



