<script setup lang="ts">
  import type { Column, Task } from '~/types'
  import { nanoid } from 'nanoid';
  import draggable from 'vuedraggable';

  const columns = useLocalStorage<Column[]>("trelloBoard", [
    {
      id: nanoid(),
      title: 'Todo',
      tasks: []
    },
    {
      id: nanoid(),
      title: 'In Progress',
      tasks: []
    },
    {
      id: nanoid(),
      title: 'Backlog',
      tasks: [
        {
          id: nanoid(),
          title: 'Create landing page',
          createdAt: new Date()
        },
        {
          id: nanoid(),
          title: 'Add payment system',
          createdAt: new Date()
        }
      ]
    },
    {
      id: nanoid(),
      title: 'Complete',
      tasks: []
    }
  ])

  const alt = useKeyModifier('Alt')

  const titleRef = ref(null)

  const createColumn = () => {
    const column: Column = {
      id: nanoid(),
      title: '',
      tasks: []
    }

    columns.value.push(column)
    nextTick(() => {
      if(titleRef.value.name === column.id) {
          titleRef.value.focus()
      }
    })
  }

  const handleColumn = (id: string) => {
    columns.value = columns.value.filter(col => col.id !== id)
  } 
</script>

<template>

  <div class="flex items-start overflow-x-auto gap-4">
    <draggable v-model="columns"
     :animation="150" handle=".drag-handle"
     group="columns" item-key="id"
     class="flex gap-4 overflow-x-auto items-start">

    <template #item="{element: column}: { element: Column }">

      <div class="column bg-gray-200 p-5 rounded min-w-[250px] relative">
        <header class="font-bold mb-4">
          <DragHandle /> 

          <input type="text"
           class="bg-transparent focus:bg-white rounded px-1 w-4/5 capitalize"
           @keyup.enter="($event.target as HTMLInputElement).blur()"
           v-model="column.title"
           :name="column.id"
           ref="titleRef" />

          <Icon name="uil:times" 
           class="absolute right-4 top-4 cursor-pointer text-gray-400"
           @click="handleColumn(column.id)" />
        </header>

        <draggable 
         v-model="column.tasks"
         :group="{name: 'tasks', pull: alt ? 'clone' : true }" 
         :animation="150" handle=".drag-handle"
         item-key="id">

          <template #item="{element: task}: { element: Task }">
            <TrelloBoardTask :task="task"
             @delete="column.tasks = column.tasks.filter(t => t.id !== $event)" />
          </template>
        </draggable>

        <footer>
          <newTask @add="column.tasks.push($event)" />
        </footer>
      </div>
    </template>
   </draggable>

   <button
     class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50"
     @click="createColumn">
      + Add Another Column
   </button>

  </div>
</template>

<style>
  .sortable-ghost {
    opacity: .5;
    background: rgba(0 0 0 /.1)
  }
</style>
