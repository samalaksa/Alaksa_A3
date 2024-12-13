<script>
  let { 
    isOpen = false,
    onClose = () => {},
    onCreateNewList = (newListName) => {},
    onSelectList = (listId) => {},
    todoLists = [],
    currentList, currentListId,
    onDeleteList = (listId) => {},
    onUpdateListName = (listId, newName) => {},
  } = $props();
  let newListName = $state('');
  let isCreatingNewList = $state(false);
  let isEditing = $state(false);
  let editListId = $state(null);
  let editListName = $state('');

  const createList = () => {
    if (newListName.trim()) {
      onCreateNewList(newListName);
      newListName = ''; 
      isCreatingNewList = false
    }
  };

  // Cancel button
  const cancelCreateList = () => {
    newListName = '';
    isCreatingNewList = false; 
  };
  // Toggle edit mode for a list
  const toggleEditList = (listId, name) => {
    editListId = listId;
    editListName = name;
    isEditing = true;
  };

  // Save the edited list name
  const saveEditListName = () => {
    if (editListName.trim()) {
      onUpdateListName(editListId, editListName);
      isEditing = false;
      editListId = null;
      editListName = '';
    }
  };

  // Cancel editing the list name
  const cancelEditList = () => {
    isEditing = false;
    editListId = null;
    editListName = '';
  };

  const deleteList = (listId) => {
    onDeleteList(listId);
  };
</script>

<div class={`sidebar ${isOpen ? 'open' : ''}`}>
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
    <div class="list-item">
      {#if isEditing && list.id === editListId}
      <div class="edit-mode">
        <input
        type="text"
        bind:value={editListName}
        onkeydown={(e) => e.key === 'Enter' && saveEditListName()}
      />
      <button onclick={saveEditListName}>Save</button>
          <button onclick={cancelEditList}>Cancel</button>
    </div>
  {:else}
  <div class="list-name">
    <button class="list-item-button" onclick={() => onSelectList(list.id)}>
      {list.name}
    </button>
    <button class="edit-button" onclick={() => toggleEditList(list.id, list.name)}>
      ✏️
    </button>
    <button class="delete-button" onclick={() => deleteList(list.id)}>
      🗑️
    </button>
  </div>
{/if}
</div>
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
  .edit-mode input {
    width: 80%;
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }

  .edit-mode button {
    padding: 5px 10px;
    margin-left: 10px;
    background-color: #DC7AC0;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .edit-mode button:hover {
    background-color: #b65c9c;
  }

  .list-item-button {
    background: none;
    border: none;
    font-size: 16px;
    color: #333;
    cursor: pointer;
  }

  .list-item-button:hover {
    text-decoration: underline;
  }

  .edit-button,
  .delete-button {
    background: none;
    border: none;
    font-size: 18px;
    cursor: pointer;
  }

  .edit-button:hover {
    color: #DC7AC0;
  }

  .delete-button:hover {
    color: red;
  }
</style>