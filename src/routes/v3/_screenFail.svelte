<script>
	import {
		AlertDialog,
		AlertDialogAction,
		AlertDialogCancel,
		AlertDialogContent,
		AlertDialogDescription,
		AlertDialogFooter,
		AlertDialogHeader,
		AlertDialogTitle,
		AlertDialogTrigger
	} from '$lib/components/ui/alert-dialog';
	import { Button } from '$lib/components/ui/button';

	export let open = false;
	export let onYes = () => {};
	export let onNo = () => {};
	export let onOpenChange = (open) => {};
</script>

<AlertDialog {open} {onOpenChange}>
	<!-- <AlertDialogTrigger asChil</AlertDialogTrigger>d>
		<Button variant="outline">Screen Fail Subject</Button>
	</AlertDialogTrigger> -->
	<AlertDialogContent class="sm:max-w-md">
		<AlertDialogHeader>
			<AlertDialogTitle>Are you screen failing this subject?</AlertDialogTitle>
		</AlertDialogHeader>
		<AlertDialogFooter class="sm:justify-start">
			<AlertDialogAction on:click={onYes} class="bg-red-600 hover:bg-red-700">Yes</AlertDialogAction
			>
			<AlertDialogCancel on:click={onNo}>No</AlertDialogCancel>
		</AlertDialogFooter>
	</AlertDialogContent>
</AlertDialog>
