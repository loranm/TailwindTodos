<script lang="ts">
	import type { Snippet } from 'svelte';

	type HeaderProps = {
		name?: string;
		fake_name?: string;
	};
	let { fake_name, name }: HeaderProps = $props();
</script>

<h1 class="text-3xl font-bold">
	<span>
		Hello {name ?? 'User'}
	</span>
</h1>

<!-- {@render secondChild()} -->

{#if fake_name}
	<p>Fake name: {fake_name}</p>
{/if}
