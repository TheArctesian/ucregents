<script lang="ts">
	const images = [
		{ src: '/signal-2025-10-30-210836.jpeg', alt: 'UC Regent Carmen Chu' },
		{ src: '/signal-2025-10-30-210836_002.jpeg', alt: 'UC Regent' },
		{ src: '/signal-2025-10-30-210836_003.jpeg', alt: 'UC Regent Hadi Makarechian' },
		{ src: '/signal-2025-10-30-210836_004.jpeg', alt: 'UC Regent' },
		{ src: '/signal-2025-10-30-210836_005.jpeg', alt: 'UC Regent Lark Park' },
		{ src: '/signal-2025-10-30-210836_006.jpeg', alt: 'UC Regent' },
		{ src: '/signal-2025-10-30-210836_007.jpeg', alt: 'UC Regent' }
	];

	let selectedImage = $state<string | null>(null);

	function openLightbox(src: string) {
		selectedImage = src;
	}

	function closeLightbox() {
		selectedImage = null;
	}

	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Escape') {
			closeLightbox();
		}
	}
</script>

<svelte:window onkeydown={handleKeydown} />

<main class="container mx-auto px-4 py-12 md:py-20">
	<!-- Hero Section -->
	<header class="mb-16 text-center">
		<h1
			class="mb-6 text-6xl font-black tracking-tight text-red-600 uppercase md:text-8xl"
			style="text-shadow: 3px 3px 0 rgba(0,0,0,0.5);"
		>
			WANTED!
		</h1>
		<div class="mx-auto mb-6 h-1 w-32 bg-red-600"></div>
		<h2 class="text-2xl font-bold tracking-wide text-white uppercase md:text-4xl">UC Regents</h2>
	</header>

	<!-- Image Gallery -->
	<div class="grid grid-cols-1 gap-8 md:grid-cols-2 lg:grid-cols-3">
		{#each images as image (image.src)}
			<button
				type="button"
				onclick={() => openLightbox(image.src)}
				class="group relative overflow-hidden rounded-lg border-4 border-red-600 bg-black shadow-2xl transition-all duration-300 hover:scale-105 hover:shadow-red-600/50"
			>
				<img
					src={image.src}
					alt={image.alt}
					class="h-full w-full object-cover transition-opacity duration-300 group-hover:opacity-90"
				/>
				<div
					class="absolute inset-0 flex items-center justify-center bg-black/0 opacity-0 transition-all duration-300 group-hover:bg-black/30 group-hover:opacity-100"
				>
					<span class="text-lg font-bold text-white uppercase">Click to Enlarge</span>
				</div>
			</button>
		{/each}
	</div>

	<!-- Footer -->
	<footer class="mt-20 border-t border-red-600 pt-8 text-center">
		<p class="text-sm text-gray-400">Demand accountability from UC leadership</p>
	</footer>
</main>

<!-- Lightbox Modal -->
{#if selectedImage}
	<div
		role="button"
		tabindex="0"
		onclick={closeLightbox}
		onkeydown={(e) => e.key === 'Enter' && closeLightbox()}
		class="fixed inset-0 z-50 flex items-center justify-center bg-black/95 p-4 backdrop-blur-sm"
	>
		<button
			type="button"
			onclick={closeLightbox}
			class="absolute top-4 right-4 text-5xl font-bold text-white transition-colors hover:text-red-600"
			aria-label="Close lightbox"
		>
			&times;
		</button>
		<div class="max-h-[90vh] max-w-4xl">
			<img
				src={selectedImage}
				alt="UC Regent WANTED Poster"
				class="h-auto w-full rounded-lg border-4 border-red-600 shadow-2xl"
			/>
		</div>
	</div>
{/if}
