<script lang="ts">
	import { supabase } from '$lib/supabase';

	let loading = false;
	let email = '';

	const handleLogin = async () => {
		try {
			loading = true;
			const { error } = await supabase.auth.signInWithOtp({ email });
			if (error) throw error;
			alert('Check your email for login link!');
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message);
			}
		} finally {
			loading = false;
		}
	};
</script>

<div class="row flex-center flex">
	<div class="col-6 form-widget" aria-live="polite">
		<form class="form-widget" on:submit|preventDefault={handleLogin}>
			<div class="px-8 text-xl">
				<label for="email">Email</label>
				<input
					id="email"
					class="inputField"
					type="email"
					placeholder="Your email"
					bind:value={email}
				/>
			</div>
			<div class="px-8">
				<button type="submit" class="button block btn my-4" aria-live="polite" disabled={loading}>
					<span>{loading ? 'Loading' : 'Send magic link'}</span>
				</button>
			</div>
		</form>
	</div>
</div>
