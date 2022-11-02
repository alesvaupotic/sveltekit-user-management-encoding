<script lang="ts">
	import { onMount } from 'svelte'
	import type { AuthSession } from '@supabase/supabase-js'
	import { supabaseClient } from '$lib/supabaseClient'
	import Avatar from './Avatar.svelte'

	export let session: AuthSession

	let loading = false
	let username: string | null = null
	let fullName: string | null = null
	let avatarUrl: string | null = null

	onMount(() => {
		getProfile()
	})

	const getProfile = async () => {
		const { user } = session

		username = user.user_metadata.email
		fullName = user.user_metadata.full_name
		avatarUrl = user.user_metadata.avatar_url
	}

	async function signOut() {
		try {
			loading = true
			let { error } = await supabaseClient.auth.signOut()
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

<form class="form-widget">
	<Avatar bind:url={avatarUrl} size={10} />
	<div>
		<label for="email">Email</label>
		<input id="email" type="text" value={session.user.email} disabled />
	</div>
	<div>
		<label for="username">Name</label>
		<input id="username" type="text" bind:value={username} />
	</div>
	<div>
		<label for="full_name">Full Name</label>
		<input id="full_name" type="text" bind:value={fullName} />
	</div>

	<div>
		<button class="button block" on:click={signOut} disabled={loading}> Sign Out </button>
	</div>
</form>
