<script lang="ts">
  import TaskForm from "./lib/components/task-form.svelte";
  import type { Task } from "./types";

  let message = "Hello, World!";
  let tasks = $state<Task[]>([]);

  const addTask = (newTask: string) => {
    console.log(newTask);
    tasks.push({
      id: crypto.randomUUID(),
      title: newTask,
      done: false,
    });
  };
</script>

<main>
  <h1>{message}</h1>
  <TaskForm {addTask} />
  <section>
    {#each tasks as task}
      <article>{task.title}</article>
    {/each}
  </section>
</main>

<style>
  main {
    margin: 1rem;
    max-width: 800px;
  }
</style>
