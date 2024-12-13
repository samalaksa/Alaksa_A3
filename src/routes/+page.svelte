<script>
    import Todo from './Todo.svelte';
    import Sidebar from './Sidebar.svelte'
    import '../app.css';
    import underline from "$lib/img/underline.png"
    import menu from "$lib/img/menu.png"

// LIST MANAGER
let storedList, storedId;
let currentListId = $state(1);
let currentList = $state([]);
let todoLists = $state([
    {id: 1, name: 'Self Care', items: [] },
]);

/* Load and update LocalStorage */
$effect.pre(() => {
  const storedList = localStorage.getItem('todoLists');
  const storedId = localStorage.getItem('currentListId');
  if (storedList) {
    todoLists = JSON.parse(storedList);
  }
  if (storedId) {
    currentListId = JSON.parse(storedId);
  }
}); 

// update localstorage and current list 
$effect(() => {
    currentList = todoLists.find(list => list.id === currentListId);  
    if (currentListId) {
      localStorage.setItem('currentListId', currentListId);
    }
    if (todoLists) {
      localStorage.setItem('todoLists', JSON.stringify(todoLists));
    }
});

  let isSidebarOpen = $state(false);

  const toggleSidebar = () => {
    isSidebarOpen = !isSidebarOpen;
  };

  const closeSidebar = () => {
    isSidebarOpen = false;
  };

  const createNewList = (newListName) => {
    const newList = { id: todoLists.length + 1, name: newListName, items: [] };
    todoLists = [...todoLists, newList];
    currentListId = newList.id;
  };

  const selectList = (listId) => {
    currentListId = listId;
    closeSidebar();
  };

  const updateList = (updatedList) => {
    todoLists = todoLists.map((list) =>
      list.id === currentListId ? { ...list, items: updatedList } : list
    );
  };
  // Function to edit the name of a list
  const editListName = (listId, newName) => {
    todoLists = todoLists.map((list) => 
      list.id === listId ? { ...list, name: newName } : list
    );
    currentList = todoLists.find(list => list.id === currentListId); // Update current list after renaming
  };

  // Function to delete a list
  const deleteList = (listId) => {
    todoLists = todoLists.filter((list) => list.id !== listId);
    if (todoLists.length > 0) {
      currentListId = todoLists[0].id; // Set the first list as the current list if one exists
    } else {
      currentListId = null; // No lists left, reset current list
    }
  };

</script>

    <h1>TO DO LIST</h1>
    <img class="underline" src="{underline}" alt="squiggly line"/>
    <button type="menubutton" onclick={toggleSidebar}><img class="list-menu" src="{menu}" alt="menu" /></button>
    <h2>{currentList.name}</h2>
    

    <Sidebar
    isOpen={isSidebarOpen}
    onClose={closeSidebar}
    onCreateNewList={createNewList}
    onSelectList={selectList}
    {todoLists}
    {currentList}
    {currentListId}
    onUpdateListName={editListName}
    onDeleteList={deleteList}
  />

  <Todo todoList={currentList} onUpdateList={updateList}  />

<style>
    h1 {
      padding-top: 5vw;
    }
    h2 {
    padding-bottom: 3vw;
  }
    .underline {
        padding-bottom: 3vw;
        width: 35%;
        opacity: 30%;
    }
    button[type="menubutton"] {
      background: none; 
      border: none;
      padding: 12px; /* Adjust padding for better button appearance */
      cursor: pointer;
      outline: none;
      position: fixed;  /* Fix button in place */
      top: 10px;         /* Adjust the top distance */
      right: -340px;        /* Adjust the left distance */
      z-index: 1000;   
      }

  button[type="menubutton"]:focus {
  outline: none;
  }
  
.list-menu {
  width: 8%;
}
 /* Media Query */ 
 /* Load and update LocalStorage */
 @media (max-width: 768px) {
    h1 {
      font-size: 2.5rem; /* Smaller font size for screens 768px or smaller */
    }
    .underline {
      width: 50%;
    }
    h2 {
      font-size: 3rem;
    }
  }

  @media (max-width: 480px) {
    h1 {
      font-size: 2rem; /* Even smaller font size for screens 480px or smaller */
    }
  }
</style>