<script lang="ts">
	import { goto } from '$app/navigation';
	import { createEventDispatcher } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import Modal from '$lib/common/Modal.svelte';
	const dispatch = createEventDispatcher();

	export let id: string;
	export let nombre: string;
	export let url: string;
	export let descripcion: string;

	let showingDeleteModal = false;

	async function clickEdit() {
		dispatch('editarClicked', { id });
	}
	function confirmDelete() {
		//todo: confirm action
		dispatch('deleteClicked', { id });
	}
</script>

<tr>
	<td>
		<button class="btn btn-primary" on:click={clickEdit}>EDIT</button>
		<button class="btn btn-error" on:click={() => (showingDeleteModal = true)}>DELETE</button>
	</td>
	<td>
		<div class="flex items-center space-x-3">
			<div class="avatar">
				<div class="mask mask-square w-12 h-12">
					<img src="/images/curso-online.png" alt={nombre} />
				</div>
			</div>
			<div>
				<div class="font-bold">
					<a href={url} target="_blank" class="titulo" rel="noreferrer">{nombre}</a>
				</div>
				<div class="text-sm opacity-50" />
			</div>
		</div>
	</td>
	<td>
		{descripcion}
	</td>
	<td>
		{url}
	</td>
</tr>

{#if showingDeleteModal}
	<Modal
		confirmBtnText="Delete!"
		on:cancel={() => (showingDeleteModal = false)}
		on:confirm={confirmDelete}
	>
		<h3 class="font-bold text-lg">Elimar Curso</h3>
		<p class="py-4">¿Está seguro que desea eliminar el curso?</p>
	</Modal>
{/if}

<style>
	a.titulo:hover {
		color: red;
	}
</style>
