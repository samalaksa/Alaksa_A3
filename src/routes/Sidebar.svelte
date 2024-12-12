<script>
let { 
  isOpen = false,
  onClose = () => {},
  onCreateNewList = () => {},
  onSelectList = (listId) => {},
  todoLists = [],
  currentList, currentListId
} = $props();

  </script>
  
  <div class={`sidebar ${isOpen ? 'open' : ''}`}>
    <!-- Button to create a new list -->
    <button onclick={onCreateNewList}>Create New List</button>
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
    .sidebar {
      position: fixed;
      top: 0;
      left: 0;
      width: 250px;
      height: 100%;
      background-color: #f0f0f0;
      box-shadow: 2px 0 5px rgba(0, 0, 0, 0.3);
      transform: translateX(-100%);
      transition: transform 0.3s ease;
      z-index: 100;
      padding: 20px;
      overflow-y: auto;
    }
  
    .sidebar.open {
      transform: translateX(0);
    }
  
    .sidebar button {
      padding: 10px;
      margin-bottom: 20px;
      width: 100%;
      background-color: #3b82f6;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
  
    .sidebar button:hover {
      background-color: #2563eb;
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
  </style>