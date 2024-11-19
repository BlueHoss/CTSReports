<script lang="ts">
	import { Flag } from 'lucide-svelte';
	export let status: string;
</script>

<div
	class="mb-4 flex items-center gap-2 rounded-md bg-yellow-400 p-2 text-white print:rounded-md print:border-2 print:border-solid print:border-gray-400 print:bg-gray-400 print:text-gray-600"
>
	<div class="justify-left flex items-center gap-2">
		<Flag size={24} />
		{status}
	</div>
</div>
