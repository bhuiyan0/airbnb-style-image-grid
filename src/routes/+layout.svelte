<script>
	import '../app.postcss';
	import '../app.css';
	import NavBar from "$lib/Components/Header/NavBar.svelte";
</script>

<header class="sticky top-0 h-[4.5rem] shadow-md">
	<NavBar />
</header>

<main class="main overflow-auto h-[calc(100%-4.5rem)]">
	<slot />
</main>
