<script>
    export let object;
</script>

<tr>
    {#each Object.keys(object) as key}
        <td>{object[key]}</td>
    {/each}
</tr>

<style>
    td {
        border: 2px solid black;
        padding: 5px 10px;
    }
    tr:nth-child(even){
        background-color: #CCC4;
    }
</style>