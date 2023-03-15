<script>
	import Loading from '$lib/common/Loading.svelte';
	import PruebaRow from './PruebaRow.svelte';
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';

	let loading = true;
	let pruebas = [];
	let datospruebas = {
		id: null,
		nombre: '',
		url: '',
		descripcion: ''
	};

	export let editNombre;
	export let editDescripcion;
	export let editUrl;

	const cargarContenido = async () => {
		try {
			await wait(1000);

			const result = await fetch(`${API_URL}/Pruebas`);

			if (result.ok) {
				pruebas = await result.json();
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	onMount(cargarContenido);

	const deletePrueba = async ({ detail: { id } }) => {
		try {
			loading = true;

			const result = await fetch(`${API_URL}/Pruebas/${id}`, {
				method: 'DELETE'
			});

			if (result.ok) {
				pruebas = pruebas.filter((prueba) => prueba.id != id);
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	const editPrueba = async ({ detail: { id } }) => {
		const pruebaAEditar = pruebas.find((prueba) => prueba.id == id);

		if (!pruebaAEditar) return;
		datospruebas = { ...pruebaAEditar };
	};

	let agregarprueba = async () => {
		const nuevoprueba = {
			id: datospruebas.id,
			nombre: datospruebas.nombre,
			url: datospruebas.url,
			descripcion: datospruebas.descripcion
		};
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Pruebas`, {
				method: 'POST',
				body: JSON.stringify(nuevoprueba),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				const nuevo = await result.json();

				pruebas = [...pruebas, nuevo];
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	let actualizarprueba = async () => {
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Pruebas/` + datospruebas.id, {
				method: 'PUT',
				body: JSON.stringify({ editNombre, editDescripcion, editUrl }),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				datospruebas.nombre = editNombre;
				datospruebas.descripcion = editDescripcion;
				datospruebas.url = editUrl;
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};
</script>

{#if loading}
	<Loading />
{:else}
	<h1 style="color:yellow;">PRUEBAS</h1>
	<div class="overflow-x-auto w-full">
		<table class="table w-full">
			<!-- head -->
			<thead>
				<tr>
					<th> Actions </th>
					<th>Nombre</th>
					<th>Descripción</th>
					<th>Url</th>
				</tr>
				<tr>
					<th>
						<button class="btn btn-success" on:click|preventDefault={agregarprueba}
							>AgregarCurso</button
						>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datospruebas.nombre}
								required
								type="text"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">Nombre</label>
						</div>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datospruebas.descripcion}
								required
								type="checkbox"
								name="Ruta 1"
								class="box"
							/>Ruta 1
							<input
								bind:value={datospruebas.descripcion}
								required
								type="checkbox"
								name="Ruta 2"
								class="box"
							/>Ruta 2
							<label for="nome" />
						</div>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datospruebas.url}
								required
								type="number"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">url</label>
						</div>
					</th>
				</tr>
			</thead>
			<tbody>
				{#each pruebas as prueba}
					<PruebaRow
						id={prueba.id}
						nombre={prueba.nombre}
						descripcion={prueba.descripcion}
						url={prueba.url}
						{loading}
						on:deleteClicked={deletePrueba}
						on:editarClicked={editPrueba}
					/>
				{:else}
					<tr>
						<td colspan="4">
							<div class="text-center p-10">
								<p>No hay nada para mostrar</p>
							</div>
						</td>
					</tr>
				{/each}
			</tbody>
		</table>
	</div>
{/if}

<style>
	div.single-input {
		width: 100%;
		max-width: 30px 0;
		position: relative;
	}
	div.single-input label {
		font-size: 16px;
		position: absolute;
		left: 0;
		bottom: 10px;
		color: rgb(16, 54, 119);
		cursor: text;
		transition: 0.5s ease-in-out;
	}
	div.single-input .box {
		width: 25px;
		height: 25px;
	}
	div.single-input .input {
		width: 100%;
		padding: 5px;
		color: rgb(150, 150, 150);
		border: 0;
		border-bottom: 2px solid rgb(200, 200, 200);
		outline: 0;
		font-size: 16px;
	}
	div.single-input .input:focus,
	div.single-input .input:valid {
		border-bottom: 2px solid cornflowerblue;
	}
	div.single-input .input:focus ~ label,
	div.single-input .input:valid ~ label {
		transform: translateY(-24px);
		font-size: 12px;
		color: cornflowerblue;
	}
</style>
