<script>
	export let indicator;

	import {
		CircleX,
		Flag,
		FlagTriangleRight,
		ArrowRight,
		SquareCheck,
		CheckSquareIcon,
		Printer
	} from 'lucide-svelte';
	const iconMap = {
		met: SquareCheck,
		notmet: CircleX,
		investigate: Flag
	};
</script>

<div class="header-container">
	<div class="left-content">
		<div class="info-row">
			<span class="label">Print Date:</span>
			<span>{new Date().toLocaleDateString()}</span>
		</div>
		<div class="info-row">
			<span class="label">Visit Date:</span>
			<span>{indicator.visitDate}</span>
		</div>
	</div>
	<div class="right-content">
		<div class="info-row">
			<span class="label">Site:</span>
			<span>{indicator.site}</span>
		</div>
		<div class="info-row">
			<span class="label">Protocol:</span>
			<span>{indicator.protocol}</span>
		</div>
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
		{#if indicator.icon === 'met'}
			<span>The participant has met the protocol specifications.</span>
		{:else}
			<span>The following eligibility criteria have been flagged: </span>
		{/if}
	</div>
	<div class="notes">
		{#if indicator.flags && indicator.flags.length > 0}
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

<h2 class="section text-gray-800">Subject Identifiers</h2>
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
		<h3>Sex</h3>
		<p>{indicator.sex}</p>
	</div>
</div>
<br />
