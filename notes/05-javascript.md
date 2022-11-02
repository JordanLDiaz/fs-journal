i# JavaScript
Monday, October 31st, 2022

-JS file names - app.js and others, but app.js is commonly used. Depends on preference
- After setting up file, always check that css is properly loading.
- Be sure to link JS file using a script tag in bottom of body so that it loads last 
  -(as you add more and more JS, it will take longer to load, so this way you show content to page first, and THEN the js loads last)
  - <script src="app.js">
    check with console.log ("fdlsa") inside script tag
  </script>
  - Check console log with dev tools - console (MONITOR CONSTANTLY)

JS land 
  - No longer talking about content on page, so don't forget to make content strings when needed so things actually show

Data Types
- Primitive Types
  - String
    - let string = "string"
  -Numbers
    - let number = 2
    - let number2 = 54853
  Boolean (true or false)
    - let bool = true 
    - let bool = false
  Undefined
    - let dontKnow = undefined // unknown nothing (more unexpected or just variables that were never defined)
      Example: let variable1    (never defined variable)
  Null
    - let nothing = null // known nothing (confirmed nothing was there and I know that there is no value)
      - let variable2 = null ('I told you there was nothing there so why are you looking for something?')

  - Reference Types
    -Object (different from arrays, but still store info. objects store info by key, value pair. Objects don't contain a certain order, don't know which order it will go in. If order is important, don't use object)
    - link created between values
      - let obj = {} (curly brackets)
      - Example:
      let obj = {
        key: 'value'
        name: 'Mick'
        costume: 'Mega-Mind'
      }

      Dot-notation
        -obj.name // Mick
        -obj.costume // "Mega-Mind"


    - Array (items are accessed by position in array)
      - let arr = [] (array, square brackets, stores info by index/position/order, can contain strings, numbers, objects, etc.)
        - let numArr = [5, 10, 15]
        - let strArr = ['Howdy', 'Hey', 'Sup']
        - mixArr ['5', 10, ['sup]]    (not best practice!! AVOID)

    - Function data type
      - function doSomething () {
        console.log ('I did something')
      }
      do something()
      ** Don't forget to invoke function

      let funky = function () {
        console.log('this is a funny way to do it')
      }
      funky()

      let funcArr = [funky]

      Functions that return something (returns the value from function that was supplied to where function was called)
        -Example: function returnSomething() {
          return 'something'
        }

        function sum () {
          return 5 + 10
        }

How to link html buttons to js
  - Event handlers
    -Buttons
    - onclick="doSomething()"> press me </button>
    - onblur
    - onfocus (not a great one for buttons though)

    -Image
      - ondrag
      - ondragstart
      - ondragstop

Notes from tiny app
  - let secretCode = 'placed emojis here which is valid'
  - let yourCode = ''   (empty string)

  function ghost () {
    yourCode += 'ghostemoji'
    console.log(yourCode)
    drawCode()
  }
  (repeated 3 more times w/different emoji)
  - To actually run, make sure you invoke in your html and don't forget to console.log to make sure the output/data has changed as we expected it to
  - window.alert('correct')  <-- will alert user with pop-up box
  - function drawCode() {
    let codeElm = document.getElememtById('your-code)
    console.log(codeElm);
    codeElm.innerText = yourCode;
  } 

  debugger placed in curly brackets of function - allows you to pause code in action to see where bugs are 

emoji access = windows key .


Tuesday, November 1st Notes

Array Methods
- if you want to access info from array you need to go through the layers (like an onion)
Ex: catArr[2].livesLeft would return infinity (see breakroom example)


let catArr = [
  {
    name: 'Fido',
    age: 22,
    livesLeft: 0,
    color: 'calico tapioca',
    favoriteToys: ['mouse', 'hairball', 'laser']
  },
  {
    name: 'Yoshi',
    age: 3,
    livesLeft: 0,
    color: 'Grey',
  },
  {
    name: 'Morpheus',
    age: 8,
    livesLeft: Infinity,
    color: 'Brown striped',
  }
]

catArr[0].age   //22

for (let index = 0; index < catArr.length; index++) {
  console.log('looped', catArr[index])
}

// This would log data for all 3 cats

- Within for loop it gives const element = array[index]; 
    -What does this mean?
        - When it grabs array at index, it's grabbing out one element of the array
        - in this case it would be catArr[index] which is grabbing the objects (each cat) and its data. It's just assigning catArr = [index] as the element.
        - We should try to rename the 'element' to more closely match what the objects are. In this case it would be const cats. This is just an alias that makes code more clear
        - console.log('looped', cat.name) would spit out looped and the names of the cats

catArr.forEach((cat, i) => console.log(cat, i, 'for eached'))

console.log('found: ', catArr.find(cat => cat.name == 'Fido'))

Murder Mystery Notes

// Functions and Buttons
1. created animals array with objects representing each animal. 
2. updated html w/ container for lineup
3. in js, created function for drawAnimals and tested in console w/drawAnimals()
4. added animal lineup id to body
5. filtered animals by weapon type - claws
6. filtered for intellect but still returned same as claws. Need to make new function for each weapon type (filterClaws(), filterIntellect(), etc)
BREAK
1. Added more animals, then created filter functions for mammals, created buttons for mammal/not mammal
2. Created sort function for age, created buttonsfor old to young and young to old
3. created function to filter by diet (herbivores, carnivores). Diet was listed more specifically as meat/veggies, etc so we need to do extra here.
  Filter for herbivores (strictly)
  let herbivores = animals.filter(animal => (animal.diet.includes('vegetables') || animal.diet.includes('fruits')) && !(animal.diet.includes('meats') || animal.diet.includes('fishes') || animal.diet.includes('bugs')))
              // or could use && to say has both instead of || which is OR
  Filter for carnivores (strictly)
  let carnivores = animals.filter(animal => animal.diet.includes('meats') || animal.diet.includes('fishes') || animal.diet.includes('bugs')) && ! (animal.diet.includes('vegetables') || animal.diet.includes('fruits')
  Filter for omnivores (didn't mess with this but would be a good question to see how )
  let omnivores = animals.filter(animal => animal.diet.includes('meats') && animal.diet.includes('vegetables'))

4. created buttons for herbivores, carnivores, omnivores,
5. updated drawAnimals function so that it doesn't keep redrawing all animals over and over.
6. added drawSuspects function   (drawSuspects(suspects)) to target new arrays that are made when accessing the different properties

//Game Logic
1. created doCrime function - will grab the guilty property and change it to true. Added doCrime() to end of js after drawAnimals(). This is what randomizes the guilty animal. 
2. Added clues section in html and getClue () in js using find method
3. added swtich(randomClue) to getClue 

innertext vs. innerhtml   (this string is no longer text, it's html   if you switch to html. This allows you to put a p tag within the string which renders it as a p tag to the page)
- if you see an actual p tag on the page it means you didn't change it to innerhtml
- this formats the clues on the page much more nicely since it's creating new elements as clues are given. 

Using `` allows you to do string interpolation (sp?) which treats it like javascript and has to go search for the info in the object.
    e.g. clueElm.innerHTML += `<p>the murdered is ${guiltyanimal.age}<p>`

4. added accuse()

Takeaway - remember how to use .filter, sort, find, etc.

Wednesday, November 2nd

Storefront notes
- Mid fidelity wireframe provided
- designing out visual first - i like!
- don't forget the style debug
- 1 container each for header, body, footer (blue = container, red = row, green = col, purple = divs)
- within body, 1 row with a col-8 and col-4.
  - In col-8, 1 row that utilizes d-flex to stack in rows once space runs out in first "row"
  - that 1 row has col-4
  -in col-4 from earlier, just use divs.
    -notice that button is justified to end, so be sure to give it its own div as parent that can be manipulated 

Sammy's Sammies

Menu section
- put both containers first ( 1 in header, 1 in main)
- section tags for main rows are helpful for organization, as is writing good notes throughout to keep track of what I'm doing. 
- default to mobile development, add mobile view columns at same time as full screen (proactive, not reactive!)
- repeat design elements could be made as css classes to cut down on workload.
- justify-content-between for sandwich name and price, create new divs to separate them so that you can actually affect the spacing. If you have too many things contained w/i same element, it is harder to place things the way you want.

Checkout Menu section
  - anytime you want to justify/align content not w/in a container/row, be sure to d-flex
  - borders: 
    must have color, style, and width, but can be in any order. 
    Make sure it's on the correct parent element or it won't work the way you want. If you have border errors, check its in the right spot. 
    lots of bootstrap border styles 
  - Margin and padding can be used interchangeably depending on the parent element you apply it to. 

Javascript
- Now that basic structure is laid out, let's make it functional
- Want to be able to click on sandwich in menu and add it to cart
  - created array in js w/dif sandwiches. properties = name and price. 
- MAke buy function
  - code small, test small. console.log first
  - made function, then add onclick w/function to corrent element. Didn't make it a true button, but adding the onclick to a div makes it possible to interact with. This isn't best practice, but can be done. 
  - added sandwich class with cursor: pointer to img so that mouse changes when hovering over img

- Next added quantity to sandwich array properties so that we can add multiple of same items. We want to add sandwich I want to buy, and after finding the sandwich, increase the quantity. Directions need to be very explicit for computer. 
- to find, inside buyBLT function put...: let sandwich = sandwiches.find(s => s.name == 'BLT')       (s is just shorhand for the new variable sandwich that was made)
console.log(sandwich);
- still inside buyBLT function, once we've found sandwich and checked console, now we need to access the quantity of sandwich...
sandwich.quantity++
console.log(sandwich);
( this will help us add sandwiches but can only see in console.)

- Next we actually need to draw the sandwiches to the cart by checking to see if quantity is greater than 0. If it is, we need to draw to cart
- first we added more sandwiches to html and to sandwiches array, then created new functions to buy each kind of sandwich (see above process)
- note that these functions are all basically the same, so we can consolidate by making new function call buySandwich

  function buySandwich(sandwichName) {
    let sandwich = sandwiches.find(s => s.name == sandwichName)
    sandwich.quantity++
    console.log(sandwich)
  }

  this combines all the different buy buttons into just one. we can comment out the old functions, and update our onclicks in html to include buySandwich('BLT'), buySandwich('
  PBBD'), etc...

We need a way to iterate through the sandwiches selected and add them to our cart. FOR LOOP with for each
- /// test first!, then add drawCart function to buySandwich function so that it draws to cart each time a sandwich is selected.
after console logging drawing cart, s, add let template and copy html template into js function.
At this point we don't have an id, so we need to add one so we can link to js
  - add function drawCart() {
     let template = ''   <--moved let template here so that it would keep all selected in cart-->
      sandwiches.forEach(s => {
        if(s.quantity > 0) {
          console.log('drawing cart', s); 
          let template = ''    <-- took this and moved it outside forEach-->
          template += `
          <div>                 // took out div classes here just for this example
            <p>${s.name} </p>
            <p>${s.quantity}</p>   <-- add this to also be able to see quantity of each item in cart>
            <p>${s.price}</p>
          </div>
        }
      })
      document.getElementById('cart').innerHTML = template
  }

  ** At this point it's drawing to cart, but only the one that was last selected. How to keep all selected? move the let template = '' to outside of forEach (see notes above in forEach)

  How do we get total of each item? price * quantity - need to make new function and iterate through all sandwiches. if there is a sandwich, add its price/quantity to total
  - in html wehnt to menu template and added new div for total:, gave it id='total'
      <div>
        <p> Total: </p>
        <p>$ <span id='total'>0</span>
      </div>
  function drawTotal() {
    let total = 0 
    sandwiches.forEach(s =>{
      total += s.price * s.quantity      // iterate through each sandwich, add new price to the total. don't forget to multiply by s.quantity
    })
    document.getElementById('total').innerText = total.toFixed(2)     // don't need innerHTML because we're not adding HTML here, just affecting the text. toFixed specifies rounding off to 2 decimal places only so we don't get crazy decimal values. number in parentheses specifies how many decimal places we want
  }

  once this is done, we can add drawTotal to drawCart function (after getElementById), need to do this each time. 

How to actually buy sandwiches - create checkout function that clears out the cart when we click buy
- if quanitity is greater than 0, thats when it draws cart, so we want to reset quantity of all sandwiches to 0
- don't forget to add function to onclick in html

function checkout() {
  sandwiches.forEach(s => {
    s.quantity = 0
  })
  drawCart()    // since drawTotal is inside drawCart function, we don't need to also put drawTotal here.

}

Add alert to ask user if they're sure they want to buy
window.confirm will give pop up 'yes' or 'no' for action.
- to do this we want to wrap the logic inside of our confirm...

function checkout() {
  if(window.confirm('Are you sure you want to clear cart?')) {
    sandwiches.forEach(s => {
      s.quantity = 0
    })
  }
  drawCart()
}

To remove just one item that's already in cart. when I click button, find sandwich I want to remove, then decrease amount by 1 
- return to html and find sandwich name/price, add quantity p tag, then add mdi icon for x 'button' with an i class
  <i class="mdi mdi-close text-danger></i>
** note icons are inline text so we can style right in i tag. don't forget to update js with mdi too
- add onclick to html i class with removeItem('${s.name}')
- find returns just one item while filter returns array

function removeItem(sandwichName) {
  let foundSandwich = sandwiches.find(s => s.name == sandwichName)
  console.log(foundSandwich, 'found sandwich');
  foundSandwich.quantity--
  console.log('decrease', foundSandwich);
  drawCart()
}

debugger -> add above line you want to check and view each step in console to see if/where something is going wrong

HTML/CSS restyling
 add css class with .sandwich-img class containing height and any other properties you want to affect your element. 
  - make sure it's on correct tag so it doesn't affect more than you want it to. 