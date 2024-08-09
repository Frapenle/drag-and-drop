<script setup>
import { ref } from 'vue'

const items = ref([
  {
    id: 0,
    title: 'Item A',
    list: 1
  },
  {
    id: 1,
    title: 'Item B',
    list: 1
  },
  {
    id: 2,
    title: 'Item C',
    list: 2
  },
]);

const getList = (list) => {
  return items.value.filter((item) => item.list == list)
}
const startDrag = (event, item) => {
  console.log(item)
  event.dataTransfer.dropEffect = 'move'
  event.dataTransfer.effectAllowed = 'move'
  event.dataTransfer.setData('itemID', item.id)
}
const onDrop = (event, list) => {
  const itemID = event.dataTransfer.getData('itemID')
  const item = items.value.find((item) => item.id == itemID)
  item.list = list
}

</script>

<template>
  <div class="drop-zone" @drop="onDrop($event, 1)" @dragenter.prevent @dragover.prevent>
    <div class="title">List A</div>
    <div v-for="item in getList(1)" :key="item.id" class="drag-el" draggable="true"
      @dragstart="startDrag($event, item)">
      {{ item.title }}
    </div>
  </div>

  <div div class="drop-zone" @drop="onDrop($event, 2)" @dragenter.prevent @dragover.prevent>
    <div class="title">List B</div>
    <div v-for="item in getList(2)" :key="item.id" class="drag-el" draggable="true"
      @dragstart="startDrag($event, item)">
      {{ item.title }}
    </div>
  </div>
</template>

<style scoped>
.drop-zone {
  width: 50%;
  margin: 50px auto;
  background-color: #fefefe;
  padding: 10px;
  min-height: 20px;
  box-shadow: rgba(0, 0, 0, 0.06) 0px 2px 4px 0px inset;
  position: relative;

  &>.title {
    position: absolute;
    top: -10px;
    background-color: #ffffff;
    padding: 0px 10px;
  }
}


.drag-el {
  width: 200px;
  background-color: #f03636;
  color: white;
  padding: 10px;
  margin-bottom: 10px;
}

.drag-el[draggable="true"] {
  cursor: grab;
}

.drag-el:active {
  cursor: grabbing;
}
</style>
