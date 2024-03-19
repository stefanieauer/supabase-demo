<script>
	import { supabase } from '$lib/supabase';
	let promise = supabase.from('countries').select();
	let names = supabase.from('rabbits').select();
	let name = '';

	async function sendName() {
		const response = await fetch('/api/sendName', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({ name })
		});
		const data = await response.text();
		console.log(data);
		name = '';
		names = supabase.from('rabbits').select();
	}
</script>

<div class="prose">
	<h1>APIs</h1>
	<ul>
		<li><a href="/api/hello?name=Isabell">Link zur API mit Query-Parameter</a></li>
		<li><a href="/api/hello/Isabell">Link zur API mit Route-Parameter</a></li>
	</ul>

	<form>
		<h2>All my rabbits</h2>
		<input type="text" bind:value={name} /> <br /><button class="btn" on:click={sendName}
			>Add rabbit!</button
		>
	</form>

	<h2>Rabbit names</h2>

	{#await names}
		<span class="loading loading-bars loading-lg text-primary" />
	{:then result}
		<ul>
			{#each result.data as rabbit (rabbit.id)}
				<li>{rabbit.name}</li>
			{/each}
		</ul>
	{/await}
</div>
<!-- <div class="m-8">
    <h1 class="text-8xl my-3 text-center">Home</h1>
    <p class="text-2xl my-3 text-center">
        Exolore our world. Discover new places. Welcome to our home.
    </p>
    <h1 class="text-4xl my-2 px-3">Countries:</h1>
    {#await promise}
        <div><span class="loading loading-spinner loading-lg" /></div>
    {:then result}
        <ul class="list-disc px-6 text-1.5xl">
            {#each result.data as country}
                <li>{country.name}</li>
            {/each}
        </ul>
    {/await}
</div> -->
