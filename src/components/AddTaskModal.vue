<template>
  <div class="fixed inset-0 flex items-center bg-opacity-75">
    <div class="bg-white mb-2 rounded-lg shadow-lg px-2">
      <h2 class="text-xl font-bold mb-4">{{ isEditing ? 'Edit Task' : 'Add Task' }}</h2>
      <form @submit.prevent="submitTask">
        <div class="mb-4">
          <label class="block text-gray-700">Name:</label>
          <input
            v-model="localTask.name"
            type="text"
            placeholder="Enter name here"
            class="form-input px-6 block w-full text-black placeholder-gray-500 bg-white border border-black"
            required
          />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700">Description:</label>
          <textarea
            v-model="localTask.description"
            placeholder="Enter Description here"
            class="form-input mt-1 px-6 block w-full text-black placeholder-gray-500 bg-white border border-black"
            required
          ></textarea>
        </div>
        <div class="mb-4">
          <label class="block text-gray-700">Assignee:</label>
          <input
            v-model="localTask.assignee"
            type="text"
            placeholder="Enter Assignee "
            class="form-input mt-1 bg-white border border-black px-3 block w-full"
            required
          />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700">Due Date:</label>
          <input
            v-model="localTask.dueDate"
            type="date"
            class="form-input mt-1 px-3 block w-full"
            required
          />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700">Status</label>
          <div class="relative mt-1">
            <select
              v-model="localTask.status"
              class="form-select mt-1 block w-full py-2 px-3 border border-sky-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
              required
            >
              <option>Backlog</option>
              <option>To Do</option>
              <option>Open</option>
              <option>Ready for Testing</option>
              <option>Feedback Needed</option>
            </select>
          </div>
        </div>

        <div class="mb-4">
          <label class="block text-gray-700">Spent Time:</label>
          <input
            v-model="localTask.spentTime"
            placeholder="Hours/Month/years"
            type="text"
            class="form-input mt-1 bg-white border border-black block w-full"
            required
          />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700">Priority</label>
          <select v-model="localTask.priority" class="form-select mt-1 block w-full" required>
            <option>High</option>
            <option>Medium</option>
            <option>Low</option>
          </select>
        </div>
        <div class="flex justify-end">
          <button type="button" @click="$emit('close')" class="btn btn-secondary mr-2 ml-3">
            Cancel
          </button>
          <button type="submit" class="btn btn-primary">
            {{ isEditing ? 'Update Task' : 'Add Task' }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    task: Object,
    isEditing: Boolean
  },
  data() {
    return {
      localTask: { ...this.task }
    }
  },
  methods: {
    submitTask() {
      this.$emit('add-task', { ...this.localTask })
      this.$emit('close')
    }
  },
  watch: {
    task(newTask) {
      this.localTask = { ...newTask }
    }
  }
}
</script>
