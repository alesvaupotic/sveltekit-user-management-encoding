<script lang="ts">
	import { supabaseClient } from '$lib/supabaseClient'

	let loading = false
	let email: string

	const handleLogin = async () => {
		try {
			loading = true
			const { error } = await supabaseClient.auth.signInWithOAuth({
				provider: 'google',
				options: { redirectTo: window.location.origin }
			})
			if (error) throw error
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message)
			}
		} finally {
			loading = false
		}
	}
</script>

<form class="row flex-center flex" on:submit|preventDefault={handleLogin}>
	<div class="col-6 form-widget">
		<h1 class="header">Supabase + SvelteKit</h1>
		<p class="description">Sign in wth Google</p>
		<div>
			<input
				type="submit"
				class="button block"
				value={loading ? 'Loading' : 'Sign In'}
				disabled={loading}
			/>
		</div>
	</div>
</form>
