<script>
	import Loading from '$lib/common/Loading.svelte';
	import CursoRow from '$lib/cursos/CursoRow.svelte';
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';

	let loading = true;
	let cursos = [];

	const cargarContenido = async () => {
		try {
			await wait(2000);

			const result = await fetch(`${API_URL}/Cursos`);

			if (result.ok) {
				cursos = await result.json();
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
					<th />
				</tr>
			</thead>
			<tbody>
				{#each cursos as curso}
					<CursoRow nombre={curso.nombre} descripcion={curso.descripcion} url={curso.url} />
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
			<!-- foot -->
			<tfoot>
				<tr>
					<th />
					<th>Nombre</th>
					<th>Descripción</th>
					<th />
				</tr>
			</tfoot>
		</table>
	</div>
{/if}
