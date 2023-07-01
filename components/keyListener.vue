<script setup lang="ts">


  const keys = ref([])

  function handleSave (e) {
    if(keys.value.length <= 5) {
      keys.value.push(e.key)
    } else {
      keys.value.splice(0, 5)
    }
  }

  onMounted(() => window.addEventListener('keydown', handleSave))
  onBeforeUnmount(() => window.removeEventListener('keydown', handleSave));
</script>

<template>
  
  <div class="absolute bottom-12 right-32
    bg-black p-4 text-white text-3xl"
    :class="{'hidden': !keys.length}">
    {{keys.join(' ')}}
  </div>
</template>
