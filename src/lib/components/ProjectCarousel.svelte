<script lang="ts">
	import { fade } from "svelte/transition";

	const projects = [
		{
			name: "TaskWave",
			url: "https://taskwave-ai.netlify.app/",
			color: "#FFB000",
		},
		{
			name: "ToneWave",
			url: "https://tonewave-ai.netlify.app/",
			color: "#10B981",
		},
		{
			name: "TimeWave",
			url: "https://timewave-ai.netlify.app/",
			color: "#0070FF",
		},
	];

	let currentIndex = $state(1); // Start with the middle one

	function next() {
		currentIndex = (currentIndex + 1) % projects.length;
	}

	function prev() {
		currentIndex = (currentIndex - 1 + projects.length) % projects.length;
	}

	function setIndex(index: number) {
		currentIndex = index;
	}
</script>

<div
	class="relative w-full h-[60vh] flex items-center justify-center perspective-1000"
>
	<div
		class="relative w-full max-w-5xl h-full flex items-center justify-center"
	>
		{#each projects as project, i}
			{@const offset = i - currentIndex}
			{@const absOffset = Math.abs(offset)}
			{@const isLeft =
				offset < 0 || (currentIndex === 0 && i === projects.length - 1)}
			{@const isRight =
				offset > 0 || (currentIndex === projects.length - 1 && i === 0)}

			<!-- Simple carousel logic for 3 items -->
			{@const position =
				i === currentIndex
					? "center"
					: i < currentIndex
						? "left"
						: "right"}

			<div
				class="absolute transition-all duration-700 ease-out cursor-pointer group"
				style="
					transform: 
						translateX({position === 'center'
					? '0'
					: position === 'left'
						? '-110%'
						: '110%'}) 
						scale({position === 'center' ? '1' : '0.8'}) 
						rotateY({position === 'center'
					? '0deg'
					: position === 'left'
						? '25deg'
						: '-25deg'});
					z-index: {position === 'center' ? 30 : 20};
					opacity: {position === 'center' ? 1 : 0.4};
				"
				onclick={() => setIndex(i)}
			>
				<div
					class="w-[300px] h-[500px] md:w-[600px] md:h-[400px] bg-black border border-white/10 rounded-2xl overflow-hidden shadow-2xl relative group-hover:border-white/30 transition-colors"
				>
					<!-- Iframe -->
					<iframe
						src={project.url}
						title={project.name}
						class="w-full h-full pointer-events-none select-none bg-black"
						loading="lazy"
					></iframe>

					<!-- Overlay to capture clicks and prevent scrolling/nav in iframe -->
					<div class="absolute inset-0 bg-transparent z-10"></div>

					<!-- Info Bar -->
					<div
						class="absolute bottom-0 left-0 right-0 p-4 bg-gradient-to-t from-black/80 to-transparent backdrop-blur-sm sm:opacity-0 group-hover:opacity-100 transition-opacity"
					>
						<div class="flex justify-between items-center">
							<span
								class="text-xs font-bold tracking-widest uppercase font-mono"
								style="color: {project.color}"
								>{project.name}</span
							>
							<span class="text-[9px] font-mono text-white/50"
								>{project.url.replace("https://", "")}</span
							>
						</div>
					</div>
				</div>
			</div>
		{/each}
	</div>

	<!-- Navigation Arrows -->
	<button
		onclick={prev}
		class="absolute left-8 z-40 p-4 rounded-full border border-white/10 bg-white/5 backdrop-blur-md hover:bg-white/10 transition-all active:scale-95"
		aria-label="Previous"
	>
		<svg
			width="20"
			height="20"
			viewBox="0 0 24 24"
			fill="none"
			stroke="currentColor"
			stroke-width="2"
		>
			<path d="M15 18l-6-6 6-6" />
		</svg>
	</button>

	<button
		onclick={next}
		class="absolute right-8 z-40 p-4 rounded-full border border-white/10 bg-white/5 backdrop-blur-md hover:bg-white/10 transition-all active:scale-95"
		aria-label="Next"
	>
		<svg
			width="20"
			height="20"
			viewBox="0 0 24 24"
			fill="none"
			stroke="currentColor"
			stroke-width="2"
		>
			<path d="M9 18l6-6-6-6" />
		</svg>
	</button>

	<!-- Indicators -->
	<div class="absolute bottom-[-40px] flex gap-2">
		{#each projects as _, i}
			<button
				onclick={() => setIndex(i)}
				class="w-8 h-1 transition-all duration-300 {i === currentIndex
					? 'bg-white'
					: 'bg-white/10'}"
			></button>
		{/each}
	</div>
</div>

<style>
	.perspective-1000 {
		perspective: 1000px;
	}
</style>
