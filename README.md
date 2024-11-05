<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Endless Runner Game</title>
  <style>
    body { margin: 0; overflow: hidden; }
    canvas { display: block; }
  </style>
</head>
<body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/110/three.min.js"></script>
  <script>
    let scene, camera, renderer, player, obstacles = [], clock;

    init();
    animate();

    function init() {
      // Scene
      scene = new THREE.Scene();

      // Camera
      camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      camera.position.z = 5;

      // Renderer
      renderer = new THREE.WebGLRenderer();
      renderer.setSize(window.innerWidth, window.innerHeight);
      document.body.appendChild(renderer.domElement);

      // Player
      let geometry = new THREE.BoxGeometry();
      let material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
      player = new THREE.Mesh(geometry, material);
      scene.add(player);

      // Obstacles
      for (let i = 0; i < 5; i++) {
        let obstacle = new THREE.Mesh(
          new THREE.BoxGeometry(),
          new THREE.MeshBasicMaterial({ color: 0xff0000 })
        );
        obstacle.position.set(Math.random() * 4 - 2, Math.random() * 4 - 2, -5 - i * 2);
        scene.add(obstacle);
        obstacles.push(obstacle);
      }

      // Clock
      clock = new THREE.Clock();
    }

    function animate() {
      requestAnimationFrame(animate);

      let delta = clock.getDelta();
      obstacles.forEach(obstacle => {
        obstacle.position.z += delta * 2;
        if (obstacle.position.z > 5) {
          obstacle.position.z = -10;
          obstacle.position.x = Math.random() * 4 - 2;
          obstacle.position.y = Math.random() * 4 - 2;
        }
      });

      // Player controls
      document.addEventListener('keydown', function (event) {
        if (event.key === 'ArrowLeft') player.position.x -= 0.1;
        if (event.key === 'ArrowRight') player.position.x += 0.1;
      });

      renderer.render(scene, camera);
    }
  </script>
</body>
</html>
