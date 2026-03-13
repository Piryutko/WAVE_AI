<script lang="ts">
	import { fade, fly } from 'svelte/transition';

	interface ProjectData {
		title: string;
		bullets: string[];
		color: string;
		id: string;
	}

	interface Props {
		project: ProjectData | null;
	}

	let { project }: Props = $props();
</script>

<div class="fixed right-12 top-1/2 -translate-y-1/2 w-80 pointer-events-none">
	{#if project}
		<div
			in:fly={{ x: 50, duration: 400, delay: 100 }}
			out:fade={{ duration: 200 }}
			class="p-8 rounded-2xl bg-black/40 backdrop-blur-xl border border-white/10 pointer-events-auto shadow-2xl"
		>
			<h2 class="text-3xl font-bold mb-6" style="color: {project.color}">
				{project.title}
			</h2>
			<ul class="space-y-4 mb-8">
				{#each project.bullets as bullet}
					<li class="flex items-start gap-3 text-gray-300">
						<span class="mt-2 w-1.5 h-1.5 rounded-full flex-shrink-0" style="background: {project.color}"></span>
						<span class="text-sm leading-relaxed">{bullet}</span>
					</li>
				{/each}
			</ul>
			<button
				class="w-full py-3 rounded-lg font-bold transition-all duration-300 hover:scale-105 active:scale-95"
				style="background: {project.color}; color: #000;"
			>
				Launch System
			</button>
		</div>
	{/if}
</div>
