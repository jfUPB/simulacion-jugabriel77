#### N Cuerpos

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/OT7-EldwA)



``` js
var planets = [];
var numPlanets = 1;

var gravitationalConstant = 1; // Fix

function setup() {
  noStroke()
  
  createCanvas(windowWidth, windowHeight);
  background(51);
  frameRate(60);
  
  
  planets.push(new Planet(width/2, height/2, 30))
  for(let i = 0; i < numPlanets; i++){
    planets.push(new Planet(random(width), random(height), random(20)));
  }
 
}

function draw() {
  // background(51);
  for(let i = 0; i < planets.length; i++){
    planets[i].paint();
  }
  
  // skipping first
  for(let i = 1; i < planets.length; i++){
      
    
      let planetPos = planets[i].p;
      let planetMass = planets[i].m;
      
      let grav = getGravityForce(planets, createVector(planetPos.x, planetPos.y))
        //line(planetPos.x, planetPos.y, abs(grav.x-planetPos.x)*1, abs(grav.y-planetPos.y)*1)
        planets[i].addPos(grav)

    
  }
  
}


function getGravityForce(masses, point){
  
  resultingVector = createVector(0,0)
  
  for(let i = 0; i < masses.length; i++){
    
    let mPos = masses[i].p;
    //console.log(mPos);
    let mMass = masses[i].m;
    let distance = dist(mPos.x, mPos.y, point.x, point.y);
    
    let cosI = sin( abs( mPos.x - point.x ) / distance );
    let sinJ = sin( abs( mPos.y - point.y ) / distance );
    
    if(point.x < mPos.x)
      cosI = -cosI
    if(point.y < mPos.y)
      sinJ = -sinJ
    
    resultingVector.add(createVector(
      gravitationalConstant * (mMass / (distance )) * cosI, 
      gravitationalConstant * (mMass / (distance )) * sinJ)
                       );
    
  }
  
  return resultingVector;
  
}


// Add planet on click
function mouseClicked(){
  planets.push(new Planet(mouseX, mouseY, random(20)))
}


class Planet{
  
  constructor(xPos, yPos, mass){
    this.pos = createVector(xPos, yPos)
    this.mass = mass + 5;
    this.inertia = createVector(random(10)-5, random(10)-5);
    this.color = color(random(100)+125, random(100)+125, random(100)+125);
  }
  
  paint(){
    fill(this.color)
    circle(this.pos.x, this.pos.y, sqrt(this.mass*100/PI)-5);
  }
  
  get m(){
    return this.mass; 
  }
  
  addM(mass){
    this.mass += mass;
  }
  
  get p(){
    return this.pos;
  }
  
  addPos(force){

    this.inertia.add(force)
    this.pos.sub(this.inertia)

  }
  
}
```

![image](https://github.com/user-attachments/assets/fdaf06d8-3bed-4d95-93b1-2fef89520d00)
