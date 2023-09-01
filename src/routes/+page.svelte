<script lang="ts">
	import Scene from '$lib/components/Scene.svelte';
	import { Studio } from '@threlte/theatre';
	import { Canvas } from '@threlte/core';
	import { dev } from '$app/environment';
</script>

<Studio enabled={dev} />

<div class="h-screen w-screen bg-black">
	<Canvas>
		<Scene />
	</Canvas>
</div>
