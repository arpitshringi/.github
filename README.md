<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ramayana Alternate Ending: Jatayu Victory</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background-color: #000;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            user-select: none;
        }
        #canvas-container {
            width: 100vw;
            height: 100vh;
        }
        #ui-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            box-sizing: border-box;
            padding: 20px;
        }
        .header {
            text-align: center;
            color: #ffd700;
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.7), 0 0 20px rgba(255, 69, 0, 0.5);
        }
        .header h1 {
            margin: 0;
            font-size: 28px;
            letter-spacing: 2px;
        }
        .header p {
            margin: 5px 0 0 0;
            font-size: 14px;
            color: #fff;
        }
        #subtitle-box {
            align-self: center;
            background: rgba(0, 0, 0, 0.75);
            border: 2px solid #ffd700;
            border-radius: 10px;
            padding: 15px 25px;
            max-width: 80%;
            text-align: center;
            color: #fff;
            font-size: 18px;
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.3);
            margin-bottom: 20px;
            transition: all 0.5s ease;
        }
        .controls {
            position: absolute;
            bottom: 20px;
            right: 20px;
            pointer-events: auto;
            display: flex;
            gap: 10px;
        }
        .btn {
            background: linear-gradient(135deg, #ff4500, #ff8c00);
            color: white;
            border: none;
            padding: 10px 18px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 4px 6px rgba(0,0,0,0.3);
            transition: transform 0.2s;
        }
        .btn:hover {
            transform: scale(1.05);
            background: linear-gradient(135deg, #ff8c00, #ffd700);
            color: #000;
        }
    </style>
    <!-- Three.js Library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
</head>
<body>
    <div id="canvas-container"></div>
    <div id="ui-overlay">
        <div class="header">
            <h1>रामायण: धर्म की महा विजय</h1>
            <p>Alternate Timeline: जब जटायु ने रावण को बंदी बनाया</p>
        </div>
        <div id="subtitle-box">युद्ध प्रारम्भ... जटायु ने पुष्पक विमान पर आक्रमण किया!</div>
    </div>
    <div class="controls">
        <button class="btn" onclick="restartScene()">फिर से देखें</button>
    </div>

    <script>
        let scene, camera, renderer, controls;
        let viman, ravan, jatayu, sita;
        let chains = [];
        let clock = new THREE.Clock();
        let phase = 0; // 0: Battle, 1: Golden Trap, 2: Ravan Captured & Sita Saved
        let phaseTimer = 0;

        function init() {
            const container = document.getElementById('canvas-container');

            // Scene setup
            scene = new THREE.Scene();
            scene.fog = new THREE.FogExp2(0x1a0f2e, 0.005);

            // Camera setup
            camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(0, 15, 35);

            // Renderer
            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(window.devicePixelRatio);
            renderer.shadowMap.enabled = true;
            container.appendChild(renderer.domElement);

            // Controls
            controls = new THREE.OrbitControls(camera, renderer.domElement);
            controls.enableDamping = true;
            controls.dampingFactor = 0.05;

            // Lights
            const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
            scene.add(ambientLight);

            const sunLight = new THREE.DirectionalLight(0xffaa44, 1.2);
            sunLight.position.set(50, 50, -50);
            sunLight.castShadow = true;
            scene.add(sunLight);

            // Sky & Clouds
            createEnvironment();

            // Create Objects
            viman = createPushpakViman();
            scene.add(viman);

            ravan = createRavan();
            viman.add(ravan);
            ravan.position.set(0, 2.5, 0);

            sita = createSita();
            viman.add(sita);
            sita.position.set(-3, 2, -2);

            jatayu = createJatayu();
            scene.add(jatayu);
            jatayu.position.set(0, 15, 30);

            window.addEventListener('resize', onWindowResize);
            animate();
        }

        function createEnvironment() {
            // Sky Mesh
            const skyGeo = new THREE.SphereGeometry(300, 32, 32);
            const skyMat = new THREE.MeshBasicMaterial({
                color: 0x2c1654,
                side: THREE.BackSide
            });
            const sky = new THREE.Mesh(skyGeo, skyMat);
            scene.add(sky);

            // Clouds
            const cloudGeo = new THREE.DodecahedronGeometry(5, 1);
            const cloudMat = new THREE.MeshLambertMaterial({ color: 0x4a3b69, transparent: true, opacity: 0.6 });
            for(let i=0; i<40; i++) {
                const cloud = new THREE.Mesh(cloudGeo, cloudMat);
                cloud.position.set((Math.random()-0.5)*200, (Math.random()-0.5)*50 + 10, (Math.random()-0.5)*200);
                cloud.scale.set(Math.random()*2+1, Math.random()*1+0.5, Math.random()*2+1);
                scene.add(cloud);
            }
        }

        function createPushpakViman() {
            const vimanGroup = new THREE.Group();
            
            // Base Chariot Body (Golden)
            const bodyGeo = new THREE.CylinderGeometry(6, 4, 2, 8);
            const goldMat = new THREE.MeshStandardMaterial({ color: 0xd4af37, metalness: 0.8, roughness: 0.2 });
            const body = new THREE.Mesh(bodyGeo, goldMat);
            vimanGroup.add(body);

            // Decorative Dome
            const domeGeo = new THREE.ConeGeometry(4, 5, 8);
            const dome = new THREE.Mesh(domeGeo, goldMat);
            dome.position.y = 4;
            vimanGroup.add(dome);

            return vimanGroup;
        }

        function createRavan() {
            const ravanGroup = new THREE.Group();
            
            // Armor / Body (Dark Red)
            const bodyGeo = new THREE.CylinderGeometry(0.8, 0.6, 3, 8);
            const darkMat = new THREE.MeshStandardMaterial({ color: 0x8b0000, metalness: 0.5 });
            const body = new THREE.Mesh(bodyGeo, darkMat);
            ravanGroup.add(body);

            // 10 Heads Representation
            const headMat = new THREE.MeshStandardMaterial({ color: 0x5a3d28 });
            for(let i = -4; i <= 5; i++) {
                const headGeo = new THREE.SphereGeometry(i === 0 ? 0.5 : 0.35, 16, 16);
                const head = new THREE.Mesh(headGeo, headMat);
                head.position.set(i * 0.4, 2, 0);
                ravanGroup.add(head);
                
                // Crown
                const crownGeo = new THREE.ConeGeometry(i === 0 ? 0.4 : 0.25, 0.6, 6);
                const crownMat = new THREE.MeshStandardMaterial({ color: 0xffd700 });
                const crown = new THREE.Mesh(crownGeo, crownMat);
                crown.position.set(i * 0.4, i === 0 ? 2.6 : 2.4, 0);
                ravanGroup.add(crown);
            }

            return ravanGroup;
        }

        function createSita() {
            const sitaGroup = new THREE.Group();
            const dressMat = new THREE.MeshStandardMaterial({ color: 0xff9900 });
            const bodyGeo = new THREE.CylinderGeometry(0.4, 0.6, 2.2, 8);
            const body = new THREE.Mesh(bodyGeo, dressMat);
            sitaGroup.add(body);

            // Divine Aura Sphere
            const auraGeo = new THREE.SphereGeometry(1.8, 16, 16);
            const auraMat = new THREE.MeshBasicMaterial({ color: 0xffffff, transparent: true, opacity: 0.3 });
            const aura = new THREE.Mesh(auraGeo, auraMat);
            sitaGroup.add(aura);

            return sitaGroup;
        }

        function createJatayu() {
            const jatayuGroup = new THREE.Group();
            
            // Divine Eagle Body
            const bodyGeo = new THREE.ConeGeometry(1.5, 5, 8);
            const featherMat = new THREE.MeshStandardMaterial({ color: 0xcc3300, roughness: 0.4 });
            const body = new THREE.Mesh(bodyGeo, featherMat);
            body.rotation.x = Math.PI / 3;
            jatayuGroup.add(body);

            // Wings
            const wingGeo = new THREE.BoxGeometry(10, 0.2, 2.5);
            const wingMat = new THREE.MeshStandardMaterial({ color: 0xffd700, metalness: 0.3 });
            
            const leftWing = new THREE.Mesh(wingGeo, wingMat);
            leftWing.position.set(-5, 0, 0);
            leftWing.name = "leftWing";
            jatayuGroup.add(leftWing);

            const rightWing = new THREE.Mesh(wingGeo, wingMat);
            rightWing.position.set(5, 0, 0);
            rightWing.name = "rightWing";
            jatayuGroup.add(rightWing);

            // Glowing Beak
            const beakGeo = new THREE.ConeGeometry(0.5, 1.5, 4);
            const beakMat = new THREE.MeshStandardMaterial({ color: 0xffff00 });
            const beak = new THREE.Mesh(beakGeo, beakMat);
            beak.position.set(0, 0, -2.5);
            beak.rotation.x = -Math.PI / 2;
            jatayuGroup.add(beak);

            return jatayuGroup;
        }

        function createDivineChains() {
            const chainMat = new THREE.MeshStandardMaterial({ color: 0xffd700, emissive: 0xffaa00, emissiveIntensity: 0.8 });
            for(let i = 0; i < 8; i++) {
                const chainGeo = new THREE.TorusGeometry(0.6, 0.15, 8, 16);
                const ring = new THREE.Mesh(chainGeo, chainMat);
                ring.position.copy(ravan.position);
                ring.position.y += (i * 0.4) - 1;
                ring.rotation.x = Math.PI / 2;
                viman.add(ring);
                chains.push(ring);
            }
        }

        function updateSubtitles(text) {
            document.getElementById('subtitle-box').innerText = text;
        }

        function restartScene() {
            phase = 0;
            phaseTimer = 0;
            jatayu.position.set(0, 15, 30);
            ravan.position.set(0, 2.5, 0);
            sita.position.set(-3, 2, -2);
            chains.forEach(c => viman.remove(c));
            chains = [];
            updateSubtitles("युद्ध प्रारम्भ... जटायु ने पुष्पक विमान पर आक्रमण किया!");
        }

        function onWindowResize() {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        }

        function animate() {
            requestAnimationFrame(animate);
            const delta = clock.getDelta();
            const time = clock.getElapsedTime();

            // Viman Floating Motion
            viman.position.y = Math.sin(time * 2) * 0.8;
            viman.rotation.y += 0.005;

            // Wing Flapping Animation
            const leftWing = jatayu.getObjectByName("leftWing");
            const rightWing = jatayu.getObjectByName("rightWing");
            if(leftWing && rightWing) {
                leftWing.rotation.z = Math.sin(time * 8) * 0.3;
                rightWing.rotation.z = -Math.sin(time * 8) * 0.3;
            }

            // Timeline Animation Logic
            phaseTimer += delta;

            if (phase === 0) { // Attack Phase
                jatayu.position.x = Math.sin(time * 3) * 12;
                jatayu.position.z = Math.cos(time * 3) * 12 + 5;
                jatayu.lookAt(viman.position);

                if (phaseTimer > 5) {
                    phase = 1;
                    phaseTimer = 0;
                    createDivineChains();
                    updateSubtitles("महा ट्विस्ट! जटायु जी ने दिव्यास्त्र प्रकट कर रावण को स्वर्णिम पाश (जाकी) में बांध लिया!");
                }
            } 
            else if (phase === 1) { // Capturing Ravan
                jatayu.position.lerp(new THREE.Vector3(0, 8, 0), 0.05);
                jatayu.lookAt(ravan.position);

                // Tighten Chains on Ravan
                chains.forEach((ring, index) => {
                    ring.scale.set(0.8 + Math.sin(time * 10) * 0.05, 0.8 + Math.sin(time * 10) * 0.05, 1);
                });

                if (phaseTimer > 5) {
                    phase = 2;
                    phaseTimer = 0;
                    updateSubtitles("विजय! रावण परास्त होकर बंदी बन गया। माता सीता पूरी तरह सुरक्षित हैं!");
                }
            } 
            else if (phase === 2) { // Rescue Flight
                // Sita moves under Jatayu's protection
                sita.position.lerp(new THREE.Vector3(0, 10, -2), 0.03);
                jatayu.position.y = 12 + Math.sin(time * 2) * 0.5;
            }

            controls.update();
            renderer.render(scene, camera);
        }

        window.onload = init;
    </script>
</body>
</html>
