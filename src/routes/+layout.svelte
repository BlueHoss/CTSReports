<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<main class="container mx-auto print:hidden">
	<a href="/" class="text-blue-500">v1</a>
	<a href="/v2" class="text-blue-500">v2</a>
	<a href="/v3" class="text-blue-500">v3</a>
</main>
{@render children()}
