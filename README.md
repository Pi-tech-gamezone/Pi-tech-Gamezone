## Hi there 👋

heres the code for my game try it out


<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>π Tech - Pixel Racer</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body { margin: 0; overflow: hidden; background: #222; }
    canvas { display: block; }
    #hud {
      position: absolute;
      top: 10px;
      left: 10px;
      color: #fff;
      font-family: sans-serif;
      font-size: 18px;
      z-index: 10;
    }
    #controls {
      position: absolute;
      bottom: 10px;
      left: 50%;
      transform: translateX(-50%);
      z-index: 20;
      display: flex;
      gap: 10px;
    }
    .btn {
      width: 50px;
      height: 50px;
      border-radius: 10px;
      background: #0ff;
      border: none;
      font-size: 20px;
    }
  </style>
</head>
<body>
  <div id="hud">Lap: 1 | Time: 0.0s</div>
  <div id="controls">
    <button class="btn" id="left">◀</button>
    <button class="btn" id="up">▲</button>
    <button class="btn" id="down">▼</button>
    <button class="btn" id="right">▶</button>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
  <script>
    let scene = new THREE.Scene();
    let camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
    let renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);let light = new THREE.DirectionalLight(0xffffff, 1);
light.position.set(10, 20, 10);
scene.add(light);

const track = new THREE.Mesh(
  new THREE.PlaneGeometry(50, 200),
  new THREE.MeshStandardMaterial({ color: 0x4444ff })
);
track.rotation.x = -Math.PI / 2;
scene.add(track);

const kart = new THREE.Mesh(
  new THREE.BoxGeometry(1, 0.5, 2),
  new THREE.MeshStandardMaterial({ color: 0xff5500 })
);
kart.position.y = 0.25;
scene.add(kart);

camera.position.set(0, 5, -5);
camera.lookAt(kart.position);

let keys = {};
document.addEventListener('keydown', e => keys[e.key.toLowerCase()] = true);
document.addEventListener('keyup', e => keys[e.key.toLowerCase()] = false);

// Mobile controls
document.getElementById('left').ontouchstart = () => keys['a'] = true;
document.getElementById('left').ontouchend = () => keys['a'] = false;
document.getElementById('right').ontouchstart = () => keys['d'] = true;
document.getElementById('right').ontouchend = () => keys['d'] = false;
document.getElementById('up').ontouchstart = () => keys['w'] = true;
document.getElementById('up').ontouchend = () => keys['w'] = false;
document.getElementById('down').ontouchstart = () => keys['s'] = true;
document.getElementById('down').ontouchend = () => keys['s'] = false;

let hud = document.getElementById("hud");
let time = 0;
let lap = 1;
let startTime = Date.now();

function animate() {
  requestAnimationFrame(animate);

  let speed = 0.2;
  let steer = 0.05;

  if (keys['w']) kart.position.z -= speed;
  if (keys['s']) kart.position.z += speed;
  if (keys['a']) kart.position.x -= steer;
  if (keys['d']) kart.position.x += steer;

  camera.position.x = kart.position.x;
  camera.position.z = kart.position.z - 5;
  camera.lookAt(kart.position);

  time = ((Date.now() - startTime) / 1000).toFixed(1);
  hud.textContent = `Lap: ${lap} | Time: ${time}s`;

  renderer.render(scene, camera);
}

animate();

  </script>
</body>
</html>
