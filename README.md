<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Heart 3D</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      background-color: black;
    }
    canvas {
      display: block;
    }
  </style>
</head>
<body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
  <script>
    // Khởi tạo scene, camera, renderer
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    // Tạo hình trái tim bằng particles
    const heartShape = new THREE.Shape();
    const x = 0, y = 0;
    heartShape.moveTo(x + 5, y + 5);
    heartShape.bezierCurveTo(x + 5, y + 5, x + 4, y, x, y);
    heartShape.bezierCurveTo(x - 6, y, x - 6, y + 7, x - 6, y + 7);
    heartShape.bezierCurveTo(x - 6, y + 11, x - 3, y + 15.4, x + 5, y + 19);
    heartShape.bezierCurveTo(x + 13, y + 15.4, x + 16, y + 11, x + 16, y + 7);
    heartShape.bezierCurveTo(x + 16, y + 7, x + 16, y, x + 10, y);
    heartShape.bezierCurveTo(x + 7, y, x + 5, y + 5, x + 5, y + 5);

    const points = heartShape.getPoints(200);
    const geometry = new THREE.BufferGeometry().setFromPoints(points);
    const material = new THREE.PointsMaterial({ color: 0xff66cc, size: 0.1 });
    const heart = new THREE.Points(geometry, material);
    heart.scale.set(0.5, 0.5, 0.5);
    scene.add(heart);

    // Thêm dòng chữ xoay
    const textLoader = new THREE.FontLoader();
    textLoader.load('https://threejs.org/examples/fonts/helvetiker_regular.typeface.json', function(font) {
      const textGeometry = new THREE.TextGeometry('Anh yêu em nhiều lắm 💖', {
        font: font,
        size: 0.5,
        height: 0.01
      });
      const textMaterial = new THREE.MeshBasicMaterial({ color: 0xffffff });
      const text = new THREE.Mesh(textGeometry, textMaterial);
      text.position.set(-5, -2, 0);
      scene.add(text);

      function animateText() {
        text.rotation.y += 0.01;
        requestAnimationFrame(animateText);
      }
      animateText();
    });

    // Vị trí camera
    camera.position.z = 10;

    // Animation
    function animate() {
      requestAnimationFrame(animate);
      heart.rotation.y += 0.01;
      renderer.render(scene, camera);
    }
    animate();

    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });
  </script>
</body>
</html>
