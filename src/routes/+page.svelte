<script lang="ts">
	import { browser } from '$app/environment';
	import { FY2021 as satisfactionData2021 } from '$lib/data/satisfaction.json';
	import '$lib/styles/global.css';
	import '@fontsource/merriweather';
	import { Chart, registerables } from 'chart.js';
	import { onMount } from 'svelte';

	Chart.register(...registerables);

	let barChartElement: HTMLCanvasElement;

	const chartData = {
		labels: satisfactionData2021.map(({ framework }) => framework),
		datasets: [
			{
				label: 'Satisfaction (%)',
				data: satisfactionData2021.map(({ score }) => score),
				backgroundColor: [
					'hsl(347 38% 49%)',
					'hsl(346 65% 63%)',
					'hsl(346 49% 56%)',
					'hsl(346 89% 70%)',
					'hsl(346 90% 76%)',
					'hsl(346 90% 73%)',
					'hsl(346 89% 79%)',
					'hsl(346 89% 85%)',
					'hsl(347 89% 82%)',
					'hsl(346 90% 88%)',
					'hsl(347 87% 94%)',
					'hsl(347 91% 91%)',
					'hsl(346 87% 97%)',
				],
				borderColor: ['hsl(43 100% 52%)'],
				borderRadius: 4,
				borderWidth: 2,
			},
		],
	};

	onMount(() => {
		if (browser) {
			new Chart(barChartElement, {
				type: 'bar',
				data: chartData,
				plugins: [
					{
						id: 'custom_canvas_background_colour',
						beforeDraw: (chart: Chart) => {
							const ctx = chart.canvas.getContext('2d');
							if (ctx) {
								ctx.save();
								ctx.globalCompositeOperation = 'destination-over';
								ctx.fillStyle = 'hsl(13 46% 25%)';
								ctx.fillRect(0, 0, chart.width, chart.height);
								ctx.restore();
							}
						},
					},
				],
				options: {
					plugins: {
						legend: {
							display: false,
						},
					},
					scales: {
						x: {
							grid: {
								color: 'hsl(43 100% 52% / 10%)',
							},
							ticks: { color: 'hsl(43 100% 52% )' },
						},
						y: {
							beginAtZero: false,
							ticks: { color: 'hsl(43 100% 52% )', font: { size: 18 } },
							grid: {
								color: 'hsl(43 100% 52% / 40%)',
							},
							title: {
								display: true,
								text: 'Satisfaction (%)',
								color: 'hsl(43 100% 52% )',
								font: { size: 24, family: 'Merriweather' },
							},
						},
					},
				},
			});
		}
	});
</script>

<main class="main-container">
	<h1>State of JS 2021 Backend Framework Satisfaction</h1>
	<section>
		<canvas bind:this={barChartElement}></canvas>
	</section>
</main>

<style>
	.main-container {
		width: min(100% - var(--spacing-12), var(--max-width-wrapper));
		margin: var(--spacing-18) auto;
	}
</style>
