<script lang="ts">
	import { browser } from '$app/environment';
	import * as d3 from 'd3';
	import LinePlot from './components/line-plot.svelte';

	let data = d3.ticks(-2, 2, 200).map(Math.sin);

	function onMousemove(event: any) {
		const [x, y] = d3.pointer(event);
		data = data.slice(-200).concat(Math.atan2(x, y));
	}

	console.log();
</script>

<div class="border-black flex h-screen w-screen flex-grow flex-col items-center justify-center">
	<h1 class="text-4xl font-light">The Start of Something Data Rich</h1>
	<p class="text-lgt-con-sec dark:text-drk-con-sec">A boilerplate for your next d3 sideproject</p>

	{#if browser}
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<div on:mousemove={onMousemove}>
			<LinePlot {data} />
		</div>

		<div
			class="
		gap-4 bg-lgt-base
		p-4 text-sm
		text-lgt-con transition-colors duration-300
		dark:bg-drk-base dark:text-drk-con
		"
		>
			<button
				class="rounded-sm border border-lgt-con px-2 py-1 dark:border-drk-con"
				on:click={() => {
					document.documentElement.classList.toggle('dark');
					if (document.documentElement.classList.contains('dark')) {
						localStorage.theme = 'dark';
					} else {
						localStorage.theme = 'light';
					}
				}}
			>
				Toggle Theme
			</button>

			<button
				class="rounded-sm border border-lgt-con px-2 py-1 dark:border-drk-con"
				on:click={() => {
					// Whenever the user explicitly chooses to respect the OS preference
					localStorage.removeItem('theme');
					document.documentElement.classList.remove('dark');
					if (
						localStorage.theme === 'dark' ||
						(!('theme' in localStorage) &&
							window.matchMedia('(prefers-color-scheme: dark)').matches)
					) {
						document.documentElement.classList.add('dark');
					} else {
						document.documentElement.classList.remove('dark');
					}
				}}
			>
				System pref
			</button>
		</div>
		<span class="flex flex-row justify-center items-center gap-4">
			<a href="/debug"> Features </a>
			<a href="https://github.com/RobiMez/skit-d3"> Repo </a>
		</span>
	{/if}
</div>
