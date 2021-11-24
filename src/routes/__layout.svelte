<script context="module">
  import '../layout.css';
  import PodcastPlayer from '../components/PodcastPlayer.svelte';
</script>

<main>
  <slot />
</main>
<PodcastPlayer />

<style>
  main {
    padding: 2rem;
    max-width: 800px;
    margin: 0 auto 12rem;
  }
</style>
