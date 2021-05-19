<script lang="ts">
    import Monitor from "./Monitor.svelte";
</script>

<main>
    <div class="monitor-list">
        <Monitor name="Database API"
                 url="https://dark-triad-db.herokuapp.com/status/details"
                 environment="Prod"/>
        <Monitor name="Database API Local"
                 url="http://localhost:8080/status/details"
                 environment="Local"/>
        <Monitor name="Login Service"
                 url="https://dark-triad-login-service.herokuapp.com/status/details"
                 environment="Prod"/>
        <Monitor name="Public Database API"
                 url="https://dark-triad-db-public.herokuapp.com/status/details"
                 note="Not yet deployed"
                 disabled/>
        <Monitor name="Game Server"
                 note="Not yet deployed"
                 disabled/>
    </div>
</main>

<style>
    main {
    }

    .monitor-list {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }
</style>