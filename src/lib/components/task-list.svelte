<script lang="ts">
  import { onMount } from "svelte";
  import type { Task } from "../../types";
  import { fade } from "svelte/transition";

  let ref = $state<HTMLInputElement | null>(null);
  let editingTask: Task | null = $state(null);
  let {
    tasks = $bindable(),
    toggleDone,
    deleteTask,
    editTask,
  }: {
    tasks: Task[];
    toggleDone: (task: Task) => void;
    deleteTask: (task: Task) => void;
    editTask: (task: Task) => void;
  } = $props();

  const onEditTask = () => {
    if (!editingTask) return;
    editTask(editingTask);
    editingTask = null;
  };

  $effect(() => {
    ref?.focus();
    ref?.select();
  });
</script>

<section>
  {#each tasks as task}
    <article
      class="flex gap-2 items-center justify-between"
      transition:fade={{ duration: 200 }}
    >
      <div class="flex items-center gap-x-2">
        <label class="!mb-0">
          <input
            type="checkbox"
            onchange={() => toggleDone(task)}
            checked={task.done}
          />
        </label>

        {#if editingTask?.id !== task.id}
          <button
            class="border-none focus:!ring-0"
            class:done={task.done}
            onclick={() => {
              editingTask = $state.snapshot(task);
            }}
          >
            {task.title}
          </button>
        {:else}
          <input
            bind:this={ref}
            bind:value={editingTask.title}
            class="!h-6 !mb-0 !pl-2"
            onblur={onEditTask}
            onkeydown={(e) => {
              if (e.key === "Enter") onEditTask();
            }}
          />
        {/if}
      </div>
      <button class="delete" onclick={() => deleteTask(task)}>Delete</button>
    </article>
  {/each}
</section>

<style>
  .done {
    text-decoration: line-through;
  }

  .delete {
    background-color: #ef4444;
    color: white;
    padding: 5px 10px;
    border-radius: 5px;
    border: none;
    font-size: 12px;
  }
</style>
