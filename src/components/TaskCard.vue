<template>
  <div :class="priorityClass + ' p-2 rounded-lg mb-2'">
    <h3 class="text-base font-semibold">{{ task.name }}</h3>
    <p class="text-sm mb-2">{{ task.description }}</p>
    <p class="text-xs mb-1"><strong>Assignee:</strong> {{ task.assignee }}</p>
    <p class="text-xs mb-1"><strong>Due Date:</strong> {{ task.dueDate }}</p>
    <p class="text-xs mb-1"><strong>Status:</strong> {{ task.status }}</p>
    <p class="text-xs mb-1"><strong>Spent Time:</strong> {{ task.spentTime }}</p>
    <p class="text-xs mb-1"><strong>Priority:</strong> {{ task.priority }}</p>
    <button @click="$emit('edit-task', task)" class="btn btn-xs btn-warning">Edit</button>
    <button @click="$emit('view-task', task)" class="btn btn-xs btn-info">View</button>
  </div>
</template>

<script>
export default {
  props: {
    task: Object
  },
  computed: {
    priorityClass() {
      switch (this.task.priority) {
        case 'High':
          return 'bg-sky-100'
        case 'Medium':
          return 'bg-orange-100'
        case 'Low':
          return 'bg-pink-100'
        default:
          return 'bg-gray-100'
      }
    }
  }
}
</script>

<style scoped>
.bg-sky-100 {
  background-color: skyblue;
}
.bg-orange-100 {
  background-color: rgb(248, 219, 187);
}
.bg-pink-100 {
  background-color: palevioletred;
}
.bg-gray-100 {
  background-color: gray;
}
.text-xs {
  font-size: 0.75rem;
}
.text-sm {
  font-size: 0.875rem;
}
.btn-xs {
  font-size: 0.75rem;
  padding: 0.25rem 0.5rem;
}
</style>
