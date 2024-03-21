<template>
  <div class="main-layout">
    <div>{{ title }}</div>
    <GridLayout
      v-model:layout="layout"
      :col-num="12"
      :row-height="30"
      is-draggable
      is-resizable
      vertical-compact
      use-css-transforms
    >
      <template #item="{ item }">
        <Content
          :id="item.i"
          :title="item.name"
          :content="item.content"
          :index="getIndex(item.i)"
          @update:close="onClose($event)"
          @update:title="onTitleUpdate($event, item.i)"
          @update:content="onContentUpdate($event, item.i)"
        />
      </template>
    </GridLayout>
  </div>
  <button type="button" @click="addItem">Add Item</button>
</template>

<script setup lang="ts">
import { GridLayout } from 'grid-layout-plus'
import { ref, reactive } from 'vue'
import { testLayout } from './data'
import Content from './Content.vue'

let index = testLayout.length

const layout = reactive(testLayout)

const draggable = ref(true)
const resizable = ref(true)
const colNum = ref(12)

defineProps<{ title?: string }>()

const handleResize = () => {
  console.log('handle resize')
}

const onClose = (itemIdx: number) => {
  removeItem(itemIdx.toString())
}

const getIndex = (id: string): number => {
  return parseInt(id)
}

const onTitleUpdate = (data: string, idx: number) => {
  const index = layout.findIndex((item) => item.i === idx.toString())
  if (index > -1) layout[index].name = data
}

const onContentUpdate = (data: string, idx: number) => {
  const index = layout.findIndex((item) => item.i === idx.toString())
  if (index > -1) layout[index].content = data
}

function addItem() {
  let x = layout.length % 12
  let y = layout.length < 12 ? 0 : Math.floor(layout.length / 12)
  layout.push({
    x: x,
    y: y,
    w: 1,
    h: 2,
    i: `${index++}`,
    name: 'new item',
    content: 'Lorem Ipsum',
    resizable: true,
    draggable: true,
    static: false,
  })
}

function removeItem(id: string) {
  const index = layout.findIndex((item) => item.i === id)
  if (index > -1) layout.splice(index, 1)
}
</script>

<style scoped>
.vgl-layout {
  background-color: #eee;
}

.vgl-layout::before {
  position: absolute;
  left: 0px;
  width: calc(100% - 5px);
  height: calc(100% - 5px);
  margin: 5px;
  content: '';
  background-image: linear-gradient(to right, lightgrey 1px, transparent 1px),
    linear-gradient(to bottom, lightgrey 1px, transparent 1px);
  background-repeat: repeat;
  background-size: calc(calc(100% - 5px) / 12) 40px;
}

:deep(.vgl-item:not(.vgl-item--placeholder)) {
  background-color: #efefef;
  border: 1px solid #3676a1;
  border-radius: 10px;
}

:deep(.vgl-item--resizing) {
  opacity: 90%;
}

:deep(.vgl-item--static) {
  background-color: #cce;
}

.text {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  margin: auto;
  font-size: 24px;
  text-align: center;
}
</style>
