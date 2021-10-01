<!-- Youtube Video:  https://www.youtube.com/watch?v=_TTlatg865k&list=PLm_Qt4aKpfKiGbdjaHdOpry6Neza0etxZ&index=6 -->
<script>
import { initializeApp, getApps, getApp } from 'firebase/app';
import { getFirestore, collection, onSnapshot, doc, updateDoc, deleteDoc, setDoc, addDoc } from 'firebase/firestore'
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

let task = "";
let error = "";

const addTodo = async () => {
    if (task != "") {
        const docRef = await addDoc(collection(db, "todos"), {
            task: task, 
            isComplete: false, 
            createdAt: new Date()
        });
        task = "";
        error = "";
    } else {
        error = "Task is Empty";
    }
};

const markTodoAsComplete = async (todo) => {
    // todos[index].isComplete = !todos[index].isComplete;
    await updateDoc(doc(db, "todos", todo.id), {
        isComplete : !todo.isComplete
    });
};

const deleteTodo = async (todo) => {
    // let deleteItem = todos[index];
    // todos = todos.filter((todo) => todo != deleteItem);
    await deleteDoc(doc(db, "todos", todo.id));
};

const keyIsPressed = (event) => {
    if (event.key === "Enter"){
        addTodo();
    }
};
</script>


<input type="text" placeholder="Add Task" bind:value={task} />
<button on:click={addTodo}>Add</button>

<ol>
    {#each todos as todo}
        <li>
            <span class:complete={todo.isComplete}>
                {todo.task}
            </span>
            <span>
                <button on:click={() => markTodoAsComplete(todo)}>✔</button>
                <button on:click={() => deleteTodo(todo)}>X</button>
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
