<script>
	import { onMount } from 'svelte';
	import { supabase } from '$lib/supabase';
	import '../app.postcss';
	import { session, theme } from '$lib/store';
	onMount(() => {
		supabase.auth.getSession().then(({ data }) => {
			$session = data.session;
		});

		supabase.auth.onAuthStateChange((_event, _session) => {
			$session = _session;
		});
	});
</script>

<div data-theme={$theme} class="min-h-screen p-3 px-7 py-5">
	<nav class="flex items-center gap-8 m-4 px-8 text-1.5xl">
		<a href="/" class="text-center text-1.5xl">Home</a>|
		<a href="/profile" class="text-center text-1.5xl">Profil</a>|
		<a href="/login" class="text-center text-1.5xl">Login</a>
	</nav>

	<select bind:value={$theme} class="fixed top-0 right-0">
		<option value="cyberpunk">cyberpunk</option>
		<option value="coffee">coffee</option>
		<option value="synthwave">synthwave</option>
	</select>

	<slot />
</div>
