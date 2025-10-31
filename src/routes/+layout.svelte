<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
	import { dev } from '$app/environment';
	import { injectAnalytics } from '@vercel/analytics/sveltekit';

	injectAnalytics({ mode: dev ? 'development' : 'production' });
</script>

<svelte:head>
	<title>UC Regents WANTED</title>
	<meta
		name="description"
		content="Accountability for UC Regents and their political connections"
	/>
	<link rel="icon" href={favicon} />
</svelte:head>

<div class="min-h-screen bg-black text-white">
	{@render children()}
</div>
