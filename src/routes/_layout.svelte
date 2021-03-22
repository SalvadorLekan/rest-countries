<script lang="ts">
  import Nav from "../components/Nav.svelte";
</script>

<Nav />

<main>
  <slot />
</main>

<style>
  main {
    padding-inline: 30px;
  }
  @media (min-width: 1200px) {
    main {
      padding-inline: 75px;
    }
  }
</style>
