<script lang="ts">
	import { T } from '@threlte/core';
	import { Grid } from '@threlte/extras';
	import { AutoColliders } from '@threlte/rapier';
</script>

<T.Group position={[0, -0.5, 0]}>
	<Grid
		position={[0, 0.6, 0]}
		infiniteGrid={true}
		fadeDistance={20}
		sectionThickness={1}
		sectionColor={'#ffffff'}
		cellColor={'#ffffff'}
	/>
	<AutoColliders shape={'cuboid'}>
		<T.Mesh receiveShadow>
			<T.BoxGeometry args={[200, 1, 200]} />
			<T.MeshStandardMaterial color={[0.9, 0.9, 0.9]} />
		</T.Mesh>
	</AutoColliders>
</T.Group>
