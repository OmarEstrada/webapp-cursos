<script>
	import Loading from '$lib/common/Loading.svelte';
	import RutaRow from './RutaRow.svelte';
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';

	let loading = true;
	let rutas = [];
	let datosrutas = {
		id: null,
		nombre: '',
		Activo: '',
		Created_by: '',
		Created_at: '',
		Updated_by: '',
		Updated_at: ''
	};

	export let editNombre;
	export let editActivo;
	export let editCreated_by;
	export let editCreated_at;
	export let editUpdated_by;
	export let editUpdated_at;

	const cargarContenido = async () => {
		try {
			await wait(1000);

			const result = await fetch(`${API_URL}/Rutas`);

			if (result.ok) {
				rutas = await result.json();
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	onMount(cargarContenido);

	const deleteRuta = async ({ detail: { id } }) => {
		try {
			loading = true;

			const result = await fetch(`${API_URL}/Rutas/${id}`, {
				method: 'DELETE'
			});

			if (result.ok) {
				rutas = rutas.filter((ruta) => ruta.id != id);
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	const editRuta = async ({ detail: { id } }) => {
		const rutaAEditar = rutas.find((ruta) => ruta.id == id);

		if (!rutaAEditar) return;
		datosrutas = { ...rutaAEditar };
	};

	let agregarRuta = async () => {
		const nuevoruta = {
			id: datosrutas.id,
			nombre: datosrutas.nombre,
			Activo: datosrutas.Activo,
			Created_by: datosrutas.Created_by,
			Created_at: datosrutas.Created_at,
			Updated_by: datosrutas.Updated_by,
			Updated_at: datosrutas.Updated_at
		};
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Rutas`, {
				method: 'POST',
				body: JSON.stringify(nuevoruta),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				const nuevo = await result.json();

				rutas = [...rutas, nuevo];
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	let actualizarruta = async () => {
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Rutas/` + datosrutas.id, {
				method: 'PUT',
				body: JSON.stringify({
					editNombre,
					editActivo,
					editCreated_by,
					editCreated_at,
					editUpdated_by,
					editUpdated_at
				}),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				datosrutas.nombre = editNombre;
				datosrutas.Activo = editActivo;
				datosrutas.Created_by = editCreated_by;
				datosrutas.Created_at = editCreated_at;
				datosrutas.Updated_by = editUpdated_by;
				datosrutas.Updated_at = editUpdated_at;
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
	<div class="overflow-x-auto w-full">
		<table class="table w-full">
			<!-- head -->
			<thead>
				<tr>
					<th>Actions </th>
					<th>Nombre</th>
					<th>Activo</th>
					<th>Created_by</th>
					<th>Created_at</th>
					<th>Updated_by</th>
					<th>Updated_at</th>
				</tr>
				<tr>
					<th>
						<button class="btn btn-success" on:click|preventDefault={agregarRuta}
							>Agregar Ruta</button
						>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datosrutas.nombre}
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
								bind:value={datosrutas.Activo}
								required
								type="number"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">Activo</label>
						</div>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datosrutas.Created_by}
								required
								type="text"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">Created_by</label>
						</div>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datosrutas.Created_at}
								required
								type="text"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">Created_at</label>
						</div>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datosrutas.Updated_by}
								required
								type="text"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">Updated_by</label>
						</div>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datosrutas.Updated_at}
								required
								type="text"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">Updated_at</label>
						</div>
					</th>
				</tr>
			</thead>
			<tbody>
				{#each rutas as ruta}
					<RutaRow
						id={ruta.id}
						nombre={ruta.nombre}
						Activo={ruta.Activo}
						url={ruta.url}
						Created_by={ruta.Created_by}
						Created_at={ruta.Created_at}
						Updated_by={ruta.Updated_by}
						Updated_at={ruta.Updated_at}
						{loading}
						on:deleteClicked={deleteRuta}
						on:editarClicked={editRuta}
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
