<template>
  <div
    class="bg-white rounded-lg shadow p-4 font-bold border: 2px dashed #ccc"
    @dragover.prevent
    @drop="handleDrop"
  >
    <h2 class="text-sm font-bold mb-3">{{ title }}: <br />( {{ tasks.length }} )</h2>
    <div>
      <TaskCard
        class="draggable-item"
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        @edit-task="$emit('edit-task', task)"
        @view-task="$emit('view-task', task)"
        draggable="true"
        @dragstart="handleDragStart(task)"
      />
    </div>
  </div>
</template>

<script>
import TaskCard from './TaskCard.vue'

export default {
  components: {
    TaskCard
  },
  props: {
    title: String,
    tasks: Array
  },
  methods: {
    handleDragStart(task) {
      event.dataTransfer.setData('task', JSON.stringify(task))
    },
    handleDrop(event) {
      event.preventDefault()
      const taskData = JSON.parse(event.dataTransfer.getData('task'))
      console.log('Dropped Task Data:', taskData)
      this.$emit('update-task-status', taskData.id, this.title.toLowerCase())
    }
  }
}
</script>

<style scoped>
.bg-white {
  background-color: white;
}
.draggable-item {
  cursor: grab;
}
.dragging {
  opacity: 0.6;
}
.drop-zone {
  background-color: #f0f0f0;
  border: 2px dashed #ccc;
}
</style>
