<script lang="ts">
	import Header from '../lib/Header.svelte';

	type Status = 'OPEN' | 'CLOSED';

	let name = $state('Scott');
	let status: Status = $state('OPEN');

	let fullName = $derived(`${name} NomDeFamille`);

	function onclick(): void {
		status = status === 'CLOSED' ? 'OPEN' : 'CLOSED';
	}

	$inspect(status);
</script>

<Header {name}></Header>

<p>{fullName}</p>

<div class="w-full max-w-xs mb-4">
	<form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
		<div class="mb-4">
			<label class="block text-gray-700 text-sm font-bold mb-2" for="username"> Username </label>
			<input
				class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
				id="username"
				type="text"
				placeholder="Username"
				bind:value={name}
			/>
		</div>
	</form>
</div>

<p>
	The store is now <span class="rounded-full py-2 px-4">{status}</span>
</p>
<button type="button" {onclick} class="btn btn-blue mx-8">Toggle Status</button>

<style>
	.btn {
		@apply font-bold py-2 px-4 rounded;
	}
	.btn-blue {
		@apply bg-blue-500 text-white;
	}
	.btn-blue:hover {
		@apply bg-blue-700;
	}
</style>
