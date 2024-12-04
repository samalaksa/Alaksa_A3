<script>
import trashIcon from "$lib/img/trash.png"
import editIcon from "$lib/img/pen.png"
import checkmIcon from "$lib/img/checkM.png"
import flowerImg from "$lib/img/flower.png"
import { fly } from 'svelte/transition';
let todoItem = $state('');
let todoList = $state([]);

function addItem(event) {
    event.preventDefault();
    if (todoItem == '') {
        return;
    }
    todoList = [...todoList, {
        text: todoItem,
        done: false,
        isEditing: false,
    }];
    todoItem = "";
}
function removeItem(index) {
    todoList = todoList.toSpliced(index, 1);
}
function toggleEdit(index) {
  todoList = todoList.map((item, i) =>
  i === index ? { ...item, isEditing: !item.isEditing } : item
  );
}
function updateItem(index, newText) {
  todoList = todoList.map((item, i) =>
  i === index ? { ...item, text: newText, isEditing: false } : item
  );
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
            <li in:fly={{ x: -200, duration: 500 }} out:fly={{ x:200, duration: 500}}>
              {#if item.isEditing}
                <div class="todo-item">
                  <input
                    type="text"
                    bind:value={item.text}
                    onkeydown={(event) => {
                      if (event.key === 'Enter') updateItem(index, item.text);
                    }}
                    />
                  <button type="savebutton" aria-label="save" onclick={() => updateItem(index, item.text)}>
                    <img class="save" src="{checkmIcon}" alt="Save" />
                  </button>
                </div>
              {:else}
              <div class="todo-item">
                <div class="text-container">
                  <input type="checkbox" bind:checked={item.done}/>
                    <span class:done={item.done}>{item.text}</span>
                      </div>
                        <div class="button-container">
                        <button type="editbutton" aria-label="edit" onclick={() => toggleEdit(index)}>
                      <img class="edit" src="{editIcon}" alt="Edit" />
                    </button>
                  <button type="deletebutton" aria-label="delete" onclick={()  => removeItem(index)}><img class="trash" src="{trashIcon}" alt="trash"/></button>
                </div>
              </div>
              {/if}
            </li>
        {/each}
    </ul>   
</div>
<div class="flower-image-container">
  <img src="{flowerImg}" alt="Flower"/>
</div>

<style>
  /* flower image */
  .flower-image-container {
      position: fixed; 
      bottom: 0;
      left: 0;
      width: 100%;
      z-index: -1;
      text-align: right;
      pointer-events: none; 
  }

  .flower-image-container img {
      width: auto;
      height: 400px;
      margin-left: 450px; 
      opacity: 20%;
  }
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
    font-family: "Fira Sans", sans-serif;
    font-weight: 400;
    font-style: normal;
  }

button[type="submit"]:active {
    box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.2);
  }
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  gap: 20px;
}
.text-container {
  display: flex;
  align-items: center;
  gap: 10px;
}
.button-container {
  display: flex;
  gap: 10px;
}
/* checkbox */
.text-container input[type="checkbox"] {
  appearance: none;
  width: 20px;
  height: 20px;
  margin: 0;
  border: 2px solid #DC7AC0; 
  border-radius: 4px; 
  outline: none;
  cursor: pointer;
  position: relative;
  display: inline-block;
  vertical-align: middle;
  background-color: #fff;
  transition: background-color 0.3s, border-color 0.3s, box-shadow 0.3s;
}

.text-container input[type="checkbox"]:focus {
  box-shadow: 0 0 4px 2px rgba(220, 122, 192, 0.5);
}

.text-container input[type="checkbox"]:checked {
  background-color: #DC7AC0;
  border-color: #DC7AC0;
}

.text-container input[type="checkbox"]:checked::after {
  content: '';
  position: absolute;
  top: 1px;
  left: 4.5px;
  width: 5px;
  height: 9px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
  display: block;
}
/* delete button */
button[type="deletebutton"] {
  background: none; 
  border: none;
  padding: 0;
  cursor: pointer;
  outline: none;
}

button[type="deletebutton"]:focus {
  outline: none;
}

.trash {
  width: 20px;
  height: 20px;
  display: block;
  pointer-events: none;
}
/* edit button */
button[type="editbutton"] {
  background: none; 
  border: none;
  padding: 0;
  cursor: pointer;
  outline: none;
}

button[type="editbutton"]:focus {
  outline: none;
}

.edit {
  width: 28px;
  height: 28px;
  display: block;
  pointer-events: none;
}

/* edit bar */ 
.todo-item input[type="text"]:focus {
  border-color: #DC7AC0;
  box-shadow: 0 0 5px rgba(255, 39, 241, 0.5);
}

/* save button */
.todo-item input[type="text"] {
    flex: 1;
    padding: 8px;
    font-size: 14px;
    border: 2px solid #ccc;
    border-radius: 5px;
}

button[type="savebutton"] {
  background: none; 
  border: none;
  padding: 0;
  cursor: pointer;
  outline: none;
}

button[type="savebutton"]:focus {
  outline: none;
}

.save {
  width: 24px;
  height: 24px;
  display: block;
  pointer-events: none;
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
    display: flex;
    align-items: center;
    padding: 10px;
    border-bottom: 1px solid #eee;
    position: relative;
  }

li:last-child {
    border-bottom: none;
  }
span.done {
    color: #f6d9ee;
    text-decoration: line-through;
}
</style>