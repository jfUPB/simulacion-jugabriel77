``` js

const { Engine, World, Bodies, Body, Events } = Matter;

let engine, world;
let word = "LINTERNA";
let letters = [];
let letterSize = 64;
let tBody;
let spotlightRadius = 100;
let moveForce = 0.002;
let rotationSpeed = 0.01;
let isMoving = false;

function setup() {
  createCanvas(800, 400);
  textFont('Helvetica');
  textSize(letterSize);
  textAlign(CENTER, CENTER);

  engine = Engine.create();
  world = engine.world;
  world.gravity.y = 0;

  let spacing = width / (word.length + 1);

  for (let i = 0; i < word.length; i++) {
    let x = spacing * (i + 1);
    let y = height / 2;
    let isT = word[i] === 'T';

    let body = Bodies.circle(x, y, letterSize / 2, {
      restitution: 1,
      friction: 0,
      frictionAir: 0.02,
      label: word[i],
      isStatic: !isT
    });

    if (isT) tBody = body;

    World.add(world, body);
    letters.push({ body: body, isLit: false });
  }

  let thickness = 50;
  let walls = [
    Bodies.rectangle(width / 2, -thickness / 2, width, thickness, { isStatic: true }),
    Bodies.rectangle(width / 2, height + thickness / 2, width, thickness, { isStatic: true }),
    Bodies.rectangle(-thickness / 2, height / 2, thickness, height, { isStatic: true }),
    Bodies.rectangle(width + thickness / 2, height / 2, thickness, height, { isStatic: true })
  ];
  World.add(world, walls);

  // Detectar colisiones entre "T" y otras letras
  Events.on(engine, 'collisionStart', function(event) {
    for (let pair of event.pairs) {
      let labels = [pair.bodyA.label, pair.bodyB.label];

      if (labels.includes('T')) {
        let other = pair.bodyA.label === 'T' ? pair.bodyB : pair.bodyA;

        for (let l of letters) {
          if (l.body === other) {
            l.isLit = true;
          }
        }
      }
    }
  });
}

function draw() {
  background(0);
  Engine.update(engine);

  if (isMoving) {
    drawLighting();
  }

  drawLetters();
  handleMovement();
}

function drawLetters() {
  for (let l of letters) {
    let b = l.body;
    let label = b.label;

    let dx = b.position.x - tBody.position.x;
    let dy = b.position.y - tBody.position.y;
    let d = dist(tBody.position.x, tBody.position.y, b.position.x, b.position.y);

    let lightDir = createVector(-sin(tBody.angle), -cos(tBody.angle));
    let toLetter = createVector(dx, dy).normalize();

    let angleBetween = degrees(lightDir.angleBetween(toLetter));

    let col;

    if (label === "T" || l.isLit) {
      col = 255;
    } else if (d < spotlightRadius && abs(angleBetween) < 30 && isMoving) {
      col = 255;
    } else if (isMoving) {
      col = 80;
    } else {
      col = 0;
    }

    push();
    translate(b.position.x, b.position.y);
    rotate(b.angle);
    fill(col);
    noStroke();
    text(label, 0, 0);
    pop();
  }
}

function drawLighting() {
  push();
  drawingContext.save();
  translate(tBody.position.x, tBody.position.y);
  rotate(tBody.angle);

  let ctx = drawingContext;
  let gradient = ctx.createLinearGradient(0, 0, 0, -200);
  gradient.addColorStop(0, 'rgba(255, 255, 255, 1)');
  gradient.addColorStop(1, 'rgba(255, 255, 255, 0)');

  ctx.beginPath();
  ctx.moveTo(0, 0);
  ctx.lineTo(-spotlightRadius / 2, -200);
  ctx.lineTo(spotlightRadius / 2, -200);
  ctx.closePath();

  ctx.fillStyle = gradient;
  ctx.fill();
  ctx.clip();

  resetMatrix();
  drawingContext.restore();
  pop();
}

function handleMovement() {
  let force = createVector(0, 0);
  isMoving = false;

  if (keyIsDown(UP_ARROW)) {
    force.y = 1;
    isMoving = true;
  }
  if (keyIsDown(DOWN_ARROW)) {
    force.y = -1;
    isMoving = true;
  }

  if (keyIsDown(LEFT_ARROW)) {
    Body.setAngularVelocity(tBody, -rotationSpeed);
    isMoving = true;
  }
  if (keyIsDown(RIGHT_ARROW)) {
    Body.setAngularVelocity(tBody, rotationSpeed);
    isMoving = true;
  }

  if (force.mag() > 0) {
    force.normalize().mult(moveForce);

    let angle = tBody.angle;

    let xMove = force.y * sin(angle);
    let yMove = -force.y * cos(angle);

    Body.applyForce(tBody, tBody.position, { x: xMove, y: yMove });
  }
}


```
