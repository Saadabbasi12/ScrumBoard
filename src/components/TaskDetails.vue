<template>
  <div class="fixed inset-0 flex items-center justify-center z-50 bg-gray-800 bg-opacity-75">
    <div class="bg-white rounded-lg shadow-lg p-4 max-w-sm">
      <h2 class="text-lg font-semibold mb-2 border-b pb-2 text-red-600"><b>Task Details</b></h2>
      <div class="divide-y divide-gray-300">
        <p class="py-2">
          <strong class="font-semibold">Name:</strong>
          <span class="text-gray-800 px-2">{{ task.name }}</span>
        </p>
        <p class="py-2">
          <strong class="font-semibold">Description:</strong>
          <span class="text-gray-800 px-2">{{ task.description }}</span>
        </p>
        <p class="py-2">
          <strong class="font-semibold">Assignee:</strong>
          <span class="text-gray-800 px-2">{{ task.assignee }}</span>
        </p>
        <p class="py-2">
          <strong class="font-semibold">Due Date:</strong>
          <span class="text-gray-800 px-2">{{ task.dueDate }}</span>
        </p>
        <p class="py-2">
          <strong class="font-semibold">Status:</strong>
          <span class="text-gray-800 px-2">{{ task.status }}</span>
        </p>
        <p class="py-2">
          <strong class="font-semibold">Spent Time:</strong>
          <span class="text-gray-800 px-2">{{ task.spentTime }}</span>
        </p>
        <p class="py-2">
          <strong class="font-semibold">Priority:</strong>
          <span class="text-gray-800 px-2">{{ task.priority }}</span>
        </p>
      </div>
      <div class="flex justify-end pt-4">
        <button
          @click="$emit('close')"
          class="btn btn-info hover:bg-sky-300 text-black px-3 py-1 rounded-lg font-semibold transition-colors duration-300"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    task: Object
  }
}
</script>

<style scoped>
.bg-gray-800 {
  background-color: rgba(31, 41, 55, 0.8);
}
</style>
