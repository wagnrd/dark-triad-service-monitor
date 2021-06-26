<script lang="ts">
    import Monitor from "./Monitor.svelte";
</script>

<main>
    <div class="monitor-list">
        <Monitor name="Database API"
                 url="https://dark-triad-db.herokuapp.com/status/details"
                 environment="Prod"/>
        <Monitor name="Login Service"
                 url="https://dark-triad-login-service.herokuapp.com/status/details"
                 environment="Prod"/>
        <Monitor name="Ingress"
                 url="https://dark-triad-ingress.herokuapp.com/status/details"
                 environment="Prod"/>
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