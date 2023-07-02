<script lang="ts">
	import * as Threlte from '@threlte/core';
	import * as Three from 'three';
	import * as Utils from 'three/src/math/MathUtils';
</script>

<Threlte.Canvas>
	<Threlte.PerspectiveCamera position={{ x: 20, y: 20, z: 20 }} fov={50}>
		<Threlte.OrbitControls />
	</Threlte.PerspectiveCamera>

	<Threlte.AmbientLight color="white" />

	<Threlte.Mesh
		geometry={new Three.SphereGeometry(4, 64, 64)}
		material={new Three.MeshStandardMaterial({ color: 'white' })}
	/>
</Threlte.Canvas>

<div class="scene">
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
