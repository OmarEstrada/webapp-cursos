<script lang="ts">
	import Ruta from './Ruta.svelte';
	import { goto } from '$app/navigation';
	import Modal from '$lib/common/Modal.svelte';
	import { stringify } from 'postcss';
	import { createEventDispatcher } from 'svelte';
	import { API_URL } from '../../core/constantes';
	const dispatch = createEventDispatcher();

	export let id: string;
	export let url: string;
	export let nombre: string;
	export let Created_by: string;
	export let Created_at: string;
	export let Updated_by: string;
	export let Updated_at: string;
	export let loading: any;
	export let ruta1 = false;
	export let ruta2 = false;

	let showingDeleteModal = false;
	let showingEditModal = false;

	let editNombre = '';
	let editRuta1 = false;
	let editRuta2 = false;
	let editCreated_by = '';
	let editCreated_at = '';
	let editUpdated_by = '';
	let editUpdated_at = '';

	async function clickEdit() {}
	function confirmDelete() {
		//todo: confirm action
		dispatch('deleteClicked', { id });
	}

	const saveChanges = async () => {
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Rutas/` + id, {
				method: 'PUT',
				body: JSON.stringify({
					nombre: editNombre,
					ruta1: editRuta1,
					ruta2: editRuta2,
					Created_by: editCreated_by,
					Created_at: editCreated_at,
					Updated_by: editUpdated_by,
					Updated_at: editUpdated_at
				}),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				nombre = editNombre;
				ruta1 = editRuta1;
				ruta2 = editRuta2;
				Created_by = editCreated_by;
				Created_at = editCreated_at;
				Updated_by = editUpdated_by;
				Updated_at = editUpdated_at;
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	const editStart = () => {
		editNombre = nombre;
		editRuta1 = ruta1;
		editRuta2 = ruta2;
		editCreated_by = Created_by;
		editCreated_at = Created_at;
		editUpdated_by = Updated_by;
		editUpdated_at = Updated_at;
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
		<p>Ruta1: {ruta1}</p>
		<p>Ruta2: {ruta2}</p>
	</td>
	<td>
		{Created_by}
	</td>
	<td>
		{Created_at}
	</td>
	<td>
		{Updated_by}
	</td>
	<td>
		{Updated_at}
	</td>
</tr>

{#if showingDeleteModal}
	<Modal
		confirmBtnText="Delete!"
		on:cancel={() => (showingDeleteModal = false)}
		on:confirm={confirmDelete}
	>
		<h3 class="font-bold text-lg">Eliminar Ruta</h3>
		<p class="py-4">¿Está seguro que desea eliminar esta ruta?</p>
	</Modal>
{/if}

{#if showingEditModal}
	<Modal
		confirmBtnText="Save Changes"
		on:cancel={() => (showingEditModal = false)}
		on:confirm={saveChanges}
	>
		<h3 class="font-bold text-lg">Editar Ruta</h3>
		<tr>
			<td>
				<label class="espacio" for="name">Nombre:</label>
			</td>
			<td>
				<input type="text" bind:value={editNombre} />
			</td>
		</tr>
		<tr>
			<td>
				<label class="espacio" for="name">Activo:</label>
			</td>
			<td>
				<input bind:checked={editRuta1} required type="checkbox" name="Ruta 1" class="box" />Ruta 1
			</td>
			<td>
				<input bind:checked={editRuta2} required type="checkbox" name="Ruta 1" class="box" />Ruta 2
			</td>
		</tr>
	</Modal>
{/if}
