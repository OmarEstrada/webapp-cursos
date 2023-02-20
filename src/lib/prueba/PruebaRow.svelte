<script lang="ts">
	import { goto } from "$app/navigation";
import { createEventDispatcher } from "svelte";
const dispatch = createEventDispatcher();

	export let id:string;
	export let nombre: string;
	export let url: string;
	export let descripcion: string; 

	function clickEdit() {
		goto('prueba/edit/'+id);
	}
	function clickDelete() {
		//todo: confirm action
		dispatch('deleteClicked', {id});
	}

</script>

<tr>
	<td>
		<button class="btn btn-primary" on:click={clickEdit}>EDIT</button> 
		<button class="btn btn-error" on:click={clickDelete}>DELETE</button> 
	</td>
	<td>
		<div class="flex items-center space-x-3">
			<div class="avatar">
				<div class="mask mask-square w-12 h-12">
					<img src="/images/curso-online.png" alt={nombre} />
				</div>
			</div>
			<div>
				<div class="font-bold">
					<a href={url} target="_blank" class="titulo" rel="noreferrer">{nombre}</a>
				</div>
				<div class="text-sm opacity-50" />
			</div>
		</div>
	</td>
	<td>
		{descripcion}
		<p>Mensaje extra</p>
	</td>
</tr>

<style>
a.titulo:hover{
color:red;
}
</style>
