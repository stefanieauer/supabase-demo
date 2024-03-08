<script lang="ts">
	import { onMount } from 'svelte';
	import type { AuthSession } from '@supabase/supabase-js';
	import { supabase } from '$lib/supabase';
	import { session } from '$lib/store';

	let loading = false;
	let username: string | null = null;
	let website: string | null = null;
	let avatarUrl: string | null = null;

	onMount(() => {
		getProfile();
	});

	async function getProfile() {
		try {
			loading = true;
			const { user } = $session;

			const { data, error, status } = await supabase
				.from('profiles')
				.select('username, website, avatar_url')
				.eq('id', user.id)
				.single();

			if (error && status !== 406) throw error;

			if (data) {
				username = data.username;
				website = data.website;
				avatarUrl = data.avatar_url;
			}
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message);
			}
		} finally {
			loading = false;
		}
	}

	async function updateProfile() {
		try {
			loading = true;
			const { user } = $session;

			const updates = {
				id: user.id,
				username,
				website,
				avatar_url: avatarUrl,
				updated_at: new Date().toISOString()
			};

			const { error } = await supabase.from('profiles').upsert(updates);

			if (error) {
				throw error;
			}
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message);
			}
		} finally {
			loading = false;
		}
	}
</script>

<div class="m-8">
	<h1 class="text-7xl my-3 text-center">Profile</h1>
	<p class="text-2xl my-3 text-center">View your profile!</p>
</div>
{#if $session}
	<form on:submit|preventDefault={updateProfile} class="form-widget my-2 px-10">
		<div class="text-xl my-2">Email: {$session.user.email}</div>
		<div class="text-xl my-2">
			<label for="username">Name</label>
			<input id="username" type="text" bind:value={username} />
		</div>
		<div class="text-xl my-2">
			<label for="website">Website</label>
			<input id="website" type="text" bind:value={website} />
		</div>
		<div class="text-xl my-2">
			<button type="submit" class="button primary block btn my-5" disabled={loading}>
				{loading ? 'Saving ...' : 'Update profile'}
			</button>
		</div>
		<button type="button" class="button block btn my-4" on:click={() => supabase.auth.signOut()}>
			Sign Out
		</button>
	</form>
{:else}
	<div>seems you are not logged in</div>
{/if}
