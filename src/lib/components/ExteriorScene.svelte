<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import { T } from '@threlte/core';
	import { Environment, Grid, Float, Text } from '@threlte/extras';
	import { interactivity } from '@threlte/extras';
	import Pi from '$lib/assets/models/Pi.svelte';
	import { DEG2RAD } from 'three/src/math/MathUtils';
	import { tweened, spring } from 'svelte/motion';
	import { SheetObject, Sequence, type SequenceController } from '@threlte/theatre';

	interactivity();

	let sequence: SequenceController;
	let play: (opts?: object) => Promise<boolean>;

	const dispatch = createEventDispatcher();
	const strobeInterval = 2000;
	const lightIntensity = tweened(20, { duration: strobeInterval });
	const emissiveIntensity = tweened(1, { duration: strobeInterval });
	const floatIntensity = tweened(5, { duration: 2000 });
	const rotationIntensity = tweened(0.3, { duration: 2000 });
	const scale = spring(1);
	let lockScale = false;
	let low = true;
	const boxYZ = tweened(0.8, { duration: 4000 });

	const strobeLoop = setInterval(() => {
		if (low) {
			lightIntensity.set(590);
			emissiveIntensity.set(40);
		} else {
			lightIntensity.set(20);
			emissiveIntensity.set(1);
		}
		low = !low;
	}, strobeInterval);
</script>

<SheetObject key="camera-group" let:Transform>
	<Transform>
		<T.AxesHelper scale={5} />
		<T.PerspectiveCamera makeDefault near={0.01} fov={50}>
			<!-- <OrbitControls enableDamping /> -->
		</T.PerspectiveCamera>
	</Transform>
</SheetObject>

<Sequence bind:sequence bind:play rate={1} />

<Environment
	path="/environments/"
	files="shanghai_riverside_1k.hdr"
	isBackground={false}
	format="hdr"
	groundProjection={{ radius: 200, height: 50, scale: { x: 100, y: 100, z: 100 } }}
/>

<Float floatIntensity={$floatIntensity} rotationIntensity={$rotationIntensity} rotationSpeed={2}>
	<T.Group rotation={[14 * DEG2RAD, 180 * DEG2RAD, 10 * DEG2RAD]}>
		<T.Group position={[-0.7, 0.2, 0.7]}>
			<T.Mesh
				position={[0, 0, 0]}
				scale={$scale}
				on:pointerover={() => {
					if (!lockScale) {
						scale.set(1.2);
					}
				}}
				on:pointerout={() => {
					if (!lockScale) {
						scale.set(1);
					}
				}}
				on:click={() => {
					if (!lockScale) play(); // Prevent from clicking after zooming in
					lockScale = true;
					clearInterval(strobeLoop);
					scale.set(1.2);
					setTimeout(() => {
						boxYZ.set(10);
						dispatch('zoomedIn');
					}, 5000);
					lightIntensity.set(590);
					emissiveIntensity.set(200);
					floatIntensity.set(0);
					rotationIntensity.set(0);
				}}
			>
				<T.BoxGeometry position={[0, 1, 0]} args={[0.8, $boxYZ, $boxYZ]} />
				<T.MeshStandardMaterial
					color={[0, 0, 1]}
					emissive={[0, 0, 1]}
					emissiveIntensity={$emissiveIntensity}
				/>
			</T.Mesh>
			<T.PointLight intensity={$lightIntensity} color={[0, 0, 1]} />
		</T.Group>

		<Pi />
	</T.Group>
</Float>

<Float floatIntensity={$floatIntensity} rotationIntensity={$rotationIntensity} rotationSpeed={2}>
	<Text text={'ARTIFICAL MUSEUM'} position={[0, 17, -100]} scale={100} anchorX="center" />
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
