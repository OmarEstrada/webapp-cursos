<script>
	import Loading from '$lib/common/Loading.svelte';
    import RutaRow from '$lib/rutas/RutaRow.svelte';
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';

	let loading = true;
	let rutas = [];

	const cargarContenido = async () => {
		try {
			await wait(2000);

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
</script>

{#if loading}
	<Loading />
{:else}
	<div class="overflow-x-auto w-full">
		<table class="table w-full">
			<!-- head -->
			<thead>
				<tr>
					<th>
						<label>
							<input type="checkbox" class="checkbox" />
						</label>
					</th>
					<th>Nombre</th>
					<th>Descripción</th>
                    <th>Activo</th>
                    <th>Created_by</th>
                    <th>Created_at</th>
                    <th>Updated_by</th>
                    <th>Updated_by</th>
					<th />
				</tr>
			</thead>
			<tbody>
				{#each rutas as ruta}
					<RutaRow nombre={ruta.nombre} Descripcion={ruta.Descripcion} url={ruta.url} Activo={ruta.Activo} Created_by={ruta.Created_by} Created_at={ruta.Created_at} Updated_by={ruta.Updated_by} Updated_at={ruta.Updated_at}/>
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