<template>
  <div class="modal" :class="{ 'is-active': isActive }">
    <div class="modal-background"  v-on:click="close()"></div>
    <div class="modal-content">
      {{message.message}}
    </div>
    <button class="modal-close is-large" aria-label="close"></button>
  </div>
</template>

<script lang="ts" setup>
  import { ref } from 'vue'
  const props = defineProps(['message']);

  let isActive = ref(true);

  const close = () => {
    console.log(props.message)
    isActive.value = false;
  }
</script>

<style scoped lang="scss">
</style>