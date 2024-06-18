<script lang='ts'>
    import { onMount } from 'svelte';

    import * as THREE from 'three';
    import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
    import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
  
    let rendererContainer: HTMLElement;
  
    onMount(() => {
        // Renderer
        const renderer = new THREE.WebGLRenderer({
            antialias: true,
        });
        // renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setSize(rendererContainer.clientWidth, rendererContainer.clientHeight);
        renderer.setPixelRatio(window.devicePixelRatio > 1 ? 2 : 1);
        rendererContainer.appendChild(renderer.domElement);
    
        // Scene
        const scene = new THREE.Scene();
    
        // Camera
        const camera = new THREE.PerspectiveCamera(
            75,
            rendererContainer.clientWidth / rendererContainer.clientHeight,
            // window.innerWidth / window.innerHeight,
            0.1,
            1000
        );
        
        camera.position.y = 1.5;
        camera.position.z = 4;
        scene.add(camera);
    
        // Light
        const ambientLight = new THREE.AmbientLight('white', 0.5);
        scene.add(ambientLight);
    
        const directionalLight = new THREE.DirectionalLight('white', 1);
        directionalLight.position.x = 1;
        directionalLight.position.z = 2;
        scene.add(directionalLight);
    
        // Controls
        new OrbitControls(camera, renderer.domElement);
    
        // GLTF Loader
        const gltfLoader = new GLTFLoader();
        let mixer;
        
        gltfLoader.load("/models/ilbuni.glb", (gltf) => {
            console.log(gltf.scene.children[0]);
            const ilbuniMesh = gltf.scene.children[0];
            scene.add(ilbuniMesh);

            // 애니메이션 세팅
            mixer = new THREE.AnimationMixer(ilbuniMesh);
            const actions = [];
            actions[0] = mixer.clipAction(gltf.animations[0]);
            actions[1] = mixer.clipAction(gltf.animations[1]);
            actions[0].repetitions = 2; // 반복 회수 설정
            actions[0].clampWhenFinished = true; // 애니메이션 끝난 상태에서 멈추도록 하기
            actions[0].play();
        });


        // Animation loop
        const clock = new THREE.Clock();
    
        function draw() {
            const delta = clock.getDelta();

            // mixer 업데이트 계속 해줘야함
            // mixer 가 모두 로드된 후에 실행되도록 처리해야함
            if (mixer) mixer.update(delta);

            renderer.render(scene, camera);
            renderer.setAnimationLoop(draw);
        }
    
        function setSize() {
            // camera.aspect = window.innerWidth / window.innerHeight;
            camera.aspect = rendererContainer.clientWidth / rendererContainer.clientHeight;
            camera.updateProjectionMatrix();
            // renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setSize(rendererContainer.clientWidth, rendererContainer.clientHeight);
            renderer.render(scene, camera);
        }
    
        // Event listener
        window.addEventListener('resize', setSize);
    
        draw();
    
        return () => {
            window.removeEventListener('resize', setSize);
        };

    });
</script>
  
<div bind:this={rendererContainer} class="w-full h-screen"></div>

<style>
</style>