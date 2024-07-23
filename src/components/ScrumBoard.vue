<template>
  <div class="container mx-auto p-2">
    <div class="btn-group" role="group">
      <button @click="openAddTaskModal" class="btn btn-info mb-4 px-1 mr-2">Add Task</button>
      <button @click="exportTasks" class="btn btn-warning mb-4 px-1">Export Tasks</button>
    </div>
    <AddTaskModal
      v-if="showModal"
      @close="closeModal"
      @add-task="addTask"
      :task="currentTask"
      :is-editing="isEditing"
    />
    <div class="row flex flex-wrap gap-2">
      <div class="col-md-2">
        <div class="backlog-column">
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Search tasks"
            class="search-input mb-2"
          />
          <ColumnBoard
            title="Backlog"
            :tasks="filteredBacklogTasks"
            @edit-task="editTask"
            @view-task="viewTask"
            @update-task-status="updateTaskStatus"
          />
        </div>
      </div>
      <div class="col-md-2">
        <ColumnBoard
          title="Open"
          :tasks="tasks.open"
          @edit-task="editTask"
          @view-task="viewTask"
          @update-task-status="updateTaskStatus"
        />
      </div>
      <div class="col-md-2">
        <ColumnBoard
          title="To Do"
          :tasks="tasks.todo"
          @edit-task="editTask"
          @view-task="viewTask"
          @update-task-status="updateTaskStatus"
        />
      </div>
      <div class="col-md-2">
        <ColumnBoard
          title="Ready for Testing"
          :tasks="tasks.readyfortesting"
          @edit-task="editTask"
          @view-task="viewTask"
          @update-task-status="updateTaskStatus"
        />
      </div>
      <div class="col-md-2">
        <ColumnBoard
          title="Feedback Needed"
          :tasks="tasks.feedbackneeded"
          @edit-task="editTask"
          @view-task="viewTask"
          @update-task-status="updateTaskStatus"
        />
      </div>
    </div>
    <TaskDetails
      class="mt-2"
      v-if="showTaskDetails"
      :task="currentTask"
      @close="showTaskDetails = false"
    />
    <input type="file" @change="importTasks" class="mt-4 px-2" />
  </div>
</template>

<script>
import ColumnBoard from './ColumnBoard.vue'
import AddTaskModal from './AddTaskModal.vue'
import TaskDetails from './TaskDetails.vue'

export default {
  components: {
    ColumnBoard,
    AddTaskModal,
    TaskDetails
  },
  data() {
    return {
      showModal: false,
      showTaskDetails: false,
      currentTask: null,
      isEditing: false,
      searchQuery: '',
      tasks: JSON.parse(localStorage.getItem('tasks')) || {
        backlog: [],
        open: [],
        todo: [],
        inProgress: [],
        feedbackneeded: [],
        readyfortesting: [],
        done: []
      }
    }
  },
  computed: {
    filteredBacklogTasks() {
      return this.tasks.backlog.filter((task) =>
        task.name.toLowerCase().includes(this.searchQuery.toLowerCase())
      )
    }
  },
  methods: {
    openAddTaskModal() {
      this.currentTask = {
        id: Date.now(),
        name: '',
        description: '',
        assignee: '',
        dueDate: '',
        status: 'Backlog',
        spentTime: '',
        priority: 'Medium'
      }
      this.isEditing = false
      this.showModal = true
    },
    closeModal() {
      this.showModal = false
      this.currentTask = null
      this.isEditing = false
    },
    addTask(task) {
      if (this.isEditing) {
        this.updateTask(task)
      } else {
        this.tasks.backlog.unshift(task)
      }
      this.saveTasks()
      this.closeModal()
    },
    editTask(task) {
      this.currentTask = { ...task }
      this.showModal = true
      this.isEditing = true
    },
    viewTask(task) {
      this.currentTask = task
      this.showTaskDetails = true
    },
    updateTask(updatedTask) {
      const taskListKeys = Object.keys(this.tasks)
      taskListKeys.forEach((key) => {
        this.tasks[key] = this.tasks[key].map((task) =>
          task.id === updatedTask.id ? updatedTask : task
        )
      })
      this.saveTasks()
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    exportTasks() {
      const dataStr =
        'data:text/json;charset=utf-8,' + encodeURIComponent(JSON.stringify(this.tasks))
      const downloadAnchorNode = document.createElement('a')
      downloadAnchorNode.setAttribute('href', dataStr)
      downloadAnchorNode.setAttribute('download', 'tasks.json')
      document.body.appendChild(downloadAnchorNode)
      downloadAnchorNode.click()
      downloadAnchorNode.remove()
    },
    importTasks(event) {
      const file = event.target.files[0]
      const reader = new FileReader()
      reader.onload = (e) => {
        const importedTasks = JSON.parse(e.target.result)

        for (const [status, tasksArray] of Object.entries(importedTasks)) {
          if (Array.isArray(tasksArray)) {
            if (this.tasks[status]) {
              this.tasks[status] = [...this.tasks[status], ...tasksArray]
            } else {
              this.tasks[status] = tasksArray
            }
          }
        }
        this.saveTasks()
      }
      reader.readAsText(file)
    },
    updateTaskStatus(taskId, newStatus) {
      const taskListKeys = Object.keys(this.tasks)
      taskListKeys.forEach((key) => {
        const taskIndex = this.tasks[key].findIndex((task) => task.id === taskId)
        if (taskIndex !== -1) {
          const updatedTask = { ...this.tasks[key][taskIndex], status: newStatus }
          this.tasks[key].splice(taskIndex, 1)
          const normalizedStatusKey = newStatus.replace(/\s/g, '').toLowerCase()

          if (!this.tasks[normalizedStatusKey]) {
            this.tasks[normalizedStatusKey] = []
          }
          this.tasks[normalizedStatusKey].push(updatedTask)
        }
      })
      this.saveTasks()
    }
  },
  watch: {
    tasks: {
      handler: 'saveTasks',
      deep: true
    }
  }
}
</script>

<style scoped>
.col-md-2 {
  padding: 2px 2px;
}

.search-input {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 0.25rem;
}

.backlog-column {
  position: relative;
}
</style>
