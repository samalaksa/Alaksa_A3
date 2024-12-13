<script>
  let { 
    isOpen = false,
    onClose = () => {},
    onCreateNewList = () => {},
    onSelectList = (listId) => {},
    todoLists = [],
    currentList, currentListId
  } = $props();

  // New state for handling the creation of a new list
  let newListName = $state('');  // Name of the new list
  let isCreatingNewList = $state(false); // Flag to show/hide the input form for creating a new list

  // Handle the action when user wants to create a new list
  const createList = () => {
    if (newListName.trim()) {
      onCreateNewList(newListName);
      newListName = ''; // Reset the input field
      isCreatingNewList = false; // Hide the input field after creating the list
    }
  };

  // Cancel creating a new list (resets the name input)
  const cancelCreateList = () => {
    newListName = '';
    isCreatingNewList = false; // Hide the input field
  };

</script>

<div class={`sidebar ${isOpen ? 'open' : ''}`}>
  <!-- Button to create a new list, toggle form display -->
  {#if !isCreatingNewList}
    <button onclick={() => isCreatingNewList = true}>Create New List</button>
  {/if}

  <!-- New list name input when creating a new list -->
  {#if isCreatingNewList}
    <div class="new-list-input">
      <input
        type="text"
        bind:value={newListName}
        placeholder="Enter list name"
        onkeydown={(e) => e.key === 'Enter' && createList()}
      />
      <button onclick={createList}>Save</button>
      <button onclick={cancelCreateList}>Cancel</button>
    </div>
  {/if}

  <h3>Existing Lists</h3>
  
  <!-- List of existing lists -->
  {#each todoLists as list}
    <button class="list-item" onclick={() => onSelectList(list.id)}>
      {list.name}
    </button>
  {/each}
  
  <!-- Button to close the sidebar -->
  <button onclick={onClose}>Close</button>
</div>

<style>
  h3 {
    font-family: "Emblema One", system-ui;
    font-weight: 400;
    font-style: normal;
  }

  .sidebar {
    position: fixed;
    top: 0;
    left: 0;
    width: 250px;
    height: 100%;
    background-color: #ffd4f2;
    box-shadow: 2px 0 5px rgba(0, 0, 0, 0.3);
    transform: translateX(-100%);
    transition: transform 0.3s ease;
    z-index: 100;
    padding: 20px;
    overflow-y: auto;
    font-family: "Fira Sans", sans-serif;
    font-weight: 400;
    font-style: normal;
  }

  .sidebar.open {
    transform: translateX(0);
  }

  .sidebar button {
    padding: 10px;
    margin-bottom: 20px;
    width: 100%;
    background-color: #DC7AC0;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s, box-shadow 0.3s;
  }

  .sidebar button:hover {
    background-color: #b65c9c;
  }

  .list-item {
    padding: 10px;
    cursor: pointer;
    background-color: #f9f9f9;
    border-radius: 5px;
    margin-bottom: 10px;
  }

  .list-item:hover {
    background-color: #f0f0f0;
  }

  /* Style for the new list input area */
  .new-list-input {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 20px;
  }

  .new-list-input input {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
    font-size: 14px;
  }
  .new-list-input button {
    background-color: #DC7AC0;
    color: white;
    padding: 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .new-list-input button:hover {
    background-color: #b65c9c;
  }
</style>