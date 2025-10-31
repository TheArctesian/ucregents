<script lang="ts">
	import { onMount } from 'svelte';

	const images = [
		{ src: '/signal-2025-10-30-210836.jpeg', alt: 'UC Regent Carmen Chu' },
		{ src: '/signal-2025-10-30-210836_002.jpeg', alt: 'UC Regent' },
		{ src: '/signal-2025-10-30-210836_003.jpeg', alt: 'UC Regent Hadi Makarechian' },
		{ src: '/signal-2025-10-30-210836_004.jpeg', alt: 'UC Regent' },
		{ src: '/signal-2025-10-30-210836_005.jpeg', alt: 'UC Regent Lark Park' },
		{ src: '/signal-2025-10-30-210836_006.jpeg', alt: 'UC Regent' },
		{ src: '/signal-2025-10-30-210836_007.jpeg', alt: 'UC Regent' }
	];

	let currentIndex = $state(0);
	let isPaused = $state(false);
	let intervalId: number | null = null;
	let progressPercent = $state(0);

	function nextImage() {
		currentIndex = (currentIndex + 1) % images.length;
		resetProgress();
	}

	function prevImage() {
		currentIndex = (currentIndex - 1 + images.length) % images.length;
		resetProgress();
	}

	function goToImage(index: number) {
		currentIndex = index;
		resetProgress();
	}

	function togglePause() {
		isPaused = !isPaused;
		if (isPaused) {
			if (intervalId) clearInterval(intervalId);
		} else {
			startCarousel();
		}
	}

	function resetProgress() {
		progressPercent = 0;
	}

	function startCarousel() {
		if (intervalId) clearInterval(intervalId);

		// Reset progress
		progressPercent = 0;

		// Update progress bar smoothly (update every 100ms)
		const progressInterval = setInterval(() => {
			if (!isPaused) {
				progressPercent += (100 / (15000 / 100)); // 15 seconds total
				if (progressPercent >= 100) {
					progressPercent = 100;
				}
			}
		}, 100);

		// Auto-advance every 15 seconds
		intervalId = setInterval(() => {
			if (!isPaused) {
				nextImage();
			}
		}, 15000) as unknown as number;

		return () => {
			clearInterval(progressInterval);
			if (intervalId) clearInterval(intervalId);
		};
	}

	function handleKeydown(event: KeyboardEvent) {
		switch (event.key) {
			case 'ArrowRight':
				nextImage();
				break;
			case 'ArrowLeft':
				prevImage();
				break;
			case ' ':
				event.preventDefault();
				togglePause();
				break;
			case 'Escape':
				window.history.back();
				break;
		}
	}

	onMount(() => {
		const cleanup = startCarousel();
		return cleanup;
	});
</script>

<svelte:window onkeydown={handleKeydown} />

<div class="fixed inset-0 z-50 flex flex-col bg-black overflow-hidden">
	<!-- Close button -->
	<button
		type="button"
		onclick={() => window.history.back()}
		class="absolute top-2 right-2 z-10 rounded-full bg-black/50 p-2 text-3xl font-bold text-white transition-all hover:bg-red-600 hover:scale-110 md:top-4 md:right-4 md:p-3 md:text-4xl"
		aria-label="Exit carousel"
	>
		&times;
	</button>

	<!-- Main carousel container -->
	<div class="relative flex flex-1 items-center justify-center min-h-0">
		<!-- Previous button -->
		<button
			type="button"
			onclick={prevImage}
			class="absolute left-2 z-10 rounded-full bg-black/50 p-3 text-2xl font-bold text-white transition-all hover:bg-red-600 hover:scale-110 md:left-4 md:p-4 md:text-3xl lg:p-6"
			aria-label="Previous image"
		>
			&#8249;
		</button>

		<!-- Image display -->
		<div class="relative h-full w-full flex items-center justify-center p-4 md:p-6 lg:p-12">
			{#each images as image, index (image.src)}
				{#if index === currentIndex}
					<img
						src={image.src}
						alt={image.alt}
						class="max-h-full max-w-full rounded-lg border-4 border-red-600 object-contain shadow-2xl transition-opacity duration-500"
					/>
				{/if}
			{/each}
		</div>

		<!-- Next button -->
		<button
			type="button"
			onclick={nextImage}
			class="absolute right-2 z-10 rounded-full bg-black/50 p-3 text-2xl font-bold text-white transition-all hover:bg-red-600 hover:scale-110 md:right-4 md:p-4 md:text-3xl lg:p-6"
			aria-label="Next image"
		>
			&#8250;
		</button>
	</div>

	<!-- Progress bar -->
	<div class="flex-shrink-0 p-2 md:p-3 lg:p-4">
		<div class="relative h-1 w-full overflow-hidden rounded-full bg-gray-800 md:h-2">
			<div
				class="h-full bg-red-600 transition-all duration-100 ease-linear"
				style="width: {progressPercent}%"
			></div>
		</div>
	</div>
</div>
