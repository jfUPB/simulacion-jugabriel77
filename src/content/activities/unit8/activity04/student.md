#### Final

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/MjagcMZJM)



``` js

p5.disableFriendlyErrors = true;

// CONSTANTES
const HALFPI = Math.PI * 0.5;
const RAND   = (min, max) => Math.random() * (max - min) + min;

// VARIABLES GLOBALES
let W, H;
let baseColors, baseColorIndex;
let gradientPairs, currentGradient;
let positions, velocities, voronoi;

let audioInput, audioLoaded = false;
let ampAnalyzer, fft, peakDetect;
let speedFactor = 1;

// Suavizado de número de puntos
let targetPoints = 20;
let displayPoints = 20;

// Sistema de partículas
let particles = [];
const PARTICLE_LIFE = 30;
const PARTICLES_PER_BEAT = 40;

function setup() {
  [W, H] = [windowWidth, windowHeight];
  createCanvas(W, H);
  strokeWeight(4);
  fill('#fff');

  // Colores de fondo
  baseColors = [
    '#fcd5ce','#cdb4db','#b5ead7','#f9f871','#b5b9ff',
    '#ffb5a7','#ffda77','#a3c4f3','#d0f4de','#e4c1f9',
    '#1e1e24','#2e2e2e','#f0efeb','#f6f5f5','#ffd6e0',
    '#ffe5b4','#d9faff','#fffecb','#c7f9cc','#dad7cd'
  ];
  baseColorIndex = 0;

  // Pares de gradiente
  gradientPairs = [
    ['#ffcad4','#d00000'],['#ff9a8b','#ff6a88'],
    ['#f9bec7','#6a0572'],['#d8e2dc','#ff8fab'],
    ['#8ecae6','#219ebc'],['#a2d2ff','#ef476f'],
    ['#bde0fe','#f72585'],['#e0aaff','#7400b8'],
    ['#9bf6ff','#ff006e'],['#ffd6a5','#fdffb6'],
    ['#e63946','#f1faee'],['#06d6a0','#118ab2'],
    ['#3a0ca3','#7209b7'],['#ffbe0b','#fb5607'],
    ['#8338ec','#ff006e'],['#ff5d8f','#ffb5e8'],
    ['#80ed99','#38a3a5'],['#ffadad','#ff6392'],
    ['#b5ead7','#5f0f40'],['#fdfcdc','#ffc6ff'],
    ['#b8c0ff','#cdb4db']
  ];
  currentGradient = 0;

  // Botón de carga de audio
  let btn = createButton("🎵 Subir Audio");
  btn.position(20, 60);
  btn.mousePressed(handleUpload);

  // Analizadores de sonido
  ampAnalyzer = new p5.Amplitude();
  fft         = new p5.FFT();
  peakDetect  = new p5.PeakDetect(20, 20000, 0.2, 20);

  // Posiciones y velocidades iniciales (hasta 120 puntos)
  positions = Array.from({ length: 120 }, () =>
    Float64Array.from({ length: 2 }, (_, i) => RAND(0, i & 1 ? H : W))
  );
  velocities = Array.from({ length: 120 }, () =>
    Float64Array.from({ length: 2 }, () => RAND(-0.1, 0.1))
  );

  voronoi = d3.voronoi().extent([[0, 0], [W, H]]);
}

function draw() {
  background(baseColors[baseColorIndex]);

  // 1) Beat detect para speedFactor y generar partículas
  fft.analyze();
  peakDetect.update(fft);
  if (peakDetect.isDetected) {
    speedFactor = 5;
    spawnParticles();
  }
  speedFactor = lerp(speedFactor, 1, 0.1);

  // 2) Suavizado del número de puntos basado en amplitud general
  let genAmp = 0;
  if (audioLoaded && audioInput.isPlaying()) {
    genAmp = ampAnalyzer.getLevel();
    genAmp = constrain(genAmp * 10, 0, 1);
  }
  // Calcula objetivo y suaviza
  targetPoints = map(genAmp, 0, 1, 20, 120);
  displayPoints = lerp(displayPoints, targetPoints, 0.05);
  let numPoints = floor(displayPoints);

  // 3) Energía de medios → saturación
  let midEnergy = audioLoaded && audioInput.isPlaying()
                ? fft.getEnergy("mid")
                : 0;

  // 4) Diagrama Voronoi con puntos activos + mouse
  const active = positions.slice(0, numPoints);
  const allPos = active.concat([ Float64Array.from([mouseX, mouseY]) ]);
  const polys  = voronoi(allPos).polygons();

  for (let i = 0; i < numPoints + 1; i++) {
    let isMouse = (i === numPoints);
    let pos     = isMouse ? [mouseX, mouseY] : active[i];

    if (!isMouse) {
      let v = velocities[i];
      v[0] += RAND(-0.1, 0.1) * speedFactor;
      v[1] += RAND(-0.1, 0.1) * speedFactor;
      pos[0] += v[0]; pos[1] += v[1];
      v[0] *= 0.99;   v[1] *= 0.99;
      if (pos[0] <= 4 || pos[0] >= W - 4) v[0] *= -1;
      if (pos[1] <= 4 || pos[1] >= H - 4) v[1] *= -1;
    }

    let cell = polys[i];
    if (!cell) continue;
    let verts  = cell.map(v => createVector(v[0], v[1]));
    let rverts = roundCorners(verts, 15);

    // Color dinámico con saturación de midEnergy
    let d     = dist(pos[0], pos[1], mouseX, mouseY);
    let t     = constrain(map(d, 0, 300, 1, 0), 0, 1);
    let pulse = sin(frameCount * 0.02 + i) * 0.5 + 0.5;

    colorMode(RGB);
    let cA   = color(gradientPairs[currentGradient][0]);
    let cB   = color(gradientPairs[currentGradient][1]);
    let mixC = lerpColor(cA, cB, t * pulse);

    colorMode(HSB, 360, 100, 100);
    let h = hue(mixC),
        s = map(midEnergy, 0, 255, 10, 100),
        b = brightness(mixC);
    fill(h, s, b);
    noStroke();

    beginShape();
    rverts.forEach(v => vertex(v.x, v.y));
    endShape(CLOSE);

    colorMode(RGB);
    point(pos[0], pos[1]);
  }

  // 5) Actualizar y dibujar partículas
  updateParticles();

  // UI
  fill(0);
  textSize(14);
  noStroke();
  text(`Puntos: ${numPoints}`, 20, 20);
  text(`Speed: ${nf(speedFactor,1,2)}`, 20, 40);
  text(`Gradiente: ${currentGradient+1}/${gradientPairs.length}`, 20, 60);
}

function spawnParticles() {
  for (let i = 0; i < PARTICLES_PER_BEAT; i++) {
    let x = RAND(0, W);
    let y = RAND(0, H);
    let angle = RAND(0, TWO_PI);
    let speed = RAND(1, 4);
    particles.push({ x, y, vx: cos(angle)*speed, vy: sin(angle)*speed, life: PARTICLE_LIFE });
  }
}

function updateParticles() {
  noStroke();
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.x += p.vx; p.y += p.vy;
    p.life--;
    fill(255, 255, 255, map(p.life, 0, PARTICLE_LIFE, 0, 200));
    ellipse(p.x, p.y, 4);
    if (p.life <= 0) particles.splice(i, 1);
  }
}

function handleUpload() {
  let fi = createFileInput(handleFile);
  fi.hide();
  fi.elt.click();
}

function handleFile(file) {
  if (file.type === 'audio') {
    if (audioInput && audioInput.isPlaying()) audioInput.stop();
    audioInput = loadSound(file.data, () => { audioInput.loop(); audioLoaded = true; });
  } else {
    alert("Por favor sube un .mp3 o .wav válido.");
  }
}

function mousePressed() {
  if (mouseButton === LEFT) {
    currentGradient = (currentGradient + 1 + Math.floor(RAND(1, gradientPairs.length))) % gradientPairs.length;
  }
}

function mouseWheel(event) {
  baseColorIndex = (baseColorIndex + (event.delta>0 ? 1 : -1) + baseColors.length) % baseColors.length;
  return false;
}

function roundCorners(verts, m) {
  const P = [], N = verts.length, n = 15;
  for (let i = 0; i < N; i++) {
    let sv = verts[(i+N-2)%N], pv = verts[(i+N-1)%N], cv = verts[i], nv = verts[(i+1)%N], ev = verts[(i+2)%N];
    let pb = bisector(sv,pv,cv,m), cb = bisector(pv,cv,nv,m), nb = bisector(cv,nv,ev,m);
    let v = cb[1], md = Infinity, nearest = null;
    [pb,nb].forEach(ib => {
      let is = intersect(cb[0],cb[1],ib[0],ib[1]);
      if (is) {
        let d = cv.dist(is); if (d < md) { md = d; nearest = is; }
      }
    });
    if (nearest) v = nearest;
    let d1 = p5.Vector.sub(cv,pv).normalize(), d2 = p5.Vector.sub(nv,cv).normalize();
    let op1 = orthoProjection(d1,cv,v), op2 = orthoProjection(d2,cv,v);
    let dn = p5.Vector.sub(op2,v), dp = p5.Vector.sub(op1,v);
    let ang = dn.angleBetween(dp), th = ang/n, ph = Math.atan2(op2.y-v.y,op2.x-v.x), r = op1.dist(v);
    for (let j = n; j > 0; j--) {
      let x = Math.cos(th*j+ph)*r*0.9, y = Math.sin(th*j+ph)*r*0.9;
      P.push(createVector(x+v.x, y+v.y));
    }
  }
  return P;
}

function orthoProjection(dir, ep, op) {
  let dv = p5.Vector.sub(op,ep);
  dir = dir.copy().mult(dv.dot(dir));
  return p5.Vector.add(ep,dir);
}

function intersect(p1,p2,p3,p4) {
  let uA = ((p4.x-p3.x)*(p1.y-p3.y)-(p4.y-p3.y)*(p1.x-p3.x))/((p4.y-p3.y)*(p2.x-p1.x)-(p4.x-p3.x)*(p2.y-p1.y));
  let uB = ((p2.x-p1.x)*(p1.y-p3.y)-(p2.y-p1.y)*(p1.x-p3.x))/((p4.y-p3.y)*(p2.x-p1.x)-(p4.x-p3.x)*(p2.y-p1.y));
  if (uA>=0&&uA<=1&&uB>=0&&uB<=1) return createVector(p1.x+uA*(p2.x-p1.x), p1.y+uA*(p2.y-p1.y));
}

function bisector(pv, cv, nv, m) {
  let d1 = p5.Vector.sub(cv,pv).normalize(), d2 = p5.Vector.sub(nv,cv).normalize();
  let sum= p5.Vector.add(d1,d2), s = sum.mag()/d1.dot(sum), v = sum.rotate(-HALFPI).setMag(s*m).add(cv);
  return [cv, v];
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
  W = windowWidth; H = windowHeight;
  voronoi.extent([[0,0],[W,H]]);
}


```
