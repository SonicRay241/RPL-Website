<script lang="ts">
    import * as THREE from 'three'
    import { onMount } from 'svelte';
    import img0 from "../lib/static/0.jpg"
    import img1 from "../lib/static/1.jpg"
    import img2 from "../lib/static/2.jpg"
    import img3 from "../lib/static/3.jpg"

    onMount( () => {
        //Texture Loader
        const textureLoader = new THREE.TextureLoader()

        // Canvas
        const canvas: HTMLCanvasElement = document.querySelector('canvas.webgl')!

        // Scene
        const scene = new THREE.Scene()

        const geometry = new THREE.PlaneGeometry(1, 1.3)

        const material0 = new THREE.MeshBasicMaterial({
            map: textureLoader.load(img0)
        })
        const image0 = new THREE.Mesh(geometry, material0)
        image0.position.set(0, 0, 0)

        const material1 = new THREE.MeshBasicMaterial({
            map: textureLoader.load(img1)
        })
        const image1 = new THREE.Mesh(geometry, material1)
        image1.position.set(1.3, 0, 0)

        const material2 = new THREE.MeshBasicMaterial({
            map: textureLoader.load(img2)
        })
        const image2 = new THREE.Mesh(geometry, material2)
        image2.position.set(2.6, 0, 0)

        const material3 = new THREE.MeshBasicMaterial({
            map: textureLoader.load(img3)
        })
        const image3 = new THREE.Mesh(geometry, material3)
        image3.position.set(3.9, 0, 0)

        scene.add(image0)
        scene.add(image1)
        scene.add(image2)
        scene.add(image3)
        
        

        // Lights
        const pointLight = new THREE.PointLight(0xffffff, 0.1)
        pointLight.position.x = 2
        pointLight.position.y = 3
        pointLight.position.z = 4
        scene.add(pointLight)

        /**
         * Sizes
         */
        const sizes = {
            width: window.innerWidth,
            height: window.innerHeight
        }

        window.addEventListener('resize', () =>
        {
            // Update sizes
            sizes.width = window.innerWidth
            sizes.height = window.innerHeight

            // Update camera
            camera.aspect = sizes.width / sizes.height
            camera.updateProjectionMatrix()

            // Update renderer
            renderer.setSize(sizes.width, sizes.height)
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
        })

        /**
         * Camera
         */
        // Base camera
        const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100)
        camera.position.x = 0
        camera.position.y = 0
        camera.position.z = 2
        scene.add(camera)

        // Controls
        // const controls = new OrbitControls(camera, canvas)
        // controls.enableDamping = true
        let position = 0
        let y = 0
        const onMouseWheel = (event: any) => {
            y += event.deltaY * 0.0007
        }
        
        window.addEventListener("wheel", onMouseWheel)

        /**
         * Renderer
         */
        const renderer = new THREE.WebGLRenderer({
            canvas: canvas
        })
        renderer.setSize(sizes.width, sizes.height)
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

        const tick = () =>
        {

            // const elapsedTime = clock.getElapsedTime()

            position += y
            y *= .9
            camera.position.x = position

            // Update Orbital Controls
            // controls.update()

            // Render
            renderer.render(scene, camera)

            // Call tick again on the next frame
            window.requestAnimationFrame(tick)
        }

        tick()
            })
</script>
<svelte:head>
    <style>
        *
        {
            margin: 0;
            padding: 0;
        }

        html,
        body
        {
            height: 100vh;
            font-family: 'Poppins';
        }

        .webgl
        {
            position: fixed;
            top: 0;
            left: 0;
            outline: none;
        }
    </style>
</svelte:head>
<canvas class="webgl"></canvas>
