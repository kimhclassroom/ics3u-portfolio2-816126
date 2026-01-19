Day 1 Learning Log 

Variables 
I used variables to track positions, movement speed, input states, game state, and vision cone size.


int teacherX, teacherY;
int playerX, playerY;
int speed = 5;
boolean gameStarted = false;
boolean wDown, aDown, sDown, dDown;
int coneRange = 200;
int coneHalfWidth = 100; 
Where I used it and why I did it this way
teacherX teacherY store the teacher position so the teacher circle and vision cone always follow the same coordinates. playerX playerY store the player position so movement and drawing are consistent. speed is separate so I can change movement feel without rewriting movement logic.
gameStarted separates the start screen from gameplay so the game does not run map and movement before the player clicks start. wDown aDown sDown dDown store which keys are currently held so movement stays smooth while a key is held and allows diagonal movement.
coneRange and coneHalfWidth make the vision cone easy to tune for difficulty without changing triangle math.


Challenges and how I fixed them
Challenge: movement felt unreliable if I only used key presses.
Fix: I tracked key states with booleans in keyPressed and keyReleased, then moved the player every frame inside movePlayer using those booleans.
Challenge: player could move off screen.
Fix: I added constrain after movement so playerX and playerY stay inside the window.
Selection Structure If statements
 I used selection structures to decide what screen to display, to handle button hover and clicks, and to handle movement direction.
void draw() {
  if (!gameStarted) {
    drawStartScreen();
  } else {
    runGame();
  }
}
Where I used it and why I did it this way
In draw I used an if else to switch between start screen and gameplay. This solved the problem of the game running before the user starts.
In drawStartScreen I used a hover boolean based on mouseX and mouseY to change button color. This gives the player feedback that the button is clickable.
In movePlayer I used if statements to build dx and dy based on which keys are down. This is clean because multiple keys can be true at the same time.
Challenges and how I fixed them
Challenge: the start button sometimes felt like it did not respond.
Fix: I made sure the same bx by bw bh values are used in both drawStartScreen and mousePressed so the visual button and clickable area match exactly.
Challenge: diagonal movement could break if I used else if.
Fix: I used separate if statements so W and D can both apply in the same frame.
Loops
I used a for loop to draw a temporary grid for debugging and alignment.
void drawGridTEMP() {
  for (int x = 0; x < 1500; x += 100) {
   line(x, height, x, 0);
    line(width, x, 0, x);
  }
}
Where I used it and why I did it this way
I call drawGridTEMP inside runGame so the grid appears while testing map layout.
The loop repeats every 100 pixels and draws evenly spaced lines. This helped me place rooms and hallways accurately and check spacing quickly.


Challenges and how I fixed them
challenge: I could not tell if my rectangles were aligned or if gaps were the right size.
Fix: I added the grid loop so I could visually verify distances and placement.
Challenge: the grid made the game look messy.
Fix: I kept it as a temporary method so I can remove or disable it later without touching the core gameplay code.
Arrays and Data Structures 
Concept I am adding next
I will use arrays to store multiple possible key spawn locations so the key can spawn in random rooms but only in valid spots.
Where I will use it and why I am implementing it this way
I will call spawnKey when the game starts and when the game restarts.
The arrays store many valid coordinates without writing separate variables for each room.
Access is done using an index i, which makes the random selection simple and prevents impossible spawns like inside walls.


Custom Functions and Error Checking Restrictions
I wrote custom functions to organize the game and I included restrictions to prevent invalid movement and to control when gameplay runs.


void movePlayer() {
  int dx = 0;
  int dy = 0;
  if (aDown) dx -= speed;
  if (dDown) dx += speed;
  if (wDown) dy -= speed;
  if (sDown) dy += speed;
  playerX += dx;
  playerY += dy;
  playerX = constrain(playerX, 0, width - 25);
  playerY = constrain(playerY, 0, height - 25);
}
Where I used it and why I did it this way
I split tasks into functions so draw stays simple and I can test features one at a time.
movePlayer handles all player movement logic in one place.
drawStartScreen is separate so the UI does not mix with gameplay code.
teacherDownCone uses teacherX and teacherY so the cone stays attached to the teacher.


Error checking and restrictions in my code
screen restriction game does not run until gameStarted is true.
Movement restriction player cannot leave the screen.
Input handling restriction keys set a boolean on press and clear on release so movement stops immediately when released.
Challenges and how I fixed them
Challenge: Code got confusing when everything was inside draw.
Fix: I moved repeated tasks into custom functions like movePlayer and drawStartScreen so each method has one job.
Challenge: Movement kept going after I let go of a key when I tested other approaches.
Fix: I used keyPressed and keyReleased to keep the key states accurate.

Day 2 Learning Log

Variables

I used variables to add a new color variable to track the color of the player, and a new boolean for the color selection screen.

color playerColor;
boolean colorScreen = false;

Where I used it and why I did it that way
I used playerColor to store the current chosen color so I can use it on the player color, and the preview color. colorScreen tracks if the color selection screen should be displayed. 

Challenges and how I fixed them
Challenge: Right now the playerColor is black, because it hasn’t been initialized. 
Fix: initialize it so that a default color is chosen.

Selection structure

I used an if statement where if the mouse pressed the color screen button it’ll set the boolean colorScreen to true, which will display the color selection screen. 

void draw() {
  if (!gameStarted) {
    drawStartScreen();
  } else {
    runGame();
  }
  if (colorScreen==true){
    drawColorScreen();
  }
}

Where I used it and why I did it this way
This solved the problem of adding a second screen without deleting the start screen. The boolean lets me turn the color screen on when the user clicks the Change Color button.

Challenges and how I fixed them
Challenge:Right now there isn’t a way to set colorScreen back to false, so you can't go back.
Fix: Implement a back button that’ll set the boolean value back to false. 

Custom Functions and Error Checking Restrictions

I added a new custom function for the color selection screen.

void drawColorScreen(){
  background(220);
  drawGridTEMP();
  textAlign(CENTER);
  fill(0);
  textSize(40);
  text("COLOR SELECTED", width/2, 100);
  circle(width/2,height/2+50,200);
  circle(width/2-300,height/2+50,200);
}

Where I used it and why I did it this way

I separated the color screen into its own function so the UI code does not mix with gameplay code. It keeps drawStartScreen and runGame cleaner and makes it easier to expand color options later.

Challenges and how I fixed them
Challenge: I am still filling the player with a fixed color in drawPlayer.
Fix: I will change drawPlayer to fill(playerColor) so the chosen color actually applies to the player, not just the preview circle.

Day 3 Learning Log

Selection Structure

I added a restart game function, a color selection screen, and worked on room collision. 

Restart game function 

void keyPressed() {
  if (key == 'r' || key == 'R') {
    RestartGame();
    return;
  }

void RestartGame() {
  gameStarted = false;
  colorScreen = false;

  playerX = 100;
  playerY = 500;

  teacherX = 1200;
  teacherY = 500;

  playerColor = color(0, 120, 255);
}

Where I used it and why I did it that way

I used it in keyPressed, where if the player pressed R it would run the function. I did it like this so the player could leave the game and go back to the start screen whenever they wanted. I used if statements to track if the r or capital R key was pressed, if it was it would run the restart function and return to stop the rest of the code from running. 

Challenges I faced and how I solved them
Challenge: Overall it was straightforward, but the first time I tried making this code i didnt use boolean values for the game states, which made it hard to switch between and control which was running.
Fix: I fixed it by using boolean values to run if a certain value is true, or if it was set to true, this lets me easily control the game states, and what's running. 

Color selection screen

if (colorScreen) {
    if (mouseX > backX && mouseX < backX + backW && mouseY > backY && mouseY < backY + backH) {
      colorScreen = false;
      return;
    }
    int radius = 100;
    int gX = width/2 - 250;
    int gY = height/2 + 50;
    if (dist(mouseX, mouseY, gX, gY) < radius) playerColor = color(0, 255, 0);
    int bX = width/2;
    int bY = height/2 + 50;
    if (dist(mouseX, mouseY, bX, bY) < radius) playerColor = color(0, 120, 255);
    int pX = width/2 + 250;
    int pY = height/2 + 50;
    if (dist(mouseX, mouseY, pX, pY) < radius) playerColor = color(255, 105, 180);
Where I used it and why I did it that way

I used it to let the player control the color of the player to their preference. Also it updates what color the player should be to a variable which makes it easy to set multiple things like the preview on the start screen to that color. I used an if statement to track if the mouse is within a certain distance from a color circle, if it was and the mouse was pressed the playerColor value would update. 


Challenges and how I fixed them

Challenge: Getting the x and y values and typing it down was confusing and tedious.
Fix: I made them into variables, and labeled them by color and if it was the x or the y position this made the code easier and simpler to read. 

Room Collision 

I added room collision for the bottom left room to prevent the player from going though the walls

void bottomLeftRoom(){
  if (playerY>600 && playerY<800 && playerX==200){
    if (playerY + 25 < 650 || playerY > 700) {
      playerX= playerX + speed;
    }
  } else if (playerY>600 && playerY<800 && playerX+25==200){
    if (playerY + 25 < 650 || playerY > 700) {
      playerX= playerX - speed;
    }
  }

  if (playerX>=0 && playerX<200 && playerY+25==600){
    playerY= playerY - speed;
  } else if (playerX>=0 && playerX<200 && playerY==600){
    playerY= playerY + speed;
  }
}

Where I used it and why I did it that way

I used it in the drawMap function so it would run if the map was drawn. I used if statements to track the players X and Y, and if it was between certain values and certain locations it would reserve the players speed to prevent them from going though the wall.

Challenges and how I fixed them
Challenges: The player's x and y speed would change at the same time, making the player go up and left, the player could exit from one way, but would stop going the other.
Fix: I made separate if statements for the direction so the proper speed math would be used. I then further separated it so if the player came from any direction it would block it, accordingly. 

Day 4 learning log

  Repetition Structure
I added a timer, and made the teachers follow patrol points repeatedly. 
Teacher movement (repeating patrol)
void teacher() {
  if (millis() >= teacherWaitUntil) {
    if (teacherX < teacherTargetX) teacherX = min(teacherX + teacherSpeed, teacherTargetX);
    else if (teacherX > teacherTargetX) teacherX = max(teacherX - teacherSpeed, teacherTargetX);
    else if (teacherY < teacherTargetY) teacherY = min(teacherY + teacherSpeed, teacherTargetY);
    else if (teacherY > teacherTargetY) teacherY = max(teacherY - teacherSpeed, teacherTargetY);
    if (teacherX == teacherTargetX && teacherY == teacherTargetY) {
      teacherWaitUntil = millis() + 1000;
      if (teacherState == 0) { teacherTargetX = 300; teacherTargetY = 500; teacherState = 1; }
      else if (teacherState == 1) { teacherTargetX = 300; teacherTargetY = 200; teacherState = 2; }
      else if (teacherState == 2) { teacherTargetX = 300; teacherTargetY = 500; teacherState = 3; }
      else if (teacherState == 3) { teacherTargetX = 300; teacherTargetY = 700; teacherState = 4; }
      else if (teacherState == 4) { teacherTargetX = 300; teacherTargetY = 500; teacherState = 5; }
      else { teacherTargetX = 1200; teacherTargetY = 500; teacherState = 0; }

Where I used it and why I did it that way
I used this in runGame(). The repetition is happening because draw() runs over and over, and inside it, runGame() keeps calling the teacher functions. I did it this way so the teacher constantly updates their position a little bit each frame, which makes smooth movement. The teacherState keeps cycling, so the teacher repeats the same patrol pattern instead of stopping after one path. The teacherWaitUntil also makes the teacher pause for a second at each point, but still repeat the whole route. The way it works is that it makes the teacher move in a T shape along points I set. There's 5 states of the teacher and each state represents a point it's moving towards. It also pauses for one second at each point using millis. I did this another time for a teacher that moves only up and down. The code is similar, but it has fewer states since the path is simpler. It moves just in a straight line up and down going from one Y point to the other. 

Challenges I faced and how I solved them
Challenge: At first the teacher wouldn’t repeat the path properly and it would look like it was drifting or teleporting after a restart.
 Fix: I made sure the path always goes back to the exact same checkpoints using the same state order, and I reset all the teacher values in resetTeachers() so every run starts at the same position and state. 

Day 5 Learning Log

Arrays

I added random key spawn positions. 

void spawnKey() {
  int i = int(random(keySpawnX.length)); //generates a random integer to use for the keySpawn index
  keyX = keySpawnX[i]; //that integer genereated coresponds to a possible key spawn
  keyY = keySpawnY[i];
  hasKey = false; 
}

I used them to store the X, and Y values for the possible locations for the keys, one in each room, or two if the room is big enough. I will generate a random integer, which will then correspond to an array of the X, and Y, to choose a random spawn. This made it faster to manage, because I can change the spawn from one place, easier to add spawns, just be adding new values to the array, and supports randomness. 

Challenges I faced and How I fixed them
Arrays are simple and pretty straightforward, I did have to keep in mind that the X, and Y had to correspond correctly for the index value, that index values start at 0, and the random number must be an integer. 

Error Checking and Restrictions

I added wall collision and used constrain to keep the player within bounds of the intended playable area, and to overall keep the game running as intended.  

void bottomLeftRoom() {
  fill(150);
  rect(0, 600, 200, 200);
  fill(0, 255, 0);
  rect(190, 650, 20, 50);
  fill(150);

  if (playerY > 600 && playerY < 800 && playerX == 200) {
    if (playerY < 650 || playerY > 700) playerX = playerX + speed;
  } else if (playerY > 600 && playerY < 800 && playerX + 25 == 200) {
    if (playerY < 650 || playerY > 700) playerX = playerX - speed;
  }

  if (playerX >= 0 && playerX < 200 && playerY + 25 == 600) playerY = playerY - speed;
  else if (playerX >= 0 && playerX < 200 && playerY == 600) playerY = playerY + speed;
}

I used it to keep the player on the map, and possibly make it unplayable. I used a screen boundary restriction using constrain so the player cannot leave the screen, room wall restriction so the player cannot walk through walls except at doorway gaps, start screen restriction so the player cannot move until the game starts, color screen restriction so the game doesn’t run while choosing a color, and a restart restriction so pressing R resets all important variables to avoid broken states.

Challenges and how I fixed them

Challenge: At first the player couldn’t pass through walls in certain directions, they could exit the map, and go up towards the HUD and off the map (Timer), after restarting the game, keys, positions, or screens could be stuck in the wrong state, and the player could move during menus.
Fix: I made code for both directions of travel, used constrain and included the top HUD, used a restart method to reset all important variables (player position, teacher position, screen booleans, and movement booleans) so each run starts clean,  and added restrictions with booleans so movement only happens when gameStarted == true and the colour screen is not active
