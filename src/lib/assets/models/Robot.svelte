<!--
Auto-generated by: https://github.com/threlte/threlte/tree/main/packages/gltf
Command: npx @threlte/gltf@1.0.1 scene.gltf --transform
Author: Willy Decarpentrie (https://sketchfab.com/skudgee)
License: CC-BY-4.0 (http://creativecommons.org/licenses/by/4.0/)
Source: https://sketchfab.com/3d-models/biped-robot-801d2a245e4a4405a0c2152b35b5e486
Title: Biped robot
-->

<script>
	import { Group } from 'three';
	import { T, forwardEventHandlers } from '@threlte/core';
	import { useGltf, useGltfAnimations } from '@threlte/extras';

	export const ref = new Group();

	const gltf = useGltf('/models/robot-transformed.glb', { useDraco: true });
	export const { actions, mixer } = useGltfAnimations(gltf, ref);

	const component = forwardEventHandlers();
	$: {
		$actions.Robot_Soldat?.play();
	}
</script>

<T is={ref} dispose={false} {...$$restProps} bind:this={$component}>
	{#await gltf}
		<slot name="fallback" />
	{:then gltf}
		<T.Group name="Sketchfab_Scene">
			<T.Group name="Sketchfab_model" rotation={[-Math.PI / 2, 0, 0]}>
				<T.Group name="RobotFBX" rotation={[Math.PI / 2, 0, 0]}>
					<T.Group name="Object_2">
						<T.Group name="RootNode">
							<T.Group name="Object_4">
								<T is={gltf.nodes._rootJoint} />
								<T.Group name="Ground" position={[0, -4.34, 0]} rotation={[-Math.PI / 2, 0, 0]}>
									<T.Mesh
										name="Ground_Ground_0"
										geometry={gltf.nodes.Ground_Ground_0.geometry}
										material={gltf.materials.Ground}
									/>
								</T.Group>
								<T.SkinnedMesh
									name="Object_7"
									geometry={gltf.nodes.Object_7.geometry}
									material={gltf.materials.Metal}
									skeleton={gltf.nodes.Object_7.skeleton}
								/>
								<T.SkinnedMesh
									name="Object_8"
									geometry={gltf.nodes.Object_8.geometry}
									material={gltf.materials.Paint}
									skeleton={gltf.nodes.Object_8.skeleton}
								/>
								<T.SkinnedMesh
									name="Object_9"
									geometry={gltf.nodes.Object_9.geometry}
									material={gltf.materials.Plastic}
									skeleton={gltf.nodes.Object_9.skeleton}
								/>
							</T.Group>
						</T.Group>
					</T.Group>
				</T.Group>
			</T.Group>
		</T.Group>
	{:catch error}
		<slot name="error" {error} />
	{/await}

	<slot {ref} />
</T>
