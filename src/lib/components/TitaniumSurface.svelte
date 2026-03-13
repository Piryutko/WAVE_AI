<script lang="ts">
	interface Props {
		color?: string;
		active?: boolean;
	}

	let { color = '#3B82F6', active = false }: Props = $props();

	// Subtle shine intensity
	let shineOpacity = $derived(active ? 'opacity-20' : 'opacity-05');
	let shineScale = $derived(active ? 'scale-125' : 'scale-100');
</script>

<div class="fixed inset-0 pointer-events-none overflow-hidden bg-[#050505] z-0">
	<!-- BASE METAL TEXTURE -->
	<div class="absolute inset-0 bg-[#080808]">
		<!-- Brushed Metal Effect (Linear Gradients) -->
		<div class="absolute inset-0 opacity-20"
			 style="background: repeating-linear-gradient(90deg, transparent, transparent 2px, rgba(255,255,255,0.03) 3px, transparent 4px);">
		</div>
		<div class="absolute inset-0 opacity-10"
			 style="background: repeating-linear-gradient(0deg, transparent, transparent 1px, rgba(255,255,255,0.02) 2px, transparent 3px);">
		</div>
	</div>

	<!-- MACRO GRAIN (SVG Noise) -->
	<div class="absolute inset-0 opacity-[0.15] mix-blend-overlay pointer-events-none">
		<svg width="100%" height="100%">
			<filter id="titanium-noise">
				<feTurbulence type="fractalNoise" baseFrequency="0.8" numOctaves="4" stitchTiles="stitch" />
				<feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0" />
			</filter>
			<rect width="100%" height="100%" filter="url(#titanium-noise)" />
		</svg>
	</div>

	<!-- SPECULAR SHINE (Dynamic Light) -->
	<div 
		class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[150vw] h-[150vw] transition-all duration-1000 origin-center {shineScale} {shineOpacity}"
		style="background: radial-gradient(circle at center, {color}22 0%, transparent 60%); filter: blur(100px);"
	></div>

	<!-- VIGNETTE (Industrial Depth) -->
	<div class="absolute inset-0 shadow-[inset_0_0_200px_rgba(0,0,0,0.8)]"></div>
	<div class="absolute inset-0 bg-gradient-to-b from-black/40 via-transparent to-black/60"></div>
</div>

<style>
	/* Subtle drift for the metal texture to feel "alive" */
	div {
		will-change: transform, opacity;
	}
</style>
