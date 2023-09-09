<script lang="ts">
	import ExteriorScene from '$lib/components/ExteriorScene.svelte';
	import InteriorScene from '$lib/components/InteriorScene.svelte';
	import { World } from '@threlte/rapier';
	// import { Studio } from '@threlte/theatre';
	// import { dev } from '$app/environment';
	import { Canvas } from '@threlte/core';
	import { Project, Sheet } from '@threlte/theatre';
	import state from '$lib/assets/theatre/camera-state.json';
	import { fade } from 'svelte/transition';

	let showSeam = false;
	let showExterior = true;
</script>

<!-- <Studio enabled={dev} /> -->

{#if showExterior}
	<div class="h-screen w-screen bg-black">
		<Canvas>
			<Project config={{ state }}>
				<Sheet>
					<ExteriorScene
						on:zoomedIn={() => {
							showSeam = true;
							setTimeout(() => {
								showExterior = false;
							}, 500);
							setTimeout(() => {
								showSeam = false;
							}, 1000);
						}}
					/>
				</Sheet>
			</Project>
		</Canvas>
	</div>
{/if}

{#if !showExterior}
	<div class="h-screen w-screen">
		<Canvas>
			<World>
				<InteriorScene />
			</World>
		</Canvas>
	</div>
{/if}

{#if showSeam}
	<div
		in:fade={{ duration: 500 }}
		out:fade={{ duration: 1500 }}
		class="fixed top-0 z-20 h-screen w-screen bg-white"
	/>
{/if}
