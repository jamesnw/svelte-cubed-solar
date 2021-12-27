<script lang="ts">
	import * as THREE from 'three';
	import * as SC from 'svelte-cubed';

	let width = 1;
	let height = 0.01;
	let depth = 2;
	let azimuth = 0;
	let tilt = 45;

	$: azimuthRadians = (azimuth * Math.PI) / 180;
	$: tiltRadians = (tilt * Math.PI) / 180;

	let material = {
		color: '#cccccc',
		metalness: 0.8,
		roughness: 0.4,
		opacity: 0.5,
		transparent: false
	};
</script>

<SC.Canvas
	antialias
	background={new THREE.Color('papayawhip')}
	fog={new THREE.FogExp2('papayawhip', 0.1)}
	shadows
>
	<SC.Group position={[0, -height / 2, 0]}>
		<SC.Mesh
			geometry={new THREE.PlaneGeometry(50, 50)}
			material={new THREE.MeshStandardMaterial({ color: 'burlywood' })}
			rotation={[-Math.PI / 2, 0, 0]}
			receiveShadow
		/>
		<SC.Primitive
			object={new THREE.GridHelper(50, 50, 'papayawhip', 'papayawhip')}
			position={[0, 0.001, 0]}
		/>
	</SC.Group>

	<SC.Mesh
		geometry={new THREE.BoxGeometry()}
		material={new THREE.MeshStandardMaterial(material)}
		scale={[width, height, depth]}
		rotation={[0, azimuthRadians, tiltRadians]}
		position={[0, 1, 1]}
		castShadow
	/>

	<SC.PerspectiveCamera position={[0, 2, 6]} />
	<SC.OrbitControls enableZoom={false} maxPolarAngle={Math.PI * 0.51} />
	<SC.AmbientLight intensity={0.6} />
	<SC.DirectionalLight
		intensity={0.6}
		position={[-2, 3, 2]}
		shadow={{ mapSize: [2048, 2048] }}
	/>
</SC.Canvas>

<div class="controls">
	<label
		><input type="range" bind:value={width} min={0.1} max={3} step={0.1} /> width</label
	>
	<label
		><input type="range" bind:value={depth} min={0.1} max={3} step={0.1} /> depth</label
	>
	<label
		><input type="range" bind:value={azimuth} min={-180} max={180} step={1} /> azimuth</label
	>{azimuth}
	<label
		><input type="range" bind:value={tilt} min={-90} max={90} step={1} /> tilt</label
	>{tilt}
</div>

<style>
	.controls {
		position: absolute;
		left: 1em;
		top: 1em;
	}

	label {
		display: flex;
		width: 60px;
		gap: 0.5em;
		align-items: center;
	}

	input {
		width: 80px;
		margin: 0;
	}
</style>
