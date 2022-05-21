# Taming the Rubik's Cube / How I met Group Theory

## How the Rubik's Cube made the world a better place

Recently I've read the book [Cubed, written by Erno Rubik](https://semmi88.github.io/book_reviews/cubed.html) (the inventor of the cube), and this book rekindled my love for the Cube. The book details the process of invention and searches for reasons to explain its popularity. The argument put forth, with which I agree, is that it is not a coincidence that the Rubik's Cube became widely known around the world. There something universal, almost magical about it, and it is full of contrast and contradicitons: simplicity vs complexity, pleasure vs frustration.

The game itself is easy to understand, intuitive - there is no need for an instruction manual. A child and an adult can both understand the mechanics and the goal wihtin 3 seconds of holding the cube and playing with it. Nevertheles it is a challenging puzzle, even just after a couple of rotations our minds cannot follow the patterns and we can get lost, unable to find our way back to the unscrabled state. It is frustrating, because the difficulty of the puzzle is not evident from its simple, aesthetically pleasing appearance. The cube evokes emotions, as it is not what we expect. And we, as humans, are naturally curious and drawn to these types of challenges, we want to be able to understand its misteries and be able to tame the puzzle.

No wonder, that the Rubik's Cube became a cultural symbol, a symbol of intelligence and problem solving. It brought on a revolution in edutainment (education + entertainment) by offering fun ways to introduce mathematical concepts to kids. And in the process it made studying, thinking about problems cool again. I could even go as far as to say, that the Rubik's Cube gave us a universal value system on which we can all agree. As somehow, deep down, we can all agree that solving a scrabled cube is a good thing, being able to understand and solve complex puzzles is a useful skill in life, and in it is worth investing time to learn these skills.

## Solving methods and Cubing Basics

So how can you solve the cube? There are several well documented strategies, like the [layer-by-layer approach](https://ruwix.com/the-rubiks-cube/how-to-solve-the-rubiks-cube-beginners-method/), where you focus on completing each layer at a time by orienting and permuting the cubies. In total you need to learn about 5 algorithms, and with practice you will be able to solve the cube in around 2 minutes. That is pretty cool!  

Of course there are better, more advanced solving methods, but these require you to memorize more algorithms. Speedcubers, people who compete in cube solving world championships, learn about 200 different algorithms, which they can apply to different states of the cube, and so are able to solve a scrabled cube in under 10 seconds. Now that is impressive!

But where do these solving methods come from? Why do they work and how could you come up with a solving method? These are the questions that I will try to explore in this article. 

![alt text](bender_cube.jpg "Bender meme, come up with my own algorithms")

A first intuition about creating your own solving methods consist in the observation that every rotation can be undone. Meaning that if we know exactly the rotations that scrambled the cube, then the puzzle becomes trivial. We can just do those moves in reverse and we get back to the original state. This is a key observation, although on it's own it is not very helpful (as we usually don't know the rotations that scrabled the cube in the first place).

The difficult thing about solving the cube is that rotating any face moves a lot of pieces around, and it is hard to follow and keep up with all the changes. Ideally it would be nice to come up with algorithms that only affect a small part of the cube, only a small subset, like rotating two corners and leaving the rest of the cube unchanged. The good news is, that using intuition and bit of mathematics, we can come up with such algorithms. To be able to describe and discuss these algorithms, let's go over some cubing basics.

The Rubik's cube is made up of 26 tiny cubes (or cubelets), which can be grouped in three categories:
 - 6 center pieces, which have one colored face
 - 12 edge pieces which have two different colored faces (or facelets)
 - 8 corner pieces, which have three different colored faces (or facelets)

Solving methods usually are described using the [Standard Cube Notation](https://ruwix.com/the-rubiks-cube/notation/), which assigns a single capital letter to face rotations. 
 - 90° clockwise rotation of the 6 faces: F=Front, B=Back, R=Right, L=Left, U=Up, D=Down
 - 90° rotation of middle layers: M=middle (between R and L), E=Equator (between U and D), S=Standing (between F and B)
 - Counterclowckwise rotations of 90° are described by adding an aposthrope: F' is the opposite (or inverse operation) of F
 - Multiple rotations are described by adding a number after the letter: FFF = F3
   - Turning a face 4 times leaves the cube unchanged (identity operation), so: F4=1
   - Three turns in one direction equal a turn in the opposite direction: F3=F' (meaning that 3 x 90 clockwise equals to a 1 x 90 counterclockwise turn)



