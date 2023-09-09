<script lang="ts">
	import { Canvas } from '@threlte/core';
	import { World } from '@threlte/rapier';
	import InteriorScene from '$lib/components/InteriorScene.svelte';
</script>

<div class="w-screen h-screen">
	<Canvas>
		<World>
			<InteriorScene />
		</World>
	</Canvas>
</div>
