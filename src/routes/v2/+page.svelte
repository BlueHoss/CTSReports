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
	import data from '../../data/indicators.json';
	import './app.report.css';

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
			<div class="justify-left flex items-center gap-2">
				<img src="/logo.png" alt="CTSdatabase Logo" class="h-auto w-[100px]" />
				<h1>CTSdatabase Match Report</h1>
			</div>
			<div class="justify-right flex items-center gap-2">
				Indication: {activeIndicator.indication}
			</div>
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
			<h2 class="section">Flags</h2>

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

	<!-- Protocol -->
	{#if activeIndicator.protocol}
		<h2 class="section">Protocol</h2>
		<div class="report-meta">
			{#each activeIndicator.protocol as proto}
				<div class="meta-item">
					<h3>{proto.label}</h3>
					<p>{proto.value}</p>
				</div>
			{/each}
		</div>
	{/if}

	<!-- Subject -->
	{#if activeIndicator.subject}
		<h2 class="section">Subject Details</h2>
		<div class="report-meta">
			{#each activeIndicator.subject as sub}
				<div class="meta-item">
					<h3>{sub.label}</h3>
					<p>{sub.value}</p>
				</div>
			{/each}
		</div>
	{/if}

	<!-- Subject -->
	{#if activeIndicator.match}
		<h2 class="section">{activeIndicator.match.title}</h2>
		<p>
			{activeIndicator.match.subtitle}
		</p>

		{#if activeIndicator.match.matches}
			<table class="w-full">
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
					{#each activeIndicator.match.matches as match}
						<tr>
							<td>{match.initialVisit}</td>
							<td>{match.indication}</td>
							<td>{match.lastStatus}</td>
							<td>{match.initials}</td>
							<td>{match.yob} ({match.age})</td>
							<td>{match.siteName}</td>
							<td>{match.siteZip}</td>
							<td>{match.sitePhone}</td>
						</tr>
					{/each}
				</tbody>
			</table>
		{/if}
	{/if}

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
