<script lang="ts">
	export let userId: string;
	export let url: string;
	export let cursoId: string;
    export let assigned_by: string;
    export let created_by: string;
    export let created_at: string;
    export let updated_by: string;
    export let updated_at: string;
</script>

<tr>
	<td>
		<label>
			<input type="checkbox" class="checkbox" />
		</label>
	</td>
	<td>
		<div class="flex items-center space-x-3">
			<div class="avatar">
				<div class="mask mask-square w-12 h-12">
					<img src="/images/curso-online.png" alt={userId} />
				</div>
			</div>
			<div>
				<div class="font-bold">
					<a href={url} target="_blank" class="titulo" rel="noreferrer">{userId}</a>
				</div>
				<div class="text-sm opacity-50" />
			</div>
		</div>
	</td>
	<td>
		{cursoId}
	</td>
    <td>
        {assigned_by}
    <td>
        {created_by}
    </td>
    <td>
        {created_at}
    </td>
    <td>
        {updated_by}
    </td>
    <td>
        {updated_at}
    </td>
</tr>

<style>
a.titulo:hover{
color:red;
}
</style>
