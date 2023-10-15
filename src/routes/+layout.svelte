<script>
  import '../styles.css'
  import Header from '../components/Header.svelte';
</script>

<div class="main-layout">
    <Header></Header>
    <div class="app-content">
        <slot />
    </div>
</div>

<style>
    .main-layout {
        min-height: 100vh;
        width: 100%;
    }

    .app-content {
        padding: 0 4rem;
        width: 100%;
        height: calc(100vh - calc(3vh + 4rem));

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
</style>