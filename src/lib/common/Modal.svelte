<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let cancelBtnText = 'Cancel';
	export let confirmBtnText = 'Confirm';

	function cancel() {
		dispatch('cancel');
	}
	function confirm() {
		dispatch('confirm');
		cancel();
	}
</script>

<div class="modal modal-open">
	<div class="modal-box">
		<slot />
		<div class="modal-action">
			<button class="btn btn-primary" on:click={cancel}>{cancelBtnText}</button>
			<button class="btn btn-error" on:click={confirm}>{confirmBtnText}</button>
		</div>
	</div>
</div>

<style>
</style>
