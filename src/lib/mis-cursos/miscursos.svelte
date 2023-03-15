<script>
	import MiscursosRow from './miscursosRow.svelte';
	import Loading from '$lib/common/Loading.svelte';
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';

	let loading = true;
	let miscrusos = [];
	let datosmiscrusos = {
		id: null,
		nombre: '',
		url: '',
		descripcion: ''
	};

	const cargarContenido = async () => {
		try {
			await wait(1000);

			const result = await fetch(`${API_URL}/Cursos`);

			if (result.ok) {
				miscrusos = await result.json();
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
			<tbody>
				{#each miscrusos as miscruso}
					<MiscursosRow
						id={miscruso.id}
						nombre={miscruso.nombre}
						descripcion={miscruso.descripcion}
						url={miscruso.url}
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
