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
	export let loading: any;

	let showingDeleteModal = false;
	let showingEditModal = false;

	let editNombre = '';
	let editDescripcion = '';
	let editUrl = '';

	async function clickEdit() {}
	function confirmDelete() {
		//todo: confirm action
		dispatch('deleteClicked', { id });
	}

	const saveChanges = async () => {
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Cursos/` + id, {
				method: 'PUT',
				body: JSON.stringify({ editNombre, editDescripcion, editUrl }),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				nombre = editNombre;
				descripcion = editDescripcion;
				url = editUrl;
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	const editStart = () => {
		editNombre = nombre;
		editDescripcion = descripcion;
		editUrl = url;
		showingEditModal = true;
	};
</script>

<tr>
	<td>
		<button class="btn btn-primary" on:click={editStart}>EDIT</button>
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
			<input type="text" bind:value={editNombre} />
		</p>
		<p class="py-4">
			<label for="name">Descripción:</label>
			<input type="text" bind:value={editDescripcion} />
		</p>
		<p class="py-4">
			<label for="name">URL:</label>
			<input type="text" bind:value={editUrl} />
		</p>
	</Modal>
{/if}
