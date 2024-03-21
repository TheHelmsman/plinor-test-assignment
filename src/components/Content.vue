<template>
  <div class="main-layout">
    <input
      type="text"
      :value="title"
      class="header"
      @input="(event) => onChange(event.target as HTMLInputElement)"
    />
    <textarea
      type="text"
      :value="content"
      class="text-area"
      cols="10"
      rows="1"
      @input="(event) => onContent(event.target as HTMLInputElement)"
    ></textarea>
    <span class="remove" @click="onClick">x</span>
  </div>
</template>

<script setup lang="ts">
interface IContent {
  title?: string
  content?: string
  index?: number
}

const props = defineProps<IContent>()

const emit = defineEmits<{
  (e: 'update:close', value: number): void
  (e: 'update:title', value: string): void
  (e: 'update:content', value: string): void
}>()

const onClick = () => {
  if (props.index) emit('update:close', props.index)
  console.log('onClick, idx: ', props.index)
}

const onChange = (data: HTMLInputElement) => {
  console.log('data: ', data.value)
  if (props.index) emit('update:title', data.value)
}

const onContent = (data: HTMLInputElement) => {
  console.log('data: ', data.value)
  if (props.index) emit('update:content', data.value)
}
</script>

<style scoped>
.main-layout {
  text-align: left;
  .header {
    background: transparent;
    border: none;
    width: auto;
    color: #3676a1;
  }
  .text-area {
    background: transparent;
    border: none;
    display: block;
    width: -webkit-fill-available;
    color: #3676a1;
  }
  .remove {
    position: absolute;
    right: 2px;
    top: -5px;
    cursor: pointer;
    color: #3676a1;
  }
}
</style>
