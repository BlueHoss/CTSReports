<script lang="ts">
	import {
		SquareCheck,
		Flag,
		CircleX,
		CheckSquareIcon,
		FlagTriangleRight,
		ArrowLeft,
		HeartPulse
	} from 'lucide-svelte';
	import data from './indicators3.json';
	import './app.report.css';
	import Report from './Report.svelte';

	let activeIndicator: any = null;
	let groupedIndicators = {};

	let activeGroup: string | null = 'notmet';

	// Group the indicators by icon
	groupedIndicators = data.indicators.reduce((acc, indicator) => {
		const iconType = indicator.icon;
		if (!acc[iconType]) {
			acc[iconType] = [];
		}
		acc[iconType].push({
			id: indicator.id,
			title: indicator.title
		});
		return acc;
	}, {});

	function setGroup(group) {
		activeGroup = group;
		activeIndicator = null; // Reset active indicator when changing groups
	}

	function setIndicator(id) {
		activeIndicator = data.indicators.find((ind) => ind.id === id);
	}

	// Map icon names to components
	const iconMap = {
		met: SquareCheck,
		notmet: CircleX,
		investigate: Flag
	};
</script>

<div class="flex flex-col print:hidden">
	<!-- Icon buttons -->
	<div class="mb-4 flex items-center justify-center gap-6 bg-gray-100 p-4">
		<button
			class="rounded-lg p-2 transition-colors duration-200 {activeGroup === 'notmet'
				? 'bg-blue-500 text-white'
				: 'hover:bg-gray-200'}"
			on:click={() => setGroup('notmet')}
		>
			<CircleX color="red" size={24} />
		</button>
		<button
			class="rounded-lg p-2 transition-colors duration-200 {activeGroup === 'investigate'
				? 'bg-blue-500 text-white'
				: 'hover:bg-gray-200'}"
			on:click={() => setGroup('investigate')}
		>
			<Flag size={24} />
		</button>
		<button
			class="rounded-lg p-2 transition-colors duration-200 {activeGroup === 'met'
				? 'bg-blue-500 text-white'
				: 'hover:bg-gray-200'}"
			on:click={() => setGroup('met')}
		>
			<SquareCheck size={24} />
		</button>
		<a
			href="/"
			class="justify-left flex items-center rounded-lg p-2 transition-colors duration-200 hover:bg-gray-200"
			rel="external"
		>
			V1 <ArrowLeft size={24} />
		</a>
	</div>

	<!-- Indicator selection when group is active -->
	{#if activeGroup && groupedIndicators[activeGroup]}
		<div class="flex flex-col space-y-2 p-4">
			<h3 class="mb-2 text-lg font-semibold">Select an indicator: {activeGroup}</h3>
			{#each groupedIndicators[activeGroup] as indicator}
				<button
					class="rounded-lg px-4 py-2 text-left transition-colors duration-200
                        {activeIndicator?.id === indicator.id
						? 'bg-blue-100 text-blue-700'
						: 'hover:bg-gray-100'}"
					on:click={() => setIndicator(indicator.id)}
				>
					{indicator.title}
				</button>
			{/each}
		</div>
	{/if}
</div>
<div class="print:hidden">
	<hr />
	<hr />
</div>

{#if activeIndicator}
	<Report indicator={activeIndicator} />
{:else}
	<hr />
	<h1>Select an report</h1>
{/if}
