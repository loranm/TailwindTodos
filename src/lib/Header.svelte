<script lang="ts">
	type HeaderProps = { name: string; fake_name?: string };
	let { name, fake_name }: HeaderProps = $props();
</script>

<h1 class="text-3xl font-bold">Hello from our {name}</h1>

{#if fake_name}
	<p>Fake name: {fake_name}</p>
{:else}
	<p>No fake name provided</p>
{/if}
