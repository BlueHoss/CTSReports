<script lang="ts">
	import {
		CircleX,
		Flag,
		FlagTriangleRight,
		ArrowRight,
		SquareCheck,
		CheckSquareIcon
	} from 'lucide-svelte';

	const iconMap = {
		met: SquareCheck,
		notmet: CircleX,
		investigate: Flag
	};

	export let indicator: {
		id: string;
		icon: string;
		color: string;
		title: string;
		status: string;
		statusDescription: string;
		subjectId: string;
		yearOfBirth: string;
		age: string;
		sex: string;
		initials: string;
		visitDate: string;
		site: string;
		protocol: string;
		flags?: [
			{
				color: string;
				header: string;
			}
		];
		nextSteps?: [
			{
				id: string;
				text: string;
			}
		];
	};
</script>

{#if indicator}
	<div class="mx-auto max-w-[950px] space-y-6 p-8 text-gray-900">
		<!-- Header -->
		<div class="header-status flex items-center justify-between pb-4">
			<div class="flex items-center gap-4">
				<img src="/logo.png" alt="CTSdatabase Logo" class="h-auto w-[100px]" />
				<h1 class="text-2xl font-semibold text-gray-800">Match Report</h1>
			</div>
			<div class="text-sm text-gray-600">
				<ul>
					<li><strong>Print Date:</strong> {new Date().toLocaleDateString()}</li>
					<li><strong>Visit Date:</strong> {indicator.visitDate}</li>
					<li><strong>Site:</strong> {indicator.site}</li>
					<li><strong>Protocol:</strong> {indicator.protocol}</li>
				</ul>
			</div>
		</div>

		<!-- Status Banner 2 -->
		<div class="header-status">
			<div class="status-banner {indicator.color}">
				<div class="justify-left flex items-center gap-2">
					<svelte:component this={iconMap[indicator.icon]} size={24} />
					{indicator.status}
				</div>
			</div>

			<div class="alert-description">
				{#if indicator.icon === 'notmet'}
					<span>The participant has not met the protocol specifications.</span>
				{:else if indicator.icon === 'met'}
					<span>The participant has met the protocol specifications.</span>
				{:else}
					<span
						>An investigation is required regarding this participants elegibility for this study.
					</span>
				{/if}
			</div>
			<div class="notes">
				{#if indicator.flags && indicator.flags.length > 0}
					<span> The following elegibility criteria has been flagged:</span>
					<div class="space-y-2">
						{#each indicator.flags as flag}
							<div class="mt-5 flex items-center gap-2">
								<FlagTriangleRight color={flag.color} />
								<span>{flag.header}</span>
							</div>
						{/each}
					</div>
				{/if}

				{#if indicator.nextSteps && indicator.nextSteps.length > 0}
					<div class="mt-5">Next Steps:</div>
					<div class="space-y-2">
						{#each indicator.nextSteps as step}
							<div class="flex items-start gap-2">
								<CheckSquareIcon />
								<p>{step.text}</p>
							</div>
						{/each}
					</div>
				{/if}
			</div>
		</div>

		<!-- <h2 class="section">Protocol</h2> -->
		<!-- <div class="report-meta">
			<div class="meta-item">
				<h3>Visit Date</h3>
				<p>{indicator.visitDate}</p>
			</div>
			<div class="meta-item">
				<h3>Site</h3>
				<p>{indicator.site}</p>
			</div>
			<div class="meta-item">
				<h3>Protocol</h3>
				<p>{indicator.protocol}</p>
			</div>
		</div> -->

		<h2 class="section">Subject</h2>
		<div class="report-meta">
			<div class="meta-item">
				<h3>Subject Number</h3>
				<p>{indicator.subjectId}</p>
			</div>
			<div class="meta-item">
				<h3>Initials</h3>
				<p>{indicator.initials}</p>
			</div>
			<div class="meta-item">
				<h3>Year of Birth (age)</h3>
				<p>{indicator.yearOfBirth} ({indicator.age})</p>
			</div>
			<div class="meta-item">
				<h3>Sex)</h3>
				<p>{indicator.sex}</p>
			</div>
		</div>

		<img src="/certainmatches.png" alt="Virtual Certain" class="w-full" />

		<img src="/probablematches.png" alt="Probable Matches" class="w-full" />

		<!-- Footer -->
		<div class="space-y-2 border-t border-gray-200 pt-4 text-sm text-gray-500">
			<p>PS = Prescreen, EOT = End Of Treatment</p>
			<p>
				If any of above information is incorrect, please contact us at
				<a href="mailto:support@ctsdatabase.com" class="text-blue-600"> support@ctsdatabase.com </a>
				or 1-855 CTS-CTSd(1-855-287-2873)
			</p>
			<p class="text-xs">
				Disclaimer: These are statistical estimates only. It is possible that a subject may be a
				database match and yet not have participated in the studies described above.
			</p>
			<p class="text-xs">All dates and times are in GMT</p>
			<p class="text-xs">Printed On: {new Date().toLocaleString()}</p>
		</div>
	</div>
{/if}

<style>
	.status-summary {
		padding: 0.5rem;
		margin: 0.5rem 0;
	}

	.report-meta {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(130px, 0.5fr));
		margin: 0.25rem 0;
	}

	.meta-item {
		padding: 0.25rem;
		border: 1px solid #dee2e6;
		border-radius: 10px;
	}

	.meta-item h3 {
		font-size: 9pt;
		font-weight: bold;
	}

	.meta-item p {
		font-size: 9pt;
	}

	.header-status {
		padding: 0.25rem 0.75rem;
		font-size: 10pt;
		border-width: 2px;
		border-radius: 10px;
	}

	/* Print-specific refinements */
	@media print {
		.status-banner {
			padding: 0.25rem 0.75rem;
			font-size: 10pt;
			border-width: 1px;
		}
		.status-banner.approved {
			color: black;
			background-color: lightgray;
		}

		.status-banner.denied {
			color: black;
			background-color: lightgray;
		}

		.status-banner.investigate {
			color: black;
			background-color: lightgray;
		}
	}
</style>
