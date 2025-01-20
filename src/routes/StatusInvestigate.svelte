<script lang="ts">
	import { Flag } from 'lucide-svelte';
	export let indicator;
</script>

<div
	class="mb-4 flex items-center gap-2 rounded-md bg-yellow-400 p-2 text-white print:rounded-md print:border-2 print:border-solid print:border-gray-400 print:bg-gray-400 print:text-gray-600"
>
	<div class="justify-left flex items-center gap-2">
		<Flag size={24} />
		{indicator.status}
	</div>
</div>
<!-- {#if !indicator.investigationStatus}
	<p class="mb-4 text-gray-600"></p>
{:else if indicator.investigationStatus === 'Site'}
	<p class="mb-4 text-center text-lg text-gray-600">
		Site investigation required. [what needs to be investigated by the site?]
	</p>
{:else if indicator.investigationStatus === 'CTS'}
	<p class="mb-4 text-center text-lg text-gray-600">
		CTS investigation required. You will receive a reply back very soon.*
	</p>
{/if} -->
