<script>
    import Todo from './Todo.svelte';
    import Sidebar from './Sidebar.svelte'
    import '../app.css';
    import underline from "$lib/img/underline.png"

    
// LIST MANAGER
//let storedList, storedId;
let currentListId = $state('');
let currentList = $state('');
let todoLists = $state([
    {id: 1, name: 'School', items: [] },
    {id: 2, name: 'Personal', items: []},
]);

/* Load and update LocalStorage
onMount(() => {
  storedList = localStorage.getItem('storedList');
  if (storedList) {
    console.log('there is storage!')
    currentList = (JSON.parse(storedList));
  } 
  storedId = localStorage.getItem('currentListId');
  if (storedId) {
    currentListId = storedId;
  } else {
    currentListId = 1;
  }
})
*/
$effect(() => {
    currentList = todoLists.find(list => list.id === currentListId)
  })
currentListId = 1;
$inspect('current list', currentList);
$inspect('current list items', currentList.items);

  let isSidebarOpen = $state(false);
   /*
  $effect(() => {
    if (currentListId) { localStorage.setItem('currentListId', currentListId)};
  })
   */


  const toggleSidebar = () => {
    isSidebarOpen = !isSidebarOpen;
  };

  const closeSidebar = () => {
    isSidebarOpen = false;
  };

  const createNewList = () => {
    const newList = { id: todoLists.length + 1, name: `New List ${todoLists.length + 1}`, items: [] };
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

</script>

    <h2>Self Care</h2>
    <img class="underline" src="{underline}" alt="trash"/>
    <h1>TO DO LIST</h1>
    <button onclick={toggleSidebar}>Open Sidebar</button>
    <h2>Current List: {currentList.name}</h2>
    

    <Sidebar
    isOpen={isSidebarOpen}
    onClose={closeSidebar}
    onCreateNewList={createNewList}
    onSelectList={selectList}
    {todoLists}
    {currentList}
    {currentListId}
  />

  <Todo todoList={currentList} onUpdateList={updateList}  />

<style>
    .underline {
        width: 35%;
        opacity: 30%;
    }
</style>