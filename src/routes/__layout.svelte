<script>
  import '../app.css'
  import Nav from '$lib/Nav.svelte'
</script>

<Nav />
<main>
  <slot />
</main>

<style>
  main {
    max-width: 600px;
    margin: 2rem auto;
    font-size: 1rem;
  }
</style>
