  <script setup lang="ts">
    import type { Task } from '~/types'
    import { nanoid } from 'nanoid';

    const emit = defineEmits<{
      (e: 'add', payload: Task): void;
    }>()

    const focused = ref(false)
    const title = ref('')

    function createTask(e: Event) {
      e.preventDefault()
      if(title.value.trim()) {
        emit('add', {
          title: title.value.trim(),
          createdAt: new Date(),
          id: nanoid()
        } as Task)
      }

      title.value = ''
      e.target.blur() as HTMLTextAreaElement
    }
  </script>

  <template>
    <div>
      <textarea 
        @keydown.tab="createTask"
        @keyup.enter="createTask"
        class="focus:bg-white bg-transparent focus:shadow 
        resize-none rounded w-full border-none"
        :class="{
                 'h-7': !focused,
                 'h-20': focused,
                 'p-2': focused
                 }"
        @focus="focused = true"
        @blur="focused = false"
        v-model="title"
        :placeholder="!focused ? '+ Add A Card' : 'Enter a title for this card'" />
    </div>
  </template>
