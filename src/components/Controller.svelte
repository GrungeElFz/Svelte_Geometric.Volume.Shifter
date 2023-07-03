<script lang="ts">
	import { browser } from '$app/environment';
	import { Pane } from 'tweakpane';

	import * as Threlte from '@threlte/core';
	import * as Three from 'three';
	import * as Utils from 'three/src/math/MathUtils';

	const sphere = {
		position: { x: 0, y: 4, z: 0 },
		color: '0xffffff'
	};

	const floor = {
		color: '0xffffff'
	};

	const autoRotate = {
		rotate: true
	};

	const helpers = {
		gridHelper: {
			size: 20,
			divisions: 20
		},
		axesHelper: {
			size: 10
		}
	};

	$: gridHelper = new Three.GridHelper(helpers.gridHelper.size, helpers.gridHelper.divisions);
	$: axesHelper = new Three.AxesHelper(helpers.axesHelper.size);

	function updateSpherePosition() {
		sphere.position.x = Math.round(sphere.position.x);
		sphere.position.y = Math.round(sphere.position.y);
		sphere.position.z = Math.round(sphere.position.z);
	}

	function updateHelper() {
		helpers.gridHelper.size = Math.round(helpers.gridHelper.size);
		helpers.gridHelper.divisions = Math.round(helpers.gridHelper.divisions);
	}

	if (browser) {
		// Pane: ------------------ Scene ------------------
		const pane = new Pane({ title: 'Scene' });

		// Pane: ------------------ Auto Rotate ------------------
		const rotateControls = pane.addFolder({ title: 'Auto Rotate' });

		// Rotate: Auto Rotate
		rotateControls.addInput(autoRotate, 'rotate');
		rotateControls.on('change', ({ value }) => {
			autoRotate.rotate = value as boolean;
		});

		// Pane: ------------------ Helpers ------------------
		const helperControls = pane.addTab({
			pages: [{ title: 'Grid' }, { title: 'Axes' }]
		});

		// Helper: Grid Size
		helperControls.pages[0]
			.addInput(helpers.gridHelper, 'size', {
				step: 1,
				min: 0,
				max: 50
			})
			.on('change', ({ value }) => {
				updateHelper();
			});

		// Helper: Grid Divisions
		helperControls.pages[0]
			.addInput(helpers.gridHelper, 'divisions', {
				step: 1,
				min: 0,
				max: 50
			})
			.on('change', ({ value }) => {
				updateHelper();
			});

		// Helper: Axes Size
		helperControls.pages[1]
			.addInput(helpers.axesHelper, 'size', {
				step: 1,
				min: 0,
				max: 30
			})
			.on('change', ({ value }) => {
				updateHelper();
			});

		// Pane: ------------------ Sphere ------------------
		const sphereControls = pane.addFolder({ title: 'Sphere' });

		// Sphere: X Slider
		sphereControls.addInput(sphere.position, 'x', {
			step: 1,
			min: -20,
			max: 20
		});
		sphereControls.on('change', () => {
			updateSpherePosition();
		});

		// Sphere: Y Slider
		sphereControls.addInput(sphere.position, 'y', {
			step: 1,
			min: -20,
			max: 20
		});
		sphereControls.on('change', () => {
			updateSpherePosition();
		});

		// Sphere: Z Slider
		sphereControls.addInput(sphere.position, 'z', {
			step: 1,
			min: -20,
			max: 20
		});
		sphereControls.on('change', () => {
			updateSpherePosition();
		});

		// Sphere: Color Picker
		sphereControls.addInput(sphere, 'color', {
			picker: 'inline',
			expanded: true
		});
		sphereControls.on('change', ({ value }) => {
			sphere.color = value as any;
		});

		// Pane: ------------------ Floor ------------------
		const floorControls = pane.addFolder({ title: 'Floor' });

		// Floor: Color Floor
		floorControls.addInput(floor, 'color', {
			picker: 'inline',
			expanded: true
		});
		floorControls.on('change', ({ value }) => {
			floor.color = value as any;
		});
	}
</script>

<Threlte.Canvas rendererParameters={{ antialias: true }}>
	<!-- Helper -->
	<Threlte.Object3DInstance object={gridHelper} />
	<Threlte.Object3DInstance object={axesHelper} />

	<!-- Camera -->
	<Threlte.PerspectiveCamera position={{ x: 20, y: 20, z: 20 }} fov={50}>
		<Threlte.OrbitControls autoRotate={autoRotate.rotate} />
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
			color: floor.color,
			side: Three.DoubleSide
		})}
		rotation={{ x: Utils.DEG2RAD * 90 }}
		receiveShadow
	/>
</Threlte.Canvas>
