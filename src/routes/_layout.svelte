<script>
	import Nav from '../components/Nav.svelte';
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    min-height: 100vh
  }

	main {
    width: 100vw;
		background-color: white;
    display: flex;
    flex: 1;
	}
</style>

<div class="container">
  <Nav />

  <main>
    <slot></slot>
  </main>
</div>
