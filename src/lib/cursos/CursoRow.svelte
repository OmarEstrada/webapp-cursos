<script lang="ts">
	export let nombre: string;
	export let url: string;
	export let descripcion: string;
</script>

<tr>
	<th>
		<label>
			<input type="checkbox" class="checkbox" />
		</label>
	</th>
	<td>
		<div class="flex items-center space-x-3">
			<div class="avatar">
				<div class="mask mask-square w-12 h-12">
					<img src="/images/curso-online.png" alt={nombre} />
				</div>
			</div>
			<div>
				<div class="font-bold">
					<a href={url} target="_blank" rel="noreferrer">{nombre}</a>
				</div>
				<div class="text-sm opacity-50" />
			</div>
		</div>
	</td>
	<td>{descripcion}</td>
</tr>

<style>
</style>
