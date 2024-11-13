<script>
	import { SquareCheck, Flag, CircleX, CheckSquareIcon, FlagTriangleRight } from 'lucide-svelte';
	import data from '../data/indicators.json';

	let activeGroup = null;
	let activeIndicator = null;
	let groupedIndicators = {};

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

	const flagIconMap = {
		critical: SquareCheck,
		warning: CircleX,
		noflag: Flag
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
	<div class="header">
		<div class="justify-left flex items-center gap-2">
			<img src="/logo.png" alt="CTSdatabase Logo" class="h-auto w-[100px]" />
			<h1>CTSdatabase Match Report</h1>
		</div>
		<!-- <div class="status-banner denied"> -->
		<div class={activeIndicator.color}>
			<div class="justify-left flex items-center gap-2">
				<svelte:component this={iconMap[activeIndicator.icon]} size={24} />
				{activeIndicator.status.title}
			</div>
		</div>
	</div>

	<div class="status-summary approved">
		<h2>{activeIndicator.title}</h2>
		<p>
			{activeIndicator.status.text}
		</p>
		<p>Status determined on: {activeIndicator.status.date}</p>
	</div>

	<!-- Flags -->
	{#if activeIndicator.flags}
		<div class="flags-section">
			<h2>Flags</h2>

			{#each activeIndicator.flags as flag}
				<div class="justify-left flex items-center gap-2">
					<FlagTriangleRight color={flag.color} size={24} />
					<p>{flag.header}</p>
				</div>
			{/each}
		</div>
	{/if}

	<!-- Next Steps -->
	{#if activeIndicator.nextSteps}
		<div class="flags-section">
			<h2>Next Steps</h2>

			{#each activeIndicator.nextSteps as step}
				<div class="justify-left flex items-center gap-2">
					<CheckSquareIcon size={24} />
					<p>{step.text}</p>
				</div>
			{/each}
		</div>
	{/if}

	<h2>Protocol</h2>
	<div class="report-meta">
		<div class="meta-item">
			<h3>Date</h3>
			<p>November 8, 2024</p>
		</div>
		<div class="meta-item">
			<h3>Site</h3>
			<p>A-Shine</p>
		</div>
		<div class="meta-item">
			<h3>Protocol</h3>
			<p>NMRA-335140-303</p>
		</div>
	</div>

	<h2>Subject Details</h2>
	<div class="report-meta">
		<div class="meta-item">
			<h3>Subject Number</h3>
			<p>000-000-0006</p>
		</div>
		<div class="meta-item">
			<h3>Initials</h3>
			<p>BBB</p>
		</div>
		<div class="meta-item">
			<h3>Year of Birth (Age)</h3>
			<p>2019 (5)</p>
		</div>
		<div class="meta-item">
			<h3>Sex</h3>
			<p>M</p>
		</div>
	</div>

	<h2>Probable Matches</h2>
	<p>
		Identifiers matched closely enough that the odds are less than 1 in 1 million to occur by chance
	</p>

	<table>
		<thead>
			<tr>
				<th>Initial Visit</th>
				<th>Indication</th>
				<th>Last Status</th>
				<th>Initials</th>
				<th>YOB (Age)</th>
				<th>Site Name</th>
				<th>Site Zip</th>
				<th>Site Phone</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td>07-Nov-2024</td>
				<td>Major Depression</td>
				<td>Approved</td>
				<td>BBB</td>
				<td>2000 (24)</td>
				<td>A-Shine</td>
				<td>312 00</td>
				<td>+420 777242101</td>
			</tr>
		</tbody>
	</table>

	<div class="alert">
		* Any statuses that are blank have not yet been reported to CTSdatabase. You may need to call
		this site for more information.
	</div>
	<div class="footer">
		<p>PS = Prescreen, EOT = End Of Treatment</p>
		<p>
			If any of above information is incorrect, please contact us at <a
				href="mailto:support@ctsdatabase.com">support@ctsdatabase.com</a
			> or 1-855 CTS-CTSd(1-855-287-2873)
		</p>
		<p>
			Disclaimer: These are statistical estimates only. It is possible that a subject may be a
			database match and yet not have participated in the studies described above.
		</p>
		<p>All dates and times are in GMT</p>
		<p>Printed On: November 10, 2024 05:38:01 PM</p>
	</div>
{/if}

<style>
	/* Base styles with reduced spacing */

	.header {
		border-bottom: 2px solid #2c3e50;
		background: none;
		padding: 0.75rem 0;
		margin-bottom: 1rem;
		display: flex;

		flex-direction: column;
	}

	/* Status banner refinements */
	.status-banner {
		padding: 0.5rem 1rem;
		margin: 0.5rem 0;
		border: 1px solid;
		box-shadow: none;
		font-size: 1rem;
	}

	.status-banner.approved {
		background-color: #e8f5e9;
		border-color: #28a745;
		color: #1e7e34;
	}

	.status-banner.denied {
		background-color: #dc3545;
		color: white;
	}

	.status-banner.investigate {
		background-color: #ffc107;
		color: black;
	}
	.status-icon {
		width: 18px;
		height: 18px;
	}

	/* Status summary refinements */
	.status-summary {
		padding: 0.75rem;
		margin: 1rem 0;
		background-color: #f8f9fa;
		border: 1px solid #dee2e6;
	}

	.status-summary h2 {
		font-size: 1.1rem;
		margin: 0 0 0.5rem 0;
	}

	.status-summary p {
		margin: 0.25rem 0;
		font-size: 0.9rem;
	}

	/* Flags section refinements */
	.flags-section {
		margin: 1rem 0;
		padding: 0;
	}

	.flags-section h2 {
		font-size: 1.1rem;
		margin: 0 0 0.5rem 0;
		padding-bottom: 0.25rem;
		border-bottom: 1px solid #dee2e6;
	}

	.flag {
		display: flex;
		align-items: center;
		padding: 0.5rem;
		margin-bottom: 0.5rem;
		border-radius: 4px;
		font-size: 0.9rem;
		font-weight: bold;
	}

	.flag-icon {
		margin-right: 0.5rem;
		width: 16px;
		height: 16px;
		flex-shrink: 0;
	}

	/* Meta items refinements */
	.report-meta {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
		gap: 0.5rem;
		margin: 0.75rem 0;
	}

	.meta-item {
		padding: 0.5rem;
		background-color: #f8f9fa;
	}

	.meta-item h3 {
		font-size: 0.75rem;
		margin-bottom: 0.25rem;
	}

	.meta-item p {
		font-size: 0.9rem;
		margin: 0;
	}

	/* Alert refinements */
	.alert {
		padding: 0.5rem;
		margin: 0.75rem 0;
		font-size: 0.85rem;
		background-color: #fff3cd;
		border: 1px solid #ffeeba;
	}

	/* Footer refinements */
	.footer {
		margin-top: 1.5rem;
		padding-top: 0.75rem;
		font-size: 0.8rem;
		color: #666;
	}
</style>
