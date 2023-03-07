<script lang="ts">
	import { goto } from '$app/navigation';
	import Modal from '$lib/common/Modal.svelte';
	import { stringify } from 'postcss';
	import { createEventDispatcher } from 'svelte';
	import { API_URL } from '../../core/constantes';
	const dispatch = createEventDispatcher();

	export let id: string;
	export let nombre: string;
	export let url: string;
	export let descripcion: string;

	let showingDeleteModal = false;
	let showingEditModal = true;

	async function clickEdit() {}
	function confirmDelete() {
		//todo: confirm action
		dispatch('deleteClicked', { id });
	}

	function saveChanges() {
		//Guardar Datos
		console.log(descripcion, nombre, url);
	}
</script>

<tr>
	<td>
		<button class="btn-primary" on:click={() => (showingEditModal = true)}>EDIT</button>
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
		<h3 class="font-bold text-lg">Eliminar Curso</h3>
		<p class="py-4">¿Está seguro que desea eliminar el curso?</p>
	</Modal>
{/if}

{#if showingEditModal}
	<Modal
		confirmBtnText="Save Changes"
		on:cancel={() => (showingEditModal = false)}
		on:confirm={saveChanges}
	>
		<h3 class="font-bold text-lg">Editar Curso</h3>
		<p class="py-4">
			<label for="name">Nombre:</label>
			<input type="text" bind:value={nombre} />
		</p>
		<p class="py-4">
			<label for="name">Descripción:</label>
			<input type="text" bind:value={descripcion} />
		</p>
		<p class="py-4">
			<label for="name">URL:</label>
			<input type="text" bind:value={url} />
		</p>
	</Modal>
{/if}

<style>
	a.titulo:hover {
		color: red;
	}
</style>
