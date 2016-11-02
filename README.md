# Processing-Pong-Game
 int x, y, w, h, speedX, speedY;
int paddleXL, paddleYL, paddleW, paddleH, paddleS;
int paddleXR, paddleYR;
boolean upL, downL;
boolean upR, downR;

color colorL = color(39,52,124);
color colorR = color(214,53,59);

int scoreL = 0;
int scoreR = 0;

int winScore = 11;

void setup() {
  size(800,800);
  stroke (255);
  
  x = width/2;
  y = height/2;
  w = 50;
  h = 50;
  speedX = 3;
  speedY = 3;
  
  textSize(40);
  textAlign(CENTER, CENTER);
  rectMode(CENTER);
  paddleXL = 40;
  paddleYL = height/2;
  
  paddleXR = width-40;
  paddleYR = height/2;
  
  paddleW = 30;
  paddleH = 100;
  paddleS = 5;
}

void draw() {
   background(255);
   smooth();

   drawCircle(); //For the drawing of the circle
   moveCircle(); // For the moving of the circle
   bounceOff(); // For the bouncing of the circle
   
   drawPaddles();
   movePaddle();
   restrictPaddle();
   contactPaddle();
   
   scores();
   
   GameOver();
   
}

void drawPaddles() {
  fill(0);
  rect(paddleXL, paddleYL, paddleW, paddleH);
  fill(0);
  rect(paddleXR, paddleYR, paddleW, paddleH);
}

void movePaddle() {
  if(upL) {
     paddleYL = paddleYL - paddleS;
  }
  if(downL) {
    paddleYL = paddleYL + paddleS;
  }
    if(upR) {
     paddleYR = paddleYR - paddleS;
  }
  if(downR) {
    paddleYR = paddleYR + paddleS;
  }
}


void restrictPaddle() {
  if(paddleYL - paddleH/2 < 0) {
    paddleYL = paddleYL + paddleS;
  }
  if(paddleYL + paddleH/2 > height) {
    paddleYL = paddleYL - paddleS;
  }
  if(paddleYR - paddleH/2 < 0) {
    paddleYR = paddleYR + paddleS;
  }
  if(paddleYR + paddleH/2 > height) {
    paddleYR = paddleYR - paddleS;
  }
}

void contactPaddle() {
  // left collision
  if(x - w/2 < paddleXL + paddleW/2 && y - h/2 < paddleYL + paddleH/2 && y - h/2 > paddleYL - paddleH/2 ) {
    if(speedX < 0) {
      speedX = -speedX;
    }
  }
  // Right Collision
  else if(x + w/2 > paddleXR - paddleW/2 && y - h/2 < paddleYR + paddleH/2 && y - h/2 > paddleYR - paddleH/2 ) {
    if (speedX > 0) {
      speedX = -speedX;
    }
  }
}


void drawCircle() {
  fill(colorL);
  ellipse(x, y, w, h);
}

void moveCircle() {
  x = x + speedX;
  y = y + speedY;
}

void bounceOff() {
  if( x > width - w/2) {
    setup();
    speedX = -speedX;
    scoreL = scoreL + 1;
  } else if ( x < 0 + w/2) {
    setup();
    scoreR = scoreR + 1;
  }
  
  if( y > height- h/2) {
    speedY =-speedY;
  }
  else if ( y < 0 + h/2) {
    speedY =-speedY;
  }
}

void scores() {
  fill(0);
  text(scoreL, 100, 50);
  text(scoreR, width-100, 50);
}

void GameOver() {
  if(scoreL == winScore) { 
    gameOverPage("Player 1 Wins!", colorL);
  }
  
  if(scoreR == winScore) {
    gameOverPage("Player 2 Wins!", colorR);
    
  }
}

void gameOverPage(String text, color c) {
  
  speedX = 0;
  speedY = 0;
  
  fill(201, 18, 192);
  text("Game over!", width/2, height/3 - 40);
  fill(c);
  text(text, width/2, height/3);
  fill(201, 18, 73);
  text("Click to play again!", width/2, height/3 + 40);
  
  if(mousePressed) {
    scoreR = 0;
    scoreL = 0;
    speedX = 3;
    speedY = 2;
  } 
}


void keyPressed() {
  if(key == 'w' || key == 'W') {
    upL = true;
  }
  if(key == 's' || key == 'S') {
    downL = true;
  }
   if(keyCode == UP) {
    upR = true;
  }
  if(keyCode == DOWN) {
    downR = true;
  }
}

void keyReleased() {
  if(key == 'w'|| key == 'W') {
    upL = false;
  }
  if(key == 's'|| key == 'S') {
    downL = false;
  }
    if(keyCode == UP) {
    upR = false;
  }
  if(keyCode == DOWN) {
    downR = false;
  }
}
