<script setup lang="ts">
  import type { Task, ID } from '@/types'

  const props = defineProps<{
    task: Task 
  }>()
  const emit = defineEmits<{
    (e: 'delete', payload: ID): void
  }>()

  const focused = ref(false)

  onKeyStroke('Delete', (e) => {
    if(focused.value) emit('delete', props.task.id) 
  })

  const formattedDate = (date: Date) => {
    return useDateFormat(date, 'DD/MM/YYYY').value
  } 
</script>

<template>
  <div class="task bg-white p-2 mb-2 rounded shadow-sm
    max-w-[250px] flex items-center"
    @focus="focused = true"
    @blur="focused = false"
    tabindex="0">

    <DragHandle />
    <div class="block">
      <p> {{task.title}} </p>
      <div class="text-xs text-gray-400 
        font-bold mt-1">
        {{formattedDate(task.createdAt)}} 
      </div>
    </div>
  </div>
</template>

<style>
  .task:focus,
  .task:focus-visible {
    outline: gray solid 1px;
  }
</style>
