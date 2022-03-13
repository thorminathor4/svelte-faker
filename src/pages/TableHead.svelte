<script>
    export let properties;
</script>

<thead>
    {#each properties as key}
        <th>{key}</th>
    {/each}
</thead>

<style>
    thead{
        background: #4CC4;
    }
    th {
        background-color: transparent;
        color: black;
        border: 2px solid black;
        padding: 5px 10px;
    }
</style>