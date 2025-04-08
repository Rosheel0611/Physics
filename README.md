# Physics

let colorlist = ['#e6194b', '#3cb44b', '#ffe119', '#4363d8', '#f58231', '#911eb4', '#46f0f0', '#f032e6', '#bcf60c', '#fabebe', '#008080', '#e6beff', '#9a6324', '#fffac8', '#800000', '#aaffc3', '#808000', '#ffd8b1', '#000075', '#808080', '#ffffff', '#000000']


let bouncers = [];
let G = 1;
let wind = 0;


function setup() {
  createCanvas(400, 400);
  for (let i = 0; i < 10; i++){
    let x = random(width);
    let y = random(height / 2);   
    let r = random(10, 30);
    let c = random(colorlist);
    let dx = random(-2, 2);
    let dy = random(-2, 2);
    bouncers.push(new Bouncer(x, y, r, c, dx, dy));
  }
}

function draw() {
  background(220);

  if (wind !== 0) {
    fill(0);
    text("💨 Wind Blowing →", 10, 20);
  }
  
  if (wind !== 0) {
     fill(0);
    text("Wind Blowing", 10, 20);
  }

  for (let bouncer of bouncers){
    bouncer.update();
    bouncer.display();
  }
}

class Bouncer {
  constructor(x, y, r, c, dx, dy){
    this.x = x;
    this.y = y;
    this.r = r;
    this.c = c;
    this.dx = dx;
    this.dy = dy;
  }

  update(){
    this.dy += G;           // gravity
    this.applyWind();       // apply wind
    this.x += this.dx;
    this.y += this.dy;
    this.checkEdges();      // edge bouncing
  }

  applyWind(){
    this.dx += wind;        // wind affects horizontal motion
  }

  checkEdges(){
  // Right or left wall
  if (this.x + this.r > width || this.x - this.r < 0){
    this.dx *= -1;
    this.x = constrain(this.x, this.r, width - this.r);
  }
  // Bottom or top wall
  if (this.y + this.r > height || this.y - this.r < 0){
    this.dy *= -1;
    this.y = constrain(this.y, this.r, height - this.r);
  }
}


  display(){
    fill(this.c);
    ellipse(this.x, this.y, this.r * 2);
  }
}

function keyPressed() {
  if (key === 'a') {
    wind = 0.5; // 💨 Start wind when 'a' is pressed
  }
}

function keyReleased() {
  if (key === 'a') {
    wind = 0; // 🛑 Stop wind when 'a' is released
  }
}

function keyPressed() {
  if (key === 'a') {
    wind = 0.5; // 💨 Start wind
  }

  if (key === 'w') {
    for (let bouncer of bouncers) {
      bouncer.dy -= 10; // ⬆️ Boost upward velocity
    }
  }
}

