<script lang="ts">
	import { browser } from '$app/environment';
	import { Pane } from 'tweakpane';

	import * as Threlte from '@threlte/core';
	import * as Three from 'three';
	import * as Utils from 'three/src/math/MathUtils';

	const gridHelper = new Three.GridHelper(20, 20);
	const axesHelper = new Three.AxesHelper(10);

	const sphere = {
		position: { x: 0, y: 4, z: 0 },
		color: '0xffffff'
	};

	function updateSpherePosition() {
		sphere.position.x = Math.round(sphere.position.x);
		sphere.position.y = Math.round(sphere.position.y);
		sphere.position.z = Math.round(sphere.position.z);
	}

	if (browser) {
		// Pane
		const pane = new Pane({ title: 'Scene' });

		// Pane: Folder
		const sphereControls = pane.addFolder({ title: 'Sphere' });

		// Pane: X Slider
		sphereControls.addInput(sphere.position, 'x', {
			min: 0,
			max: 100
		});
		sphereControls.on('change', () => {
			updateSpherePosition();
		});

		// Pane: Y Slider
		sphereControls.addInput(sphere.position, 'y', {
			min: 0,
			max: 100
		});
		sphereControls.on('change', () => {
			updateSpherePosition();
		});

		// Pane: Z Slider
		sphereControls.addInput(sphere.position, 'z', {
			min: 0,
			max: 100
		});
		sphereControls.on('change', () => {
			updateSpherePosition();
		});

		// Pane: Color Picker
		sphereControls.addInput(sphere, 'color', {
			picker: 'inline',
			expanded: true
		});
		sphereControls.on('change', ({ value }) => {
			sphere.color = value as any;
		});
	}
</script>

<div class="scene">
	<Threlte.Canvas rendererParameters={{ antialias: true }}>
		<!-- Helper -->
		<Threlte.Object3DInstance object={gridHelper} />
		<Threlte.Object3DInstance object={axesHelper} />

		<!-- Camera -->
		<Threlte.PerspectiveCamera position={{ x: 20, y: 20, z: 20 }} fov={50}>
			<Threlte.OrbitControls autoRotate />
		</Threlte.PerspectiveCamera>

		<!-- Lights -->
		<Threlte.AmbientLight color="white" intensity={0.2} />
		<Threlte.DirectionalLight
			color="white"
			intensity={2}
			position={{ x: 10, y: 20 }}
			shadow={{
				camera: { top: 10, bottom: -10, left: -10, right: 10, near: 0.1, far: 100 }
			}}
		/>

		<!-- Sphere -->
		<Threlte.Mesh
			geometry={new Three.SphereGeometry(4, 64, 64)}
			material={new Three.MeshStandardMaterial({ color: sphere.color })}
			position={sphere.position}
			receiveShadow
			castShadow
		/>

		<!-- Floor -->
		<Threlte.Mesh
			geometry={new Three.PlaneGeometry(20, 20)}
			material={new Three.MeshStandardMaterial({
				color: 'white',
				side: Three.DoubleSide
			})}
			rotation={{ x: Utils.DEG2RAD * 90 }}
			receiveShadow
		/>
	</Threlte.Canvas>

	<!-- Background -->
	<slot />
</div>

<style>
	.scene {
		width: 100%;
		height: 100%;
		position: absolute;
		inset: 0;
		background: #3494e6; /* Fallback for old browsers */
		background: -webkit-linear-gradient(
			to right,
			#ec6ead,
			#3494e6
		); /* Chrome 10-25, Safari 5.1-6 */
		background: linear-gradient(
			to right,
			#ec6ead,
			#3494e6
		); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
	}
</style>
