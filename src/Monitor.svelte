<script lang="ts">
    export let name: string;
    export let url: string;
    export let diabled = false;
</script>

<main>
    <div class="caption">
        { name }
    </div>
    <br/>
    Url: { url }
</main>

<style>
    main {
        margin: 20px;
        padding: 20px;
        min-width: 300px;
        background-color: #f4f4f4;
        border-radius: 10px;
    }

    .caption {
        font-size: 20px;
    }
</style>