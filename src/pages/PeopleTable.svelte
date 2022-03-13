<script>
    import faker from "@faker-js/faker";
    import TableHead from "./TableHead.svelte";
    import TableRow from "./TableRow.svelte";

    function prettyfy(string){
        string = string[0].toUpperCase() + string.slice(1, string.length);
        for(let i = 1; i < string.length; i++)
            if(string[i] === string[i].toUpperCase()){
                string = string.slice(0, i) + " " + string.slice(i, string.length);
                i++;
            }
        return string;
    }

    let people;
    let sortBy;
    let rows = 10;
    let accending = true;

    function sortFunction(personA, personB){
        let difference = 0;
        try{
            let a = personA[sortBy], b = personB[sortBy];
            difference = accending ? a.localeCompare(b) : b.localeCompare(a);
        }catch(error){
            let a = Number(personA[sortBy]), b = Number(personB[sortBy]);
            difference = accending ? a - b : b - a;
        }
        return difference;
    }

    class Person{
        constructor(number, firstName, lastName, proffession){
            this.number = number;
            this.firstName = firstName;
            this.lastName = lastName;
            this.age = Math.floor(20 + Math.random() * 40);
            this.proffession = proffession;
        }
    }

    generatePeople();

    function generatePeople(){
        people = [];
        for(let i = 0; i < rows; i++){
            let firstName = faker.name.firstName();
            let lastName = faker.name.lastName();
            let proffession = faker.name.jobTitle();
            let person = new Person(i+1, firstName, lastName, proffession);
            people.push(person);
        }
        if(sortBy) people = people.sort(sortFunction);
    }

    function setSorting(property){
        if(sortBy === property){
            accending = !accending;
        }
        sortBy = property;
        console.log("Sorting by", prettyfy(sortBy));
        people = people.sort(sortFunction);
    }

</script>

<h1>Random People</h1>

<p>
    {#if sortBy}
        Sorting by {prettyfy(sortBy)} - {accending ? "Accending" : "Decending"}
    {:else}
        Not Sorting
    {/if}
</p>


<table>
    <TableHead properties={Object.keys(new Person).map(prettyfy)}/>
    <tbody>
        {#each people as person}
            <TableRow object={person}/>
        {/each}
    </tbody>
</table>

<div class="buttons">
    <button class="gen-button" on:click={generatePeople}>Generate {rows} New People</button>
    {#each Object.keys(new Person) as key}
        <button on:click={() => setSorting(key)}>Sort By {prettyfy(key)}</button>
    {/each}
</div>

<style>
    h1{
        padding: 0;
        margin: 10px 0;
        width: 100%;
        text-align: center;
    }
    p{
        margin: 0;
        padding: 0;
        margin-bottom: 10px;
        width: 100%;
        text-align: center;
        font-weight: bold;
    }
    table{
        border-collapse: collapse;
        min-width: fit-content;
        width: fit-content;
        position: relative;
        transform: translate(-50%, 0);
        left: 50%;
    }
    button{
        margin: 5px;
        font-weight: bold;
        background: #4CC4;
        color: black;
        border: 2px solid black;
        border-radius: 20px;
        display: inline-block;
        width: fit-content;
        padding: 5px 20px;
    }
    button:not(:disabled):active {
        background-color: #4CC;
    }
    .buttons{
        margin-top: 20px;
        width: fit-content;
        position: relative;
        transform: translate(-50%, 0);
        left: 50%;
    }
    .gen-button{
        background-color: #CCC4;
    }
    .gen-button:not(:disabled):active {
        background-color: #CCC;
    }
</style>

<!--<h1>{faker.name.findName()}</h1>-->
