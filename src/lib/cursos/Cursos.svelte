<script>
	import Loading from '$lib/common/Loading.svelte';
	import CursoRow from './CursoRow.svelte';
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';

	let loading = true;
	let cursos = [];
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
						<button class="btn btn-success" on:click|preventDefault={agregarcurso}
							>AgregarCurso</button
						>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datoscursos.nombre}
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
								bind:value={datoscursos.descripcion}
								required
								type="text"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">Descripcion</label>
						</div>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datoscursos.url}
								required
								type="text"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">url</label>
						</div>
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
					<CursoRow
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
