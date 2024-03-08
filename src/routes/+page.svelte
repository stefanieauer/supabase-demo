<script>
	import { supabase } from '$lib/supabase';

	let promise = supabase.from('countries').select();
</script>

<div class="m-8">
	<h1 class="text-7xl my-3 text-center">Home</h1>
	<p class="text-2xl my-3 text-center">
		Explore our world. Discover new places. Welcome to our Home!
	</p>
	<h1 class="text-3xl my-3 mx-3">Countries:</h1>

	{#await promise}
		<div><span class="loading loading-spinner loading-lg" /></div>
	{:then result}
		<ul class="text-1.5xl list-disc px-6">
			{#each result.data as country}
				<li>{country.name}</li>
			{/each}
		</ul>
	{/await}
</div>
