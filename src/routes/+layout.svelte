<script>
  import "../app.css";
	import Menu from "../lib/menu/Menu.svelte";
	import MenuDrawer from "../lib/menuDrawer/MenuDrawer.svelte";
</script>

<MenuDrawer>
	<slot />
</MenuDrawer>