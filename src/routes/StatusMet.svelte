<script lang="ts">
	import { SquareCheck } from 'lucide-svelte';
	export let indicator;
</script>

<div
	class="mb-4 flex items-center gap-2 rounded-md bg-green-400 p-2 text-white print:rounded-md print:border-2 print:border-solid print:border-gray-400 print:bg-gray-400 print:text-gray-600"
>
	<div class="justify-left flex items-center gap-2">
		<SquareCheck size={24} />
		{indicator.status}
	</div>
</div>

<!-- <p class="center mb-4 text-center text-lg text-gray-600">
	Subject meets CTS screening criteria. Proceed with screening.
</p> -->
