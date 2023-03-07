<script>
	import Loading from '$lib/common/Loading.svelte';
	import UsuariosRow from './UsuariosRow.svelte';
	import { onMount } from 'svelte';
	import { API_URL } from '../../core/constantes';
	import { wait } from '../../core/wait';

	let loading = true;
	let usuarios = [];
	let datosusuarios = {
		id: null,
		nombre: '',
		url: '',
		descripcion: '',
		email: '',
		provider: '',
		password: ''
	};

	const cargarContenido = async () => {
		try {
			await wait(1000);

			const result = await fetch(`${API_URL}/Usuarios`);

			if (result.ok) {
				usuarios = await result.json();
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	onMount(cargarContenido);

	const deleteUsuario = async ({ detail: { id } }) => {
		try {
			loading = true;

			const result = await fetch(`${API_URL}/Usuarios/${id}`, {
				method: 'DELETE'
			});

			if (result.ok) {
				usuarios = usuarios.filter((usuario) => usuario.id != id);
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	const editUsuario = async ({ detail: { id } }) => {
		const usuarioAEditar = usuarios.find((usuario) => usuario.id == id);

		if (!usuarioAEditar) return;
		datosusuarios = { ...usuarioAEditar };
	};

	let agregarUsuario = async () => {
		const nuevousuario = {
			id: datosusuarios.id,
			nombre: datosusuarios.nombre,
			email: datosusuarios.email,
			provider: datosusuarios.provider,
			password: datosusuarios.password
		};
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Usuarios`, {
				method: 'POST',
				body: JSON.stringify(nuevousuario),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				const nuevo = await result.json();

				usuarios = [...usuarios, nuevo];
			}
		} catch (error) {
			console.error(error);
		} finally {
			loading = false;
		}
	};

	let actualizarusuario = async () => {
		try {
			loading = true;
			const result = await fetch(`${API_URL}/Usuarios/` + datosusuarios.id, {
				method: 'PUT',
				body: JSON.stringify(datosusuarios),
				headers: {
					'Content-Type': 'application/json'
				}
			});

			if (result.ok) {
				usuarios = usuarios.map((usuario) => {
					if (usuario.id != datosusuarios.id) {
						return usuario;
					}

					return datosusuarios;
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
					<th>email</th>
					<th>provider</th>
					<th>password</th>
				</tr>
				<tr>
					<th>
						<button class="btn btn-success" on:click|preventDefault={agregarUsuario}
							>agregarRuta</button
						>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datosusuarios.nombre}
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
								bind:value={datosusuarios.email}
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
								bind:value={datosusuarios.provider}
								required
								type="text"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">Provider</label>
						</div>
					</th>
					<th>
						<div class="single-input">
							<input
								bind:value={datosusuarios.password}
								required
								type="text"
								name=""
								id="nome"
								class="input"
								aria-describedby="help"
							/>
							<label for="nome">Password</label>
						</div>
					</th>
					<th>
						<button class="btn btn-success" on:click|preventDefault={actualizarusuario}
							>Actualizar</button
						>
					</th>
				</tr>
			</thead>
			<tbody>
				{#each usuarios as usuario}
					<UsuariosRow
						id={usuario.id}
						nombre={usuario.nombre}
						email={usuario.email}
						url={usuario.url}
						provider={usuario.provider}
						password={usuario.password}
						on:deleteClicked={deleteUsuario}
						on:editarClicked={editUsuario}
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
