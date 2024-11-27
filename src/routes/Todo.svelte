<script>
import trashIcon from "$lib/img/trash.png"
let todoItem = $state('');
let todoList = $state([]);

function addItem(event) {
    event.preventDefault();
    if (todoItem == '') {
        return;
    }
    todoList = [...todoList, {
        text: todoItem,
        done: false
    }];
    todoItem = "";
}
function removeItem(index) {
    todoList = todoList.toSpliced(index, 1);
}


$inspect(todoList);
</script>

<form onsubmit={addItem}>
<input type="text" bind:value={todoItem}>
<button type="submit">Add</button>
</form>

<div class="todo-list-container">
    <ul>
        {#each todoList as item, index}
            <li>
                <input type="checkbox" bind:checked={item.done}>
                <span class:done={item.done}>{item.text}</span>
                <button type="button" aria-label="delete" onclick={()  => removeItem(index)}><img class="trash" src="{trashIcon}" alt="trash"></button>
            </li>
        {/each}
    </ul>   
</div>
<style>

/* input bar */
form {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  
input[type="text"] {
    flex: 1; 
    padding: 10px;
    font-size: 16px;
    border: 2px solid #ccc;
    border-radius: 8px;
    outline: none;
    transition: border-color 0.3s;
  }
  
input[type="text"]:focus {
    border-color: #DC7AC0; 
    box-shadow: 0 0 5px rgba(255, 39, 241, 0.5);
  }
  
button[type="submit"] {
    margin-left: 10px;
    padding: 10px 20px;
    font-size: 16px;
    background-color: #DC7AC0;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s, box-shadow 0.3s;
  }
  
button[type="submit"]:hover {
    background-color: #b65c9c;
  }
  
button {
    font-family: "Emblema One", system-ui;
  }

button[type="submit"]:active {
    box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.2);
  }

/* White rectangle for the list */
.todo-list-container {
    width: 100%;
    max-width: 500px;
    background-color: white;
    border: 2px solid #ccc;
    border-radius: 8px;
    padding: 20px 40px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    font-family: "Fira Sans", sans-serif;
    font-weight: 400;
    font-style: normal;
  }

ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
  }

li {
    padding: 10px;
    border-bottom: 1px solid #eee;
    position: relative;
  }

li:last-child {
    border-bottom: none;
  }
span.done {
    color: #DC7AC0;
    text-decoration: line-through;
}
</style>