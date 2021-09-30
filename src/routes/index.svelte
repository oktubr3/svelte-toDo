<!-- Youtube Video:  https://www.youtube.com/watch?v=Npc_MOXZKjM&list=PLm_Qt4aKpfKiGbdjaHdOpry6Neza0etxZ&index=4 -->
<script>
import { each } from "svelte/internal";

    let todos = [];
    let task = "";
    let item = "";

    const addTodo = () => {
        let todo =  { task: task, isComplete: false, createdAt: new Date()};
        todos = [todo, ...todos];
        task = "";
    };

    const markTodoAsComplete = (index) => {
        todos[index].isComplete = !todos[index].isComplete;
    };

    const deleteTodo = (index) => {
        let deleteItem = todos[index];
        todos = todos.filter((item) => item != deleteItem);
    };

$: console.table(todos);
</script>


<input type="text" placeholder="Add Task" bind:value={task} />
<button on:click={addTodo}>Add</button>

<ol>
    {#each todos as item, index}
        <li class:complete={item.isComplete}>
            <span>
                {item.task}
            </span>
            <span>
                <button on:click={() => markTodoAsComplete(index)}>✔</button>
                <button on:click={() => deleteTodo(index)}>X</button>
            </span>
        </li> 
    {:else}
        <p>No ToDos Found</p>
    {/each}
</ol>

<style>
    .complete {
        text-decoration: line-through;
    }
</style>
