#uberdoku

## Sudoku for Uber
Sudoku app built with the following technologies:
* Javascript, CSS, HTML
* Jquery ( event listeners and DOM modification )
* Sass ( CSS precompiler )
* Grunt ( build tool for minifying, concatenating, and live reload )
* Browserify ( node style 'require' statements for module loading )
* Mocha/Chai ( testing )

## Play the game
A deployed version can be found [here](http://addisonedwardlee.github.io/uberdoku-deploy)

## Run it locally
1. Fork the repo and clone it
1. Run `grunt serve` to start a server
1. Navigate your browser to `localhost:9000` if it doesn't go there automatically
1. Play!

## Design decisions

### Board structure
There are a few different ways to build the sudoku data.  You could simply
create an array of 81 elements, but both ensuring that there are no conflicts and
rendering the solution to the page would require a lot of complicated logic using modulos
to ensure you're comparing the appropriate elements.

Instead, I chose to create an array of arrays, with each array representing one row.

```js
[[2,4,8,3,9,5,7,1,6],    // row 1
 [5,7,1,6,2,8,3,4,9],    // row 2
 [9,3,6,7,4,1,5,8,2],    // row 3
 [6,8,2,5,3,9,1,7,4],    // row 4
 [3,5,9,1,7,4,6,2,8],    // row 5
 [7,1,4,8,6,2,9,5,3],    // row 6
 [8,6,3,4,1,7,2,9,5],    // row 7
 [1,9,5,2,8,6,4,3,7],    // row 8
 [4,2,7,9,5,3,8,6,1]];   // row 9
```

This affords us a few benefits:
* First, we can easily confirm there are no row conflicts by checking that the contents
of each row add up to 45.
* Secondly, checking column conflicts is simple as checking
the ith element of each array.
* Third, and most important, is rendering the board is really simple and easy to read.
```js
// for each row in game
game.forEach(function( row, rowNum ) {
  // create a row DOM element
  main.append('<div class="row"></div>');

  // for each data element in the row
  row.forEach(function( data, colNum ) {
    // render it
    $('.row').append('<input data="data" />');
  });
});
```

### Browserify

One great feature of Node.js is the ability to require specific modules when you need them.
For example, if you need to handle API calls, you can include the http module and
access all of its helper functions using the code below

```js
var http = require('http');

http.get('/route', functionToHandleGetRequest);

```

Browserify let's do you do the exact same thing on the client.  We're able to create a file
which contains useful helper functions and reuse the helpers anywhere else in our system.
Great for creating modularity!

```js
var helpers = require('./helpers.js');

if( helpers.checkIfValid( self.solution, row, col, val) ) {
  helpers.setCellCorrect( $target );
} else {
  helpers.setCellIncorrect( $target );
}

```

## Todos
- [x] Add tests
- [x] Implement Browserify
- [ ] Create a random Sudoku generator
- [ ] Create a winning animation
