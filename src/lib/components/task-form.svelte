<script lang="ts">
  let newTask = $state("");
  let { addTask }: { addTask: (newTask: string) => void } = $props();

  const onSubmit = (event: Event) => {
    event.preventDefault();
    if (!newTask) {
      alert("Task cannot be empty");
      return;
    }
    addTask(newTask);
    newTask = "";
  };
</script>

<form onsubmit={onSubmit}>
  <label for="task">Task</label>
  <input id="task" bind:value={newTask} />
  <div class="button-container">
    <button>Add Task</button>
  </div>
</form>

<style>
  form {
    margin-bottom: 1rem;
  }
  input {
    width: 100%;
    border: 1px solid #ddd;
    padding: 0.5rem;
    border-radius: 0.25rem;
  }
  .button-container {
    display: flex;
    justify-content: end;
    button {
      @apply bg-blue-600 p-2;
        
    }
  }
</style>
