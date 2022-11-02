<script lang="ts">
	export let size = 10
	export let url: string

	let avatarUrl: string | null = null

	$: avatarUrl = url
</script>

<div>
	<img
		src={url}
		alt={url ? 'Avatar' : 'No image'}
		class="avatar image"
		style="height: {size}em; width: {size}em;"
		referrerpolicy="no-referrer"
	/>
</div>
