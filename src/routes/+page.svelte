<script>
    import Todo from './Todo.svelte';
    import Sidebar from './Sidebar.svelte'
    import '../app.css';
    import underline from "$lib/img/underline.png"

    let todoLists = [
        {id: 'list1', name: 'School', items: [] },
        {id: 'list2', name: 'Personal', items: [] },
    ];
    let currentList = todoLists[0];
    let isSidebarOpen = false;


  const toggleSidebar = () => {
    isSidebarOpen = !isSidebarOpen;
  };

  const closeSidebar = () => {
    isSidebarOpen = false;
  };

  const createNewList = () => {
    const newList = { id: Date.now().toString(), name: `New List ${todoLists.length + 1}`, items: [] };
    todoLists = [...todoLists, newList];
    currentList = newList;
  };

  const selectList = (listId) => {
    currentList = todoLists.find((list) => list.id === listId);
    closeSidebar();
  };

  const updateList = (updatedList) => {
    todoLists = todoLists.map((list) =>
      list.id === currentList.id ? { ...list, items: updatedList } : list
    );
  };

</script>

    <h2>Self Care</h2>
    <img class="underline" src="{underline}" alt="trash"/>
    <h1>TO DO LIST</h1>
    <button on:click={toggleSidebar}>Open Sidebar</button>

    <Sidebar
    isOpen={isSidebarOpen}
    onClose={closeSidebar}
    onCreateNewList={createNewList}
    onSelectList={selectList}
    {todoLists}
  />

<Todo todoList={currentList.items} onUpdateList={updateList} />

<style>
    .underline {
        width: 35%;
        opacity: 30%;
    }
</style>