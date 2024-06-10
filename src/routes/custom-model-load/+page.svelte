<script>
    import { onMount } from 'svelte';
    import * as THREE from 'three';
    import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
    import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
  
    let rendererContainer;
  
    onMount(() => {
      // Renderer
      const renderer = new THREE.WebGLRenderer({
        antialias: true,
      });
    //   renderer.setSize(window.innerWidth, window.innerHeight);

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
      const controls = new OrbitControls(camera, renderer.domElement);
  
      // GLTF Loader
      const gltfLoader = new GLTFLoader();
      gltfLoader.load('/models/ilbuni.glb', (gltf) => {
        const ilbuniMesh = gltf.scene.children[0];
        scene.add(ilbuniMesh);
      });
  
      // Animation loop
      const clock = new THREE.Clock();
  
      function draw() {
        const delta = clock.getDelta();
        controls.update(delta);
  
        renderer.render(scene, camera);
        requestAnimationFrame(draw);
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
  
  <div bind:this={rendererContainer}></div>


  <style>
    div {
        /* border: 10px solid red; */
        width: 100%;
        height: 100vh;
    }

    canvas {
        display: block;
        width: 100%;
        height: 100%;
    }
  </style>