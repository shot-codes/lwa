<script lang="ts">
	import { T } from '@threlte/core';
	import { Environment, Grid, Float } from '@threlte/extras';
	import { interactivity, OrbitControls } from '@threlte/extras';
	import Pi from '$lib/assets/models/Pi.svelte';
	import { DEG2RAD } from 'three/src/math/MathUtils';
	import { tweened, type Tweened, spring } from 'svelte/motion';

	interactivity();

	const cameraPosition: Tweened<[number, number, number]> = tweened([0, 3, 7], { duration: 5000 });
	const strobeInterval = 2000;
	const lightIntensity = tweened(20, { duration: strobeInterval });
	const emissiveIntensity = tweened(1, { duration: strobeInterval });
	const scale = spring(1);
	let lockScale = false;
	let low = true;
	let pulse = true;

	setInterval(() => {
		if (pulse) {
			if (low) {
				lightIntensity.set(590);
				emissiveIntensity.set(4);
			} else {
				lightIntensity.set(20);
				emissiveIntensity.set(1);
			}
			low = !low;
		}
	}, strobeInterval);
</script>

<T.PerspectiveCamera
	makeDefault
	fov={50}
	position={$cameraPosition}
	on:create={({ ref }) => {
		ref.lookAt(0, 1, 0);
	}}
>
	<OrbitControls enableDamping />
</T.PerspectiveCamera>

<Environment
	path="/environments/"
	files="shanghai_riverside_1k.hdr"
	isBackground={false}
	format="hdr"
	groundProjection={{ radius: 200, height: 50, scale: { x: 100, y: 100, z: 100 } }}
/>

<Float rotationIntensity={0.15} rotationSpeed={2}>
	<T.Group rotation={[14 * DEG2RAD, 180 * DEG2RAD, 10 * DEG2RAD]}>
		<T.Group position={[-0.7, 0.2, 0.7]}>
			<T.Mesh
				position={[0, 0, 0]}
				scale={$scale}
				on:pointerover={() => {
					if (!lockScale) {
						scale.set(2);
					}
				}}
				on:pointerout={() => {
					if (!lockScale) {
						scale.set(1);
					}
				}}
				on:click={() => {
					pulse = false;
					lockScale = true;
					scale.set(2);
					lightIntensity.set(590);
					emissiveIntensity.set(4);
					cameraPosition.set([-0.7, 0.1, 0.7]);
				}}
			>
				<T.PointLight intensity={$lightIntensity} color={[0, 0, 1]} />
				<T.BoxGeometry position={[0, 1, 0]} args={[0.5, 0.5, 0.5]} />
				<T.MeshStandardMaterial
					color={[0, 0, 1]}
					emissive={[0, 0, 1]}
					emissiveIntensity={$emissiveIntensity}
				/>
			</T.Mesh>
		</T.Group>

		<Pi />
	</T.Group>
</Float>

<Float rotationIntensity={0.15} rotationSpeed={2}>
	<Grid
		position.y={-5}
		sectionThickness={1}
		infiniteGrid
		cellColor="#99dd99"
		sectionColor="#99ff99"
		sectionSize={10}
		cellSize={2}
	/>
</Float>
