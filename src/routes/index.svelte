<!-- Youtube Video:  https://www.youtube.com/watch?v=_TTlatg865k&list=PLm_Qt4aKpfKiGbdjaHdOpry6Neza0etxZ&index=6 -->
<script>
import { initializeApp, getApps, getApp } from 'firebase/app';
import { getFirestore, collection, onSnapshot } from 'firebase/firestore'
import { firebaseConfig } from "$lib/firebaseConfig";
import { browser } from "$app/env";

const app = browser && (getApps().length === 0 ? initializeApp(firebaseConfig) : getApp());
const db = browser && getFirestore();

const colRef = browser && collection(db, "todos");

let todos = [];

const unsubscribe = browser && onSnapshot(colRef, (querySnapshot) => {
  let fbTodos = [];
  querySnapshot.forEach((doc) => {
      let todo = {...doc.data(), id: doc.id};
      fbTodos = [todo, ...fbTodos];
  });
  todos = fbTodos;
});

console.log( app, db)

let task = "";
let error = "";

const addTodo = () => {
    let todo =  { task: task, isComplete: false, createdAt: new Date()};
    
    if (task != "") {
        todos = [todo, ...todos];
        task = "";
        error = "";
    } else {
        error = "Task is Empty";
    }
};

const markTodoAsComplete = (index) => {
    todos[index].isComplete = !todos[index].isComplete;
};

const deleteTodo = (index) => {
    let deleteItem = todos[index];
    todos = todos.filter((todo) => todo != deleteItem);
};

const keyIsPressed = (event) => {
    if (event.key === "Enter"){
        addTodo();
    }
};

$: console.table(todos);
</script>


<input type="text" placeholder="Add Task" bind:value={task} />
<button on:click={addTodo}>Add</button>

<ol>
    {#each todos as todo, index}
        <li class:complete={todo.isComplete}>
            <span>
                {todo.task}
            </span>
            <span>
                <button on:click={() => markTodoAsComplete(index)}>✔</button>
                <button on:click={() => deleteTodo(index)}>X</button>
            </span>
        </li> 
    {:else}
        <p>No ToDos Found</p>
    {/each}
    <p class="error">{error}</p>
</ol>

<svelte:window on:keydown={keyIsPressed} />

<style>
    .complete {
        text-decoration: line-through;
    }
    .error {
        color: red;
    }
</style>
