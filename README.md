Link to game - https://wardy-t.github.io/Wordle/
Link to planning work - https://github.com/wardy-t/Wordle-plan.git

![image](https://github.com/user-attachments/assets/42f32763-dc48-4140-80d5-db077a8b7395)


Welcome to my Wordle!

Wordle is a game where you have five attempts to guess a hidden five letter word. Each time you guess a word the game will tell you if you have guessed any letters that are in the word (which are highlighted orange),
or if you have guessed any letters which are in the word AND in the correct placement within the word (these are highlighted green). If you have guessed the hidden word correctly within five attempts you win!

It plays a lot easier than it reads :)

I built this game over five days between 2/9/24 and 6/9/24 in between full time work for my General Assembly Software Engineering Bootcamp Project One, after providing a wireframe, UX/UI brief and some Pseudocode
outlining my plan.

I chose to build Wordle because I felt it put a lot of skills I had recently acquired to the test - specifically using multidimensional arrays, different loops, event listeners and query selectors in order to
provide functions and solutions that would solve some of the more complex logic problems behind the game, such as recognising and coloring individual letters within user guesses after comparing them to the hidden
word.

The game also afforded me the chance to move away from Flexbox and use CSS Grid for the first time as well as building a HTML keyboard within the browser for my gamers to use. Both of these were fun and provided
my Wordle with a fairly clean and structured look.

The technologies used were JavaScript, HTML and CSS.

I have a number of future enhancements that I plan to add to this game over time such as some limited use buttons that assist the user by revealing letters or removing keys on the keyboard, so watch this space.

# Build/Code Process

The very first thing I did for my project was build a data.js file, create an array named words and fill it with five letter words. I had a feeling I was going to need them!

After setting my HTML template I set about building out my wireframe starting with my H1 and H2, reset button, followed by my wordle board.

I decided to build one multidimensional array rather than five separate rows of arrays for my wordle game. I thought both options had merit, but I felt that I could use loops to iterate through every square in my array one at a time which turned out to be a solid choice. One of the benefits of picking a wordle game for my project brief is that the user is restricted and can only move one space forward or backwards when using my array, compared to a game like Battleships where they have a lot more autonomy to move and click all over the array.

When building my wordle board in HTML I gave each square a collective class and an individual id which matched their index number although I did not end up using the latter.

Once I had done this I moved to CSS and made a basic design for my 5x5 grid, header and subheader before loading them onto my browser. I now had a rudimentary frame to build my wordle logic within.

Hopping onto app.js I started by writing out some of the let variables I expected to be using (although these changed a lot as I got into the various function requirements) and created a querySelectorAll for my squares.

I started building my functions for the various logic steps my game would have to take in order for the user to have a smooth gaming experience, starting with the simple stuff - I built a checkWord function with “win”, “lose”, and “try again” messages and prepared it to advance the user’s turn by building a counter that would log their attempts when the checkWord function was comparing the user’s word with the hidden word.

I also built an init function to start and reset the game and wordle board.

The first major challenge came in the form of providing the game with a random hidden five letter word for the user to try and guess. I originally built in a dictionary API but as we have not studied this at GA yet I decided to stick to what I know and instead wrote a defer src script for my data.js file into my HTML so that I could pull from my separate array of five letter words.

I then made a random index generator which would work its way through my words array in my data.js file, return a random word and convert it to upper case (see below).

```jsx
const fetchHiddenWord = () => {
    const randomIndex = Math.floor(Math.random() * words.length);
    return words[randomIndex].toUpperCase();
};
```

I also established the following within my init function

```jsx

    hiddenWord = fetchHiddenWord();
```

This provided my wordle game with a fresh hidden word from the list every time the game initialises.

I then set about providing the user with a means to guess their own word using the grid I had provided. I had toyed with the idea of linking the wordle board array to the user’s physical keyboard but instead decided to design an in-browser HTML keyboard which I hope to add some extra functionality to in the future such as greying out guessed letters which are useless.

I built the keyboard by assigning every key as a button within a section id’d “keyboard”, a group class, “key”, and an individual value which was the key’s letter. I also separated the keys into four rows designed to mimic the shape of a classic qwerty keyboard.

```html
    <section id="keyboard">
        <div class = "row1">
            <button class = "key" value="Q">Q</button>
            <button class = "key" value="W">W</button>
            <button class = "key" value="E">E</button>
            <button class = "key" value="R">R</button>
            <button class = "key" value="T">T</button>
            <button class = "key" value="Y">Y</button>
            <button class = "key" value="U">U</button>
            <button class = "key" value="I">I</button>
            <button class = "key" value="O">O</button>
            <button class = "key" value="P">P</button>
        </div>
        
        <div class = "row2">
            <button class = "key" value="A">A</button>
            <button class = "key" value="S">S</button>
            <button class = "key" value="D">D</button>
            <button class = "key" value="F">F</button>
            <button class = "key" value="G">G</button>
            <button class = "key" value="H">H</button>
            <button class = "key" value="J">J</button>
            <button class = "key" value="K">K</button>
            <button class = "key" value="L">L</button>
        </div>
        
        <div class = "row3">
            <button class = "key" value="Z">Z</button>
            <button class = "key" value="X">X</button>
            <button class = "key" value="C">C</button>
            <button class = "key" value="V">V</button>
            <button class = "key" value="B">B</button>
            <button class = "key" value="N">N</button>
            <button class = "key" value="M">M</button>
        </div>
        
        <div class = "row4">
            <button class = "delete" value="Backspace">←</button>
            <button class = "enter" value="Enter">Enter</button>
        </div>
```

As you can see I gave separate classes to the backspace and enter buttons, although I did not do this until later when I attached event listeners to them.

It was around this time that I decided that I was going to use Grid as the display for my CSS rather than Flexbox as I felt it would help me shape my keyboard better and provide a bit more symmetry later on when aligning my keyboard, wordle board, etc.

The next challenge was to allow the user to populate my wordle board array with letters using a handleClick function. For this function to work I had to make an empty array called userWordArray which would hold the user’s guessed letters as they use the keys to form a word. 

The handleClick function first establishes the value of the key being clicked, then checks to make sure the letter being clicked will not create a 6 letter word and that the letter will not land outside the boundaries of the wordle board array. It then finds the correct square on the board, populates it with the value of the key pressed, adds that value to the userWordArray and finishes by moving on to the next square.

```jsx
const handleClick = (event) => {
    const keyValue = event.target.value;
    
    if (userWordArray.length < wordLength) {
        if (squareIndex < squareEls.length) {
            const square = squareEls[squareIndex];
            square.textContent = keyValue;
            userWordArray.push(keyValue);
            squareIndex++;
            render();
        }
    }
};
```

Around now I started having some trouble with my enter, backspace and reset buttons so I took the time to add separate query selectors and event listeners to them and getting them working - the backspace in particular took quite a few tries! While I figured out the code pretty quickly, using the .pop method to remove the key value from the userWordArray and moving the squareIndex backwards, working out where to place the code block took a little while.

```jsx
deleteEl.addEventListener('click', () => {
    if (userWordArray.length > 0) {
        userWordArray.pop();
        squareIndex--;
        const square = squareEls[squareIndex];
        square.textContent = '';
        render();
    }
});
```

At this point I could initialise a wordle board and HTML keyboard with working keys, select a hidden five letter word, allow a user to input letters into my grid to attempt to guess the word, allow them to iterate through five rows on the board as they made five attempts, and finally supply the user with messages advising that they had won, lost or to keep trying as they played. Pretty good going!

This brought me to what became my favourite piece of code within the whole game by far - the colorAssist function. If you have played wordle then you know that without this function winning is nearly impossible for the user. Effectively the wordle board has to feed back to the user how close they are to completing the hidden word by coloring letters that are in the right place with a green background and letters that exist elswhere within the word with an orange background. This allows the user to make more educated guesses as their turns become less.

After some tinkering I was able to achieve this with some clever use of the .split method (thank you, MDN!). As you can see, the colorAssist function takes the hidden word and splits each letter into its own array so that it can be compared letter by letter with the user word using a forEach loop. The function then establishes which squares are being used, compares the letters and provides the corresponding color code depending on the success of the user. It took me a little while to realise that .includes was the way to go for the ‘orange’ squares but I was pleased when it worked because it made for a very tidy solution.

```jsx
const colorAssist = (userWord, hiddenWord) => {
    const hiddenWordArray = hiddenWord.split('');
    
    userWord.split('').forEach((letter, index) => {
        const squareIndex = row * wordLength + index;
        const square = squareEls[squareIndex];

        if (letter === hiddenWordArray[index]) {
            square.style.backgroundColor = 'lime';
        } else if (hiddenWordArray.includes(letter)) {
            square.style.backgroundColor = 'orange';
        } else {
            square.style.backgroundColor = 'gray';
        }
    }
)};
```

Once this code block was working and with a bit of housekeeping my game was ready to go. I finished by tidying up my CSS and giving my wordle some UI/UX features that I felt mirrored games I have played from app stores - I gave it a bubbly and cute Fredoka font, pastel colors, softened the corners on the grid and buttons and gave them some box shadow for a slightly 3D pop. I also centered and spaced everything evenly.

Enjoy the game!
