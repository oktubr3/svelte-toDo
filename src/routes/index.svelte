<!-- https://youtu.be/AV04qN6frmE?t=500 -->
<script>
import { each } from "svelte/internal";

    let toDos = [
        { task: "Comer", isComplete: false, createdAt: "2021-09-21"},
        { task: "Cagar", isComplete: true, createdAt: "2021-09-21"},
        { task: "Morir", isComplete: false, createdAt: "2021-09-21"}, 
    ];
</script>

<ol>
    {#each toDos as todo}
    <li class:complete={todo.isComplete}>
        {todo.task}        
    </li> 
    {/each}
</ol>

<style>
    .complete {
        text-decoration: line-through;
    }
</style>
