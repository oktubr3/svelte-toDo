<!-- Youtube Video:  https://www.youtube.com/watch?v=50s6IKO8028&list=PLm_Qt4aKpfKiGbdjaHdOpry6Neza0etxZ&index=3 -->
<script>
import { each } from "svelte/internal";

    let todos = [];
    let task = "";
    let item = "";

    const addTodo = () => {
        let todo =  { task: task, isComplete: false, createdAt: new Date()};
        todos = [todo, ...todos];
        task = "";
    }

$: console.log(item);
</script>


<input type="text" placeholder="Add Task" bind:value={task} />
<button on:click={addTodo}>Add</button>

<ol>
    {#each todos as item}
        <li class:complete={item.isComplete}> {item.task} </li> 
    {/each}
</ol>

<style>
    .complete {
        text-decoration: line-through;
    }
</style>
