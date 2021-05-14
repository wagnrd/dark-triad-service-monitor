<script lang="ts">
    import Monitor from "./Monitor.svelte";
</script>

<main>
    <Monitor name="Database API" url="some url"/>
    <Monitor name="Login Service" url="some url"/>
    <Monitor name="Public Database API" url="some url" diabled/>
    <Monitor name="Game Server" url="some url" diabled/>
</main>

<style>
    main {
        display: flex;
        justify-content: center;
    }
</style>