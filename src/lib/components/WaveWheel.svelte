<script lang="ts">
	interface Props {
		onHover: (id: string | null) => void;
		active: string | null;
	}

	let { onHover, active }: Props = $props();

	// Math for SVG sectors with gap
	const centerX = 100;
	const centerY = 100;
	const outerRadius = 85;
	const innerRadius = 35; // Donut shape for a more "divided" and modern look
	const gapAngle = 4; // Gap between sectors in degrees

	function getPath(startAngle: number, endAngle: number) {
		const sAngle = startAngle + gapAngle / 2;
		const eAngle = endAngle - gapAngle / 2;
		
		const startOuter = polarToCartesian(centerX, centerY, outerRadius, eAngle);
		const endOuter = polarToCartesian(centerX, centerY, outerRadius, sAngle);
		const startInner = polarToCartesian(centerX, centerY, innerRadius, eAngle);
		const endInner = polarToCartesian(centerX, centerY, innerRadius, sAngle);
		
		const largeArcFlag = eAngle - sAngle <= 180 ? "0" : "1";
		
		return [
			"M", startOuter.x, startOuter.y,
			"A", outerRadius, outerRadius, 0, largeArcFlag, 0, endOuter.x, endOuter.y,
			"L", endInner.x, endInner.y,
			"A", innerRadius, innerRadius, 0, largeArcFlag, 1, startInner.x, startInner.y,
			"Z"
		].join(" ");
	}

	function polarToCartesian(centerX: number, centerY: number, radius: number, angleInDegrees: number) {
		const angleInRadians = (angleInDegrees - 90) * Math.PI / 180.0;
		return {
			x: centerX + (radius * Math.cos(angleInRadians)),
			y: centerY + (radius * Math.sin(angleInRadians))
		};
	}

	const sectors = [
		{ id: 'task', label: 'TASK', color: '#FFB800', start: 0, end: 120 },
		{ id: 'time', label: 'TIME', color: '#3B82F6', start: 120, end: 240 },
		{ id: 'tone', label: 'TONE', color: '#2DD4BF', start: 240, end: 360 }
	];
</script>

<div class="relative w-full h-full flex items-center justify-center">
	<!-- Rotating outer mechanical rings - Very subtle, thin lines -->
	<div class="absolute inset-0 border border-white/[0.03] rounded-full animate-[spin_60s_linear_infinite]"></div>
	<div class="absolute inset-12 border border-white/[0.02] rounded-full animate-[spin_40s_linear_infinite_reverse]"></div>

	<svg 
		viewBox="0 0 200 200" 
		class="w-full h-full transition-all duration-1000 origin-center relative z-10"
		class:rotate-12={active !== null}
	>
		<defs>
			<filter id="glow-sector" x="-50%" y="-50%" width="200%" height="200%">
				<feGaussianBlur stdDeviation="5" result="blur" />
				<feComposite in="SourceGraphic" in2="blur" operator="over" />
			</filter>
		</defs>

		{#each sectors as sector}
			<g 
				class="transition-all duration-500 cursor-pointer origin-center"
				class:opacity-20={active !== null && active !== sector.id}
				class:opacity-100={active === null || active === sector.id}
				onmouseenter={() => onHover(sector.id)}
				onmouseleave={() => onHover(null)}
			>
				<!-- Glassy Sector Background -->
				<path
					d={getPath(sector.start, sector.end)}
					fill={sector.color}
					fill-opacity={active === sector.id ? "0.2" : "0.05"}
					stroke={sector.color}
					stroke-width="0.5"
					stroke-opacity={active === sector.id ? "1" : "0.3"}
					class="transition-all duration-500"
					style="filter: {active === sector.id ? 'url(#glow-sector)' : 'none'};"
				/>
				
				<!-- Tech Text inside sector -->
				<text 
					x="100" 
					y="40" 
					fill={sector.color} 
					font-size="6" 
					font-weight="900" 
					text-anchor="middle"
					class="pointer-events-none tracking-[0.3em] font-mono transition-opacity duration-300"
					class:opacity-100={active === sector.id}
					class:opacity-30={active === null}
					transform="rotate({sector.start + 60} 100 100)"
				>
					{sector.label}
				</text>

				<!-- Corner technical marking on sector -->
				<circle 
					cx={polarToCartesian(100, 100, 80, sector.start + 5).x}
					cy={polarToCartesian(100, 100, 80, sector.start + 5).y}
					r="0.8"
					fill={sector.color}
					class="opacity-40"
				/>
			</g>
		{/each}

		<!-- Minimal Center Core -->
		<g class="z-20 opacity-40">
			<circle cx="100" cy="100" r="15" fill="none" stroke="white" stroke-width="0.2" stroke-dasharray="1 2" />
			<circle cx="100" cy="100" r="2" fill="white" class="animate-pulse" />
		</g>
	</svg>
</div>

<style>
	path {
		transform-box: fill-box;
		transform-origin: center;
	}
	text {
		font-family: 'JetBrains Mono', monospace;
	}
</style>
