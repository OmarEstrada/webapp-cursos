<script>
	import Loading from '$lib/common/Loading.svelte';
	import PruebaRow from './PruebaRow.svelte';
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';
	import Cursos from '$lib/cursos/Cursos.svelte';

	let loading = true;
	let cursos = [];
	let activado = true;
	let datoscursos = {
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
				method: 'DELETE'
			});

			if (result.ok) {
				cursos = cursos.filter((curso) => curso.id != id);
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	const editCurso = async ({ detail: { id } }) => {
		const cursoAEditar = cursos.find((curso) => curso.id == id);

		if (!cursoAEditar) return;
		datoscursos = { ...cursoAEditar };
	};

	let agregarcurso = async () => {
		const nuevocurso = {
			id: datoscursos.id,
			nombre: datoscursos.nombre,
			url: datoscursos.url,
			descripcion: datoscursos.url
		};
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Cursos`, {
				method: 'POST',
				body: JSON.stringify(nuevocurso),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				const nuevo = await result.json();

				cursos = [...cursos, nuevo];
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	let actualizarcurso = async () => {
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Cursos/` + datoscursos.id, {
				method: 'PUT',
				body: JSON.stringify(datoscursos),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				cursos = cursos.map((curso) => {
					if (curso.id != datoscursos.id) {
						return curso;
					}

					return datoscursos;
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
					<th />
				</tr>
				<tr>
					<th>
						<button class="btn btn-success" on:click|preventDefault={agregarcurso}
							>AgregarCurso</button
						>
					</th>
					<th>
						<input
							bind:value={datoscursos.nombre}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<input
							bind:value={datoscursos.descripcion}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<input
							bind:value={datoscursos.url}
							type="text"
							class="from control"
							name=""
							id=""
							aria-describedby="help"
						/>
					</th>
					<th>
						<button class="btn btn-success" on:click|preventDefault={actualizarcurso}
							>Actualizar</button
						>
					</th>
				</tr>
			</thead>
			<tbody>
				{#each cursos as curso}
					<PruebaRow
						id={curso.id}
						nombre={curso.nombre}
						descripcion={curso.descripcion}
						url={curso.url}
						on:deleteClicked={deleteCurso}
						on:editarClicked={editCurso}
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
