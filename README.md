# My3d-website-
A stunning 3D animated website using Three.js. Features rotating Earth, glowing effects, and interactive animations.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D Glowing Earth with Stars</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/examples/js/postprocessing/EffectComposer.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/examples/js/postprocessing/RenderPass.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/examples/js/postprocessing/UnrealBloomPass.js"></script>
    <style>
        body { margin: 0; overflow: hidden; background-color: black; }
        canvas { display: block; }
    </style>
</head>
<body>
    <script>
        // Scene और Camera सेटअप करें
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // Texture लोड करें
        const textureLoader = new THREE.TextureLoader();
        const earthTexture = textureLoader.load("https://upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Earth_Eastern_Hemisphere.jpg/800px-Earth_Eastern_Hemisphere.jpg");

        // Sphere (Earth) बनाएं
        const geometry = new THREE.SphereGeometry(2, 64, 64);
        const material = new THREE.MeshStandardMaterial({ map: earthTexture });
        const earth = new THREE.Mesh(geometry, material);
        scene.add(earth);

        // Glow Effect जोड़ें
        const glowMaterial = new THREE.MeshBasicMaterial({ color: 0x00ff00, transparent: true, opacity: 0.1 });
        const glowSphere = new THREE.Mesh(new THREE.SphereGeometry(2.2, 64, 64), glowMaterial);
        scene.add(glowSphere);

        // स्टार्स (तारे) जोड़ें
        function createStars() {
            const starsGeometry = new THREE.BufferGeometry();
            const starsVertices = [];
            for (let i = 0; i < 5000; i++) {
                starsVertices.push((Math.random() - 0.5) * 2000);
                starsVertices.push((Math.random() - 0.5) * 2000);
                starsVertices.push((Math.random() - 0.5) * 2000);
            }
            starsGeometry.setAttribute('position', new THREE.Float32BufferAttribute(starsVertices, 3));
            const starsMaterial = new THREE.PointsMaterial({ color: 0xffffff });
            const stars = new THREE.Points(starsGeometry, starsMaterial);
            scene.add(stars);
        }
        createStars();

        // लाइटिंग सेटअप करें
        const light = new THREE.PointLight(0xffffff, 1.5);
        light.position.set(5, 5, 5);
        scene.add(light);

        // कैमरा पोजीशन सेट करें
        camera.position.z = 5;

        // एनीमेशन लूप
        function animate() {
            requestAnimationFrame(animate);
            earth.rotation.y += 0.002; // पृथ्वी को घुमाएं
            glowSphere.rotation.y += 0.002; // Glow भी घुमाएं
            renderer.render(scene, camera);
        }
        animate();

        // Responsive स्क्रीन के लिए
        window.addEventListener('resize', () => {
            renderer.setSize(window.innerWidth, window.innerHeight);
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
        });
    </script>
</body>
</html>
