<script lang="ts">
	import { T, useFrame, useThrelte } from '@threlte/core';
	import { Sky, Text, Float } from '@threlte/extras';
	import { AutoColliders, CollisionGroups } from '@threlte/rapier';
	import { spring } from 'svelte/motion';
	import { type Mesh, Vector3 } from 'three';
	import Player from './Player.svelte';
	import Ground from './Ground.svelte';
	import Whale from '$lib/assets/models/Whale.svelte';
	import { DEG2RAD } from 'three/src/math/MathUtils';
	import Earth from '$lib/assets/models/Earth.svelte';
	import Robot from '$lib/assets/models/Robot.svelte';

	const { scene } = useThrelte();
	let playerMesh: Mesh;
	let positionHasBeenSet = false;
	const smoothPlayerPosX = spring(0);
	const smoothPlayerPosZ = spring(0);
	const t3 = new Vector3();
	let whaleRotation = 120 * DEG2RAD;

	useFrame(() => {
		whaleRotation -= 0.001;
		if (!playerMesh) return;
		playerMesh.getWorldPosition(t3);
		smoothPlayerPosX.set(t3.x, {
			hard: !positionHasBeenSet
		});
		smoothPlayerPosZ.set(t3.z, {
			hard: !positionHasBeenSet
		});
		if (!positionHasBeenSet) positionHasBeenSet = true;
	});
</script>

<T.DirectionalLight castShadow position={[8, 20, -3]} />

<T.Group rotation.y={whaleRotation}>
	<T.Group position={[50, 5, 0]}>
		<Float floatIntensity={20} floatRange={[-100, 100]}>
			<Whale scale={0.01} position={[0, 4, -10]} />
		</Float>
	</T.Group>
</T.Group>

<Earth position={[20, 20, -20]} scale={10} />
<Robot position={[100, 0, 100]} rotation.y={-120 * DEG2RAD} />

<T.Fog
	on:create={({ ref, cleanup }) => {
		scene.fog = ref;
		cleanup(() => (scene.fog = null));
	}}
	near={4}
	far={20}
	color={[0.88, 0.88, 0.88]}
/>

<CollisionGroups groups={[0, 15]}>
	<Ground />
</CollisionGroups>

<Text
	text={'Artificial Museum'}
	scale={6}
	color={'#000000'}
	position={[0, 4, -1.4]}
	anchorX="center"
/>
<Text
	text={'Life With Artificials'}
	scale={4}
	color={'#000000'}
	position={[0, 3, -1.4]}
	anchorX="center"
/>

<CollisionGroups groups={[0]}>
	<Player bind:playerMesh position={[0, 2, 3]} />
</CollisionGroups>
<Sky
	turbidity={0.65}
	rayleigh={0.3}
	azimuth={180}
	elevation={30}
	mieCoefficient={0.032}
	mieDirectionalG={0}
/>
