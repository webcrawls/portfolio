<script lang="ts">
    import { browser } from "$app/environment";
    import { onMount } from "svelte";
    import {
        WebGLRenderer,
        Mesh,
        Scene,
        MeshNormalMaterial,
        PerspectiveCamera,
        Color,
        MathUtils,
        Box3,
    } from "three";
    import { FontLoader } from "three/addons/loaders/FontLoader.js";
    import { TextGeometry } from "three/addons/geometries/TextGeometry.js";

    // sync spin with scroll position

    let { angle = $bindable(0)} = $props()

    let container: HTMLDivElement | undefined = undefined;
    let camera;
    let renderer;
    let textMesh;

    function getVisibleHeightAtDepth(depth, camera) {
        // Convert vertical fov to radians
        const vFOV = (camera.fov * Math.PI) / 180;

        // Calculate visible height at given depth
        return 2 * Math.tan(vFOV / 2) * depth;
    }

    function getVisibleWidthAtDepth(depth, camera) {
        const height = getVisibleHeightAtDepth(depth, camera);
        return height * camera.aspect;
    }

    const fitMeshToContainer = (camera, mesh) => {
        // Get the bounding box of your text
        const bbox = new Box3().setFromObject(mesh);
        const textWidth = bbox.max.x - bbox.min.x;
        const textHeight = bbox.max.y - bbox.min.y;

        // Where is your mesh? (probably z=0)
        const meshDepth = Math.abs(camera.position.z - mesh.position.z);

        // What's visible at that depth?
        const visibleHeight = getVisibleHeightAtDepth(meshDepth, camera);
        const visibleWidth = getVisibleWidthAtDepth(meshDepth, camera);

        // Target: use 80% of container to leave breathing room
        const targetHeight = visibleHeight * 0.8;
        const targetWidth = visibleWidth * 0.8;

        // Scale based on whichever constraint is tighter
        const scaleByHeight = targetHeight / textHeight;
        const scaleByWidth = targetWidth / textWidth;
        const scaleFactor = Math.min(scaleByHeight, scaleByWidth);

        mesh.scale.set(scaleFactor, scaleFactor, scaleFactor);
    };

    const onWindowResize = (container, camera, renderer) => {
        const width = container.clientWidth;
        const height = container.clientHeight;

        camera.aspect = width / height;

        // CRITICAL - without this, aspect changes don't take effect
        camera.updateProjectionMatrix();
        // fitMeshToContainer(textMesh, camera)

        renderer.setSize(width, height);
    };

    const setup = async () => {
        const loader = new FontLoader();
        const font = await loader.loadAsync(
            "/assets/fonts/comic_sans_bold.typeface.json",
        );

        const width = container?.clientWidth;
        const height = container?.clientHeight;

        console.log({ container });

        camera = new PerspectiveCamera(20, width / height, 1, 1000);
        camera.position.z = 120;

        const scene = new Scene();
        const material = new MeshNormalMaterial();
        const geometry = new TextGeometry("kaden", {
            font: font,
            size: 20,
            depth: 5,
            curveSegments: 12,
        });
        scene.position.set(0, 0, 0);

        const mesh = new Mesh(geometry, material);
        textMesh = mesh
        scene.add(mesh);
        mesh.geometry.center();

        const animate = (time) => {
            const t = time / 1000;

            const maxAngle = MathUtils.degToRad(20);
            mesh.rotation.y = Math.sin(t) * maxAngle;
            renderer.render(scene, camera);
        };

        renderer = new WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(width, height);
        renderer.setAnimationLoop(animate);
        renderer.setClearColor(0x0, 0);
        container.appendChild(renderer.domElement);
        onWindowResize(container, camera, renderer);
    };

    onMount(async () => {
        if (!browser) return;

        setTimeout(setup, 100);
    });
</script>

<svelte:window
    onresize={onWindowResize.bind(this, container, camera, renderer)}
/>

<div class="logo" bind:this={container}></div>

<style>
    .logo {
        width: 200px;
        height: 100px;
        aspect-ratio: 1/1;
    }
</style>
