<script lang="ts">
	interface Props {
		color?: string;
		active?: boolean;
	}

	let { color = '#3B82F6', active = false }: Props = $props();

	// "Ghost" intensity for an extremely subtle, non-straining feel
	let bloomScale = $derived(active ? 'scale-[1.3]' : 'scale-[1.1]');
	let bloomOpacity = $derived(active ? 'opacity-20' : 'opacity-05');
	let coreOpacity = $derived(active ? 'opacity-15' : 'opacity-02');
</script>

<div class="fixed inset-0 pointer-events-none overflow-hidden bg-black z-0">
	<!-- MINIMALIST RADIANCE - Extremely Subtle -->
	<div 
		class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[140vw] h-[140vw] transition-all duration-2000 origin-center {bloomScale} {bloomOpacity}"
		style="background: radial-gradient(circle at center, {color}22 0%, {color}02 40%, transparent 70%); filter: blur(140px);"
	></div>

	<!-- GHOST BLOOM - Barely Noticeable -->
	<div 
		class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[180vw] h-[180vw] transition-all duration-2000 origin-center scale-110 {active ? 'opacity-15' : 'opacity-02'}"
		style="background: radial-gradient(circle at center, {color}08 0%, transparent 60%); filter: blur(180px);"
	></div>

	<!-- SYMMETRICAL ORGANIC WAVES (SVG) - Near Transparent -->
	<div class="absolute inset-0 flex items-center justify-center">
		<svg 
			class="w-[120vw] h-[120vw] transition-all duration-2000 filter blur-[120px] md:blur-[180px] {coreOpacity}" 
			viewBox="0 0 1000 1000"
		>
			<defs>
				<radialGradient id="central-grad" cx="50%" cy="50%" r="50%">
					<stop offset="0%" style="stop-color:{color}; stop-opacity:0.2" />
					<stop offset="100%" style="stop-color:transparent; stop-opacity:0" />
				</radialGradient>
			</defs>

			<!-- Ultra-slow, barely visible orbits -->
			<g class="animate-prestige-orbit-slow">
				<circle cx="500" cy="500" r="450" fill="url(#central-grad)" class="opacity-10 transform scale-[1.3]" />
				<path 
					d="M150,500 Q500,50 850,500 T150,500" 
					fill="url(#central-grad)" 
					class="opacity-15"
				/>
			</g>

			<g class="animate-prestige-orbit-reverse">
				<path 
					d="M50,500 Q500,950 950,500 T50,500" 
					fill="url(#central-grad)" 
					class="opacity-10"
				/>
			</g>
		</svg>
	</div>
</div>

<style>
	.animate-prestige-orbit-slow {
		animation: prestige-orbit 90s linear infinite;
		transform-origin: 50% 50%;
	}

	.animate-prestige-orbit-reverse {
		animation: prestige-orbit-reverse 120s linear infinite;
		transform-origin: 50% 50%;
	}

	@keyframes prestige-orbit {
		from { transform: rotate(0deg) scale(1.02); }
		50% { transform: rotate(180deg) scale(1.05); }
		to { transform: rotate(360deg) scale(1.02); }
	}

	@keyframes prestige-orbit-reverse {
		from { transform: rotate(360deg) scale(1.05); }
		50% { transform: rotate(180deg) scale(1.02); }
		to { transform: rotate(0deg) scale(1.05); }
	}
</style>
