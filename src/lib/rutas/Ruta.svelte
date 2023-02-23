<script>
	import Loading from '$lib/common/Loading.svelte';
	import RutaRow from './RutaRow.svelte';
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';

	let loading = true;
	let rutas = [];
	let activado = true;
	let datosrutas = {
		id: null,
		nombre: '',
		url: '',
		descripcion: '',
		activo: '',
		Created_by: '',
		Created_at: '',
		Updated_by: '',
		Updated_at: ''
	};

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
			descripcion: datosrutas.descripcion,
			activo: datosrutas.activo,
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
				body: JSON.stringify(datosrutas),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				rutas = rutas.map((ruta) => {
					if (ruta.id != datosrutas.id) {
						return ruta;
					}

					return datosrutas;
				});
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
					<th> Actions </th>
					<th>Nombre</th>
					<th>Descripción</th>
					<th>Activo</th>
					<th>Created_by</th>
					<th>Created_at</th>
					<th>Updated_by</th>
					<th>Updated_at</th>
				</tr>
				<tr>
					<th>
						<button class="btn btn-success" on:click|preventDefault={agregarRuta}
							>agregarRuta</button
						>
					</th>
					<th>
						<input
							bind:value={datosrutas.nombre}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<input
							bind:value={datosrutas.descripcion}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<input
							bind:value={datosrutas.url}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<input
							bind:value={datosrutas.activo}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<input
							bind:value={datosrutas.Created_by}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<input
							bind:value={datosrutas.Created_at}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<input
							bind:value={datosrutas.Updated_by}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<input
							bind:value={datosrutas.Updated_at}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<button class="btn btn-success" on:click|preventDefault={actualizarruta}
							>Actualizar</button
						>
					</th>
				</tr>
			</thead>
			<tbody>
				{#each rutas as ruta}
					<RutaRow
						id={ruta.id}
						nombre={ruta.nombre}
						descripcion={ruta.descripcion}
						url={ruta.url}
						activo={ruta.activo}
						Created_by={ruta.Created_by}
						Created_at={ruta.Created_at}
						Updated_by={ruta.Updated_by}
						Updated_at={ruta.Updated_at}
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
