<script lang="ts">
	import Loading from './../common/Loading.svelte';
	import { goto } from '$app/navigation';
	import { createEventDispatcher } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import Modal from '$lib/common/Modal.svelte';
	const dispatch = createEventDispatcher();

	export let id: string;
	export let nombre: string;
	export let url: string;
	export let email: string;
	export let provider: string;
	export let password: string;
	export let loading: any;

	let showingDeleteModal = false;
	let showingEditModal = false;

	let editNombre = '';
	let editemail = '';
	let editprovider = '';
	let editpassword = '';

	async function clickEdit() {}
	function confirmDelete() {
		//todo: confirm action
		dispatch('deleteClicked', { id });
	}

	const saveChanges = async () => {
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Usuarios/` + id, {
				method: 'PUT',
				body: JSON.stringify({
					nombre: editNombre,
					email: editemail,
					provider: editprovider,
					password: editpassword
				}),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				nombre = editNombre;
				email = editemail;
				provider = editprovider;
				password = editpassword;
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	const editStart = () => {
		editNombre = nombre;
		editemail = email;
		editprovider = provider;
		editpassword = password;
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
		{email}
	</td>
	<td>
		{provider}
	</td>
	<td>
		{password}
	</td>
</tr>

{#if showingDeleteModal}
	<Modal
		confirmBtnText="Delete!"
		on:cancel={() => (showingDeleteModal = false)}
		on:confirm={confirmDelete}
	>
		<h3 class="font-bold text-lg">Eliminar Usuario</h3>
		<p class="py-4">¿Está seguro que desea eliminar al usuario?</p>
	</Modal>
{/if}

{#if showingEditModal}
	<Modal
		confirmBtnText="Save Changes"
		on:cancel={() => (showingEditModal = false)}
		on:confirm={saveChanges}
	>
		<h3 class="font-bold text-lg">Editar Usuarios</h3>
		<p class="py-4">
			<label for="name">Nombre:</label>
			<input type="text" bind:value={editNombre} />
		</p>
		<p class="py-4">
			<label for="name">email:</label>
			<input type="text" bind:value={editemail} />
		</p>
		<p class="py-4">
			<label for="name">Provider:</label>
			<input type="text" bind:value={editprovider} />
		</p>
		<p class="py-4">
			<label for="name">Password:</label>
			<input type="text" bind:value={editpassword} />
		</p>
	</Modal>
{/if}

<style>
	a.titulo:hover {
		color: red;
	}
</style>
