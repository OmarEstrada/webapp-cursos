<script>
	import Loading from '$lib/common/Loading.svelte';
	import PruebaRow from './PruebaRow.svelte';
	
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';

	let loading = true;
	let cursos = [];

	const cargarContenido = async () => {
		try {
			await wait(1000);

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


	const deleteCurso = async ({ detail: { id } }) => {

		try { 
            loading = true;

			const result = await fetch(`${API_URL}/Cursos/${id}`, {
                method: "DELETE"
            });

			if (result.ok) {
				cursos = cursos.filter(curso => curso.id != id);
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		} 
	}
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
					<th>
						Actions
					</th>
					<th>Nombre</th>
					<th>Descripción</th>
					<th />
				</tr>
			</thead>
			<tbody>
				{#each cursos as curso}
					<PruebaRow id={curso.id} nombre={curso.nombre} descripcion={curso.descripcion} url={curso.url} 
					on:deleteClicked={deleteCurso} 
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
