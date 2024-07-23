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
            placeholder="Enter Assignee"
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
            :min="todayDate"
            required
          />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700">Spent Time:</label>
          <input
            v-model="localTask.spentTime"
            placeholder="Enter time in hours"
            type="text"
            class="form-input mt-1 bg-white border border-black block w-full"
            required
            @input="formatSpentTime"
          />
          <p v-if="localTask.spentTime">
            {{ formattedSpentTime }}
          </p>
        </div>
        <div class="mb-4">
          <label class="block text-gray-700">Priority:</label>
          <select
            v-model="localTask.priority"
            class="form-select mt-1 block w-full p-2 border border-gray-300 rounded-md"
            required
          >
            <option value="High">High</option>
            <option value="Medium">Medium</option>
            <option value="Low">Low</option>
          </select>
        </div>
        <div class="flex justify-end space-x-4">
          <button type="button" @click="$emit('close')" class="btn btn-secondary">Cancel</button>
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
      localTask: { ...this.task },
      todayDate: new Date().toISOString().split('T')[0]
    }
  },
  computed: {
    formattedSpentTime() {
      return this.localTask.spentTime ? `${parseFloat(this.localTask.spentTime)} hr/s` : ''
    }
  },
  methods: {
    submitTask() {
      this.localTask.spentTime = parseFloat(this.localTask.spentTime) || 0
      this.$emit('add-task', { ...this.localTask })
      this.$emit('close')
    },
    formatSpentTime(event) {
      let value = event.target.value

      value = value.replace(/[^0-9.]/g, '')

      this.localTask.spentTime = value
    }
  },
  watch: {
    task(newTask) {
      this.localTask = { ...newTask }
    }
  }
}
</script>

<style scoped>
.form-input {
  padding: 0.5rem;
  border-radius: 0.25rem;
}
.btn {
  padding: 0.5rem 1rem;
}
.space-x-4 > * + * {
  margin-left: 1rem;
}
</style>
