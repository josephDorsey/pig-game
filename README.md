# Project #3 - Pig Game

## Flowchart - www.diagrams.net

- When we build games its useful to build a flow chart. It is a representation of everything that can happen in the application.
  - the yellow (the IF): shows the possible actions that the user can take and from there we see what happens in the application.
  - green (the code block)
  - blue, is the check?
  - red? if they win

## Target the score values <section>

First we want to change our player scores back to 0 since currently their value is `Player 1: 43`, `Player 2: 24`.

We see that in both of the sections have `<p class="score">` elements that have our inputted numbers 43 and 24 have the class `score` and they each have their own `id`.

```
    <p class="score" id="score--0">43</p>
    <p class="score" id="score--1">24</p>
```

## targeting id's with document getElementById

Before we would target our elements using the querySelector:

```
let score0El = document.querySelector('#score--0');
```

This will select the element with this id using a `#` symbol instead of the `.`

We could redo this as:

```
let score1El = document.getElementById('score--1');
```

This time we do not use the `#` because this is not a selector, we are just looking for the name of the id that we're looking for.

- We gave the variable the name El at the end to stand for element as to not get it confused with other scores

### querySelector and getElementById

Functionally these two pieces of code are same. They both do the same thing. getElementById is supposed to be a little faster than querySelector.

## How to setup the javascript section

We want to use comments to mark our page with what we are selecting

### // Selecting Elements

So lets create a section for our already created code:

```
// Selecting elements
const score0El = document.querySelector('#score--0');
const score1El = document.getElementById('score--1');
```

Next lets give some text content to these variables and assign it 0.

```
score0El.textContent = 0;
score1El.textContent = 0;
```

Ok so when we reload the page our values have been updated to 0.

## Hide the dice

Now lets make the dice disappear. First lets check and see if there's a hidden class, but it looks like there isnt so lets make one in CSS

```
.hidden {
  display: none;
}
```

Now like in our previous project we did this method to add and remove classes. We used `.classList.add()` or `.classList.remove()`

So we created the variable:

```
const diceEl = document.querySelector('.dice');
```

now we add the class of hidden

```
diceEl.classList.add('hidden');
```

Now our dice is hidden.

## Roll and reveal the dice

When we refer to the flowchart it is actually similar to dividing the big problem down into small sub problems! So essentially all the squares that we have here are the sub problems that we need to go ahead and implement.

### Sub-problem 1 - Rolling Dice Functionality

We want to react to clicking to that roll button so we need to select that button and add an eventListener. Lets select all three of our buttons.

```
const btnNew = document.querySelector('.btn--new');
const btnRoll = document.querySelector('.btn--roll');
const btnHold = document.querySelector('.btn--hold');
```

And lets create a new section called Rolling Dice Functionality. Since we will not be using the function we will just store it inside the event listener. Write some code within this function that pertains to the flowChart for example.

```
// Rolling dice functionality

btnRoll.addEventListener('click', function() {
  // 1. Generating a random dice roll
    const dice = Math.floor(Math.random() * 6) + 1
  // 2. Display dice
    diceEl.classList.remove('hidden');
  // 3. Check for rolled 1: if true, switch to next player
})
```

#### Generate a random dice roll

1. So here we want to make a `const dice` variable. This is why we have `diceEL` for the element. Here we can just write `dice` because we do not want this to be a global variable because every time we roll the dice we want a new number.

#### Display Dice + manipulating src

2. This is the part where we want to display dice, remember the class is already hidden. So lets remove that

Here comes the interesting part. So we know that the dice will be a number between one and six. And now, according to these numbers we want to display one of these images that we have here. So you see that we have dice-1, dice-2 etc.

We can actually target the src in an element like this:

```
`diceEl.src = `dice-${dice}.png`
```

we target the images `src` and assign it the corresponding image we want it to change to. In our case we have six different images that we want to be chosen at random when rolled. To do so we make a template literal string as the assigned value. We set it as `dice-${dice}.png`. This will randomly choose the image number based on our dice-1 to dice-6 images.

So now when we load the page. When we hit the button we get the different images. Lets log the number to see if the image displayed is the correct number. ITS CORRECT!

#### Check for rolled 1: if true, switch to next player

```
if (dice !== 1) {
  // add dice to current score
} else {
  //switch to next player
}
```

Where would we log the current score? It needs to be outside the function

```
let currentScore = 0;
```

So after declaring this globally. lets call it in the conditional. for now lets just display the score on player one. current--0 is the id in our HTML

First lets create new variables for our current--0, current--1 id. and then set the .textContent of current0El to currentScore.

```
if (dice !== 1) {
// add dice to current score
currentScore += dice;
current0El.textContent = currentScore;
} else {
//switch to next player
}
```

#### Switching to active player

So we need to create a need a variable for activePlayer.

```
const activePlayer = 0;
```

Here in our example [0] = player 1, [1] = player 2. We will store both scores in an array.

```
const scores = [0,0];
```

So let's select the element dynamically. When we called it by document.getElementById(). And we need the class name its gonna be either `current--0 for player 0` or `current--1 for player 1`. So now you see why we use the active player as 0 or 1, because now we can use that variable to basically build these class names.
When it's player 0 we get current--0, for 1 we get current-1;

Once again like with the dice example:

```
diceEl.src = `dice-${dice}.png`;
```

We will do the same to the `current--` id class by attaching it as a template literal so it will choose either 1 or 0;

```
document.getElementById(`current--${activePlayer}`).textContent = currentScore;
```

So essentially switching the next player means to change that value:

```
activePlayer  = activePlayer === 0 ? 1 : 0;
```

##### activePlayer === 0 ? 1 : 0

```
activePlayer  = activePlayer === 0 ? 1 : 0;
```

We used a ternary operator for this. So essentially we are reassigning the active player here and we're checking whether right now it is player 0. So if it is then here the result of this whole operator, `activePlayer === 0 ? 1 : 0` then the result will be 1.
So activePlayer will be equal to 1, but if this is false here, so if the player is actually 1, well then the active player will become 0.

##### reset currentScore

Ok, so now after writing this code and testing it out in the launcher. It switches to the next player when we roll a 1. However, it still updates the player 0 score. So we need to reset the current score, okay?

Also nothing visually changed yet, in the real game when the player is switched the background the non active player gets a white background. So currentScore bak to 0 and then we need to set the text Content back to 0, okay?
So basically of the active player before we switch it. So for example, player 0 was playing and then we switched to player 1, but before we switch to that switch, we need to change the currentScore of player 0 back to 0. Remove the code:

```
current0El.textContent = currentScore; // CHANGE LATER
```

```
// Rolling dice functionality

btnRoll.addEventListener('click', function () {
  // 1. Generating a random dice roll
  const dice = Math.trunc(Math.random() * 6) + 1;
  console.log(dice);
  // 2. Display dice
  diceEl.classList.remove('hidden');
  diceEl.src = `dice-${dice}.png`;
  // 3. Check for rolled 1: if true, switch to next player
  if (dice !== 1) {
    //add dice to current score
    currentScore += dice;
    document.getElementById(
      `current--${activePlayer}`
    ).textContent = currentScore;

  } else {
    // switch player
    document.getElementById(
      `current--${activePlayer}`
    ).textContent = 0;

    currentScore = 0;
    activePlayer = activePlayer === 0 ? 1 : 0;

  }
});
```

So here we took the code below. And changed its textContent to 0 instead of currentScore.

```
document.getElementById(
      `current--${activePlayer}`
    ).textContent = 0;
```

So now this works. The points reset.

#### Changing the active players background color

So when viewing our <section></section> in HTML we see that the class have the player--active class.

```
<section class="player player--0 player--active">
```

So when we switch to the other player we want to remove that class from the code above and put it in here:

```
<section class="player player--1">
```

So now we will start by selecting player--0 class and player--1 class.

##### classList.toggle()

Here is another property that is available on the class list property. It will add the class if it is not there and if it is there it will remove it.

EXAMPLE

```
    player0El.classList.toggle('player-active');
    player1El.classList.toggle('player-active');
```

Here we add both player elements because remember, if it already has the class then when toggled it will disappear. This ensures that it's only ever on one of the elements at once. So now lets check that it works.
It does!
