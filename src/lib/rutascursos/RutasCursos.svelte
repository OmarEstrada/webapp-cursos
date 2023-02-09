<script>
	import Loading from '$lib/common/Loading.svelte';
	import RutasCursosRow from '$lib/rutascursos/RutasCursosRow.svelte';
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';

	let loading = true;
	let rutascursos = [];

	const cargarContenido = async () => {
		try {
			await wait(2000);

			const result = await fetch(`${API_URL}/rutascursos`);

			if (result.ok) {
				rutascursos = await result.json();
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
					<th>rutaId</th>
					<th>cursoId</th>
					<th>created_by</th>
					<th>created_at</th>
					<th>update_by</th>
					<th>update_at</th>
					<th />
				</tr>
			</thead>
			<tbody>
				{#each rutascursos as rutacurso}
					<RutasCursosRow rutaId={rutacurso.rutaId} url={rutacurso.url} cursoId={rutacurso.cursoId} created_by={rutacurso.created_by} created_at={rutacurso.created_at} updated_by={rutacurso.updated_by} updated_at={rutacurso.updated_at} />
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
