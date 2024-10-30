<script lang="ts">
	import Header from '$lib/Header.svelte';
	import { error } from '@sveltejs/kit';

	type FormStep = {
		id: keyof FormState;
		question: string;
		type: 'text' | 'date' | 'color';
		errorMessage: string;
	};

	type FormSteps = Array<FormStep>;

	const QUESTIONS: FormSteps = [
		{ id: 'name', question: `What's your name?`, type: 'text', errorMessage: 'Name is required' },
		{
			id: 'birthday',
			question: `What's your birthday?`,
			type: 'date',
			errorMessage: 'Birthday is required'
		},
		{
			id: 'color',
			question: `What's your favorite color?`,
			errorMessage: 'Birthday is required',
			type: 'color'
		}
	];

	type FormState = {
		name: string;
		birthday: string;
		color: string;
		step: number;
		error: Error;
		answers: Record<string, string> | null;
	};

	type Error = null | {
		message: string;
	};

	let formState: FormState = $state({
		name: '',
		birthday: '',
		color: '',
		step: 0,
		error: null,
		answers: null
	});

	let displayStepIndex = $derived(formState.step + 1);
	let formEnded = $derived(formState.step >= QUESTIONS.length);

	function nextStep(id: keyof FormState) {
		if (formState[id] === '') {
			formState = {
				...formState,
				error: { message: QUESTIONS.find((q) => q.id === id)?.errorMessage ?? '' }
			};
			return;
		}

		formState = {
			...formState,
			answers: { ...formState.answers, [id]: formState[id] as string },
			error: null,
			step: ++formState.step
		};
	}

	$inspect(formState);
</script>

<main>
	<Header name={formState.name}></Header>

	{#if formEnded}
		{@render thankYou()}
	{:else}
		{@render stepTitle(displayStepIndex)}
	{/if}

	{#each QUESTIONS as question, step (question.id)}
		{#if step === formState.step}
			{@render formStep(question)}
		{/if}
	{/each}
</main>

{#snippet formStep({ id, question, type }: FormStep)}
	<div>
		<label for={id}>{question}</label>

		<input {type} {id} bind:value={formState[id]} />

		{#if formState.error}
			{@render error(formState.error.message)}
		{/if}
	</div>
	<button type="submit" onclick={() => nextStep(id)}>Next</button>
{/snippet}

{#snippet thankYou()}
	<h1>Thank you!</h1>
{/snippet}

{#snippet error(message: string)}
	<p class="error">{message}</p>
{/snippet}

{#snippet stepTitle(index: number)}
	<h1>Step {index}</h1>
{/snippet}

<style>
	.error {
		color: red;
	}
</style>
