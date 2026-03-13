<script lang="ts">
	import Carousel from "$lib/Carousel.svelte";

	let systemTime = $state(new Date().toLocaleTimeString());
	let titleColor = $state("#FFFFFF");

	onMount(() => {
		const interval = setInterval(() => {
			systemTime = new Date().toLocaleTimeString();
		}, 1000);
		return () => clearInterval(interval);
	});

	import { onMount } from "svelte";
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="" />
	<link
		href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700;800&family=Inter:wght@400;700;900&family=Noto+Sans+JP:wght@400;900&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<!-- Scrolling Marquee -->
<div class="marquee-container fixed top-6 w-full z-30 overflow-hidden pointer-events-none">
	<div class="marquee-content font-jp text-[10px] text-white/10 tracking-[0.4em] uppercase whitespace-nowrap">
		<span>TASK_WAVE_AI タスク・ウェーブ・エーアイ TIME_WAVE_AI タイム・ウェーブ・エーアイ TONE_WAVE_AI トーン・ウェーブ・エーアイ // </span>
		<span>TASK_WAVE_AI タスク・ウェーブ・エーアイ TIME_WAVE_AI タイム・ウェーブ・エーアイ TONE_WAVE_AI トーン・ウェーブ・エーアイ // </span>
		<span>TASK_WAVE_AI タスク・ウェーブ・エーアイ TIME_WAVE_AI タイム・ウェーブ・エーアイ TONE_WAVE_AI トーン・ウェーブ・エーアイ // </span>
		<span>TASK_WAVE_AI タスク・ウェーブ・エーアイ TIME_WAVE_AI タイム・ウェーブ・エーアイ TONE_WAVE_AI トーン・ウェーブ・エーアイ // </span>
	</div>
</div>

<main
	class="fixed inset-0 overflow-hidden flex flex-col items-center justify-center bg-black text-white selection:bg-white selection:text-black font-sans"
>
	<!-- App Title / Header - Minimalist -->
	<header class="absolute top-24 text-center z-20 pointer-events-none">
		<h1
			class="text-6xl md:text-9xl font-black tracking-tighter leading-none transition-colors duration-700"
			style:color={titleColor}
		>
			WAVE AI
		</h1>
	</header>

	<!-- Draggable Carousel Section -->
	<div class="relative z-10 w-full flex flex-col items-center translate-y-12">
		<Carousel onSelect={(color) => (titleColor = color)} />

		<!-- Contact Info Only -->
		<div
			class="mt-8 font-mono text-[10px] text-white/40 tracking-[0.2em] uppercase"
		>
			<span
				class="hover:text-white cursor-default transition-all duration-300"
				>maximpiryutko@gmail.com</span
			>
		</div>
	</div>

	<!-- System Status Footer -->
	<footer
		class="fixed bottom-12 left-12 font-mono text-[9px] text-white/20 tracking-[0.2em] z-20"
	>
		<p>LOCAL_TIME: {systemTime}</p>
		<p>PROD_LINK: ACTIVE</p>
	</footer>

	<footer
		class="fixed bottom-12 right-12 font-mono text-[9px] text-white/20 tracking-[0.3em] z-20 text-right"
	>
		<p>WAVE_TERMINAL // v.5.1</p>
		<p>READY_FOR_WORK</p>
	</footer>
</main>

<style>
	:global(body) {
		margin: 0;
		background: #000;
		font-family: "Inter", sans-serif;
		-webkit-font-smoothing: antialiased;
		overflow: hidden;
	}

	:global(.font-mono) {
		font-family: "JetBrains Mono", monospace;
	}

	.font-jp {
		font-family: "Noto Sans JP", sans-serif;
	}

	.marquee-container {
		mask-image: linear-gradient(to right, transparent, black 10%, black 90%, transparent);
	}

	.marquee-content {
		display: inline-block;
		animation: marquee 40s linear infinite;
	}

	@keyframes marquee {
		0% { transform: translateX(0); }
		100% { transform: translateX(-50%); }
	}
</style>
