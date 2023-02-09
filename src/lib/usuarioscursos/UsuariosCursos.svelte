<script>
	import Loading from '$lib/common/Loading.svelte';
    import UsuariosCursosRow from '$lib/usuarioscursos/UsuariosCursosRow.svelte'
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';
	let loading = true;
	let usuarioscursos = [];

	const cargarContenido = async () => {
		try {
			await wait(2000);

			const result = await fetch(`${API_URL}/usuarioscursos`);

			if (result.ok) {
				usuarioscursos = await result.json();
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
					<th>userId</th>
					<th>cursoId</th>
                    <th>asssigned_by</th>
					<th>created_by</th>
					<th>created_at</th>
					<th>update_by</th>
					<th>update_at</th>
					<th />
				</tr>
			</thead>
			<tbody>
				{#each usuarioscursos as usuariocurso}
					<UsuariosCursosRow userId={usuariocurso.userId}  url={usuariocurso.url} cursoId={usuariocurso.cursoId} assigned_by={usuariocurso.assigned_by} created_by={usuariocurso.created_by} created_at={usuariocurso.created_at} updated_by={usuariocurso.updated_by} updated_at={usuariocurso.updated_at} />
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