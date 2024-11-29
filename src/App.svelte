<script lang="ts">
  import TaskForm from "./lib/components/task-form.svelte";
  import TaskList from "./lib/components/task-list.svelte";
  import type { Task, Filter } from "./types";

  let currentFilter = $state<Filter>("all");
  let tasks = $state<Task[]>([]);
  let filteredTasks = $derived.by(() => {
    if (currentFilter === "all") return tasks;
    return tasks.filter((task) => task.done === (currentFilter === "done"));
  });
  let taskCompleted = $derived.by(() =>
    filteredTasks.reduce((acc, task) => acc + Number(task.done), 0)
  );
  const addTask = (newTask: string) => {
    tasks.push({
      id: crypto.randomUUID(),
      title: newTask,
      done: false,
    });
  };

  const toggleDone = (task: Task) => {
    task.done = !task.done;
  };

  const deleteTask = (task: Task) => {
    tasks.splice(tasks.indexOf(task), 1);
  };

  const editTask = (editingTask: Task) => {
    const findIndex = tasks.findIndex((task) => task.id === editingTask.id);
    tasks[findIndex].title = editingTask.title;
  };
</script>

<main>
  <TaskForm {addTask} />

  {#if tasks.length > 0}
    <div class="flex justify-between items-center my-4">
      <p>{taskCompleted} / {filteredTasks.length} tasks completed</p>
      <div>
        <button class:contrast={currentFilter === "all"} class="secondary" onclick={() => (currentFilter = "all")}
          >All</button
        >
        <button class:contrast={currentFilter === "todo"} class="secondary" onclick={() => (currentFilter = "todo")}
          >Todo</button
        >
        <button class:contrast={currentFilter === "done"} class="secondary" onclick={() => (currentFilter = "done")}
          >Done</button
        >
      </div>
    </div>
  {:else}
    <div class="empty-task">
      <p>Add a task to get started.</p>
    </div>
  {/if}

  <TaskList
    tasks={filteredTasks}
    {toggleDone}
    {deleteTask}
    {editTask}
  />

  <pre>{JSON.stringify(tasks, null, 2)}</pre>
</main>

<style>
  main {
    margin: 1rem;
    max-width: 800px;
  }

  .empty-task {
    height: 300px;
    display: flex;
    align-items: center;
    justify-content: center;
    p {
      color: #999;
    }
  }

  .contrast {
    background-color: #1f2937;
  }
</style>
