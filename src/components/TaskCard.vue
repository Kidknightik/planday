<template>
  <div
    class="card task-card mb-3"
    :class="{ completed: task.done }"
  >
    <div class="card-body">
      <div class="d-flex justify-content-between align-items-start">
        <h5 class="card-title mb-1">{{ task.title }}</h5>
        <span
          class="priority-badge"
          :class="priorityClass"
        >{{ task.priority }}</span>
      </div>

      <p class="card-text text-muted small mb-2">{{ task.description }}</p>

      <div class="d-flex align-items-center gap-2 mb-3">
        <span class="badge bg-secondary">{{ task.category }}</span>
        <span class="small text-muted d-none d-md-inline">
          Создано: {{ task.createdAt }}
        </span>
      </div>

      <div class="d-flex gap-2">
        <button
          class="btn btn-sm"
          :class="task.done ? 'btn-outline-secondary' : 'btn-success'"
          @click="toggleDone"
        >
          {{ task.done ? 'Возобновить' : 'Выполнено' }}
        </button>
        <button
          class="btn btn-sm btn-outline-danger"
          @click="removeTask"
        >
          Удалить
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TaskCard',

  props: {
    task: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
  },

  computed: {
    priorityClass() {
      const map = {
        Высокий: 'bg-danger text-white',
        Средний: 'bg-warning text-dark',
        Низкий: 'bg-success text-white',
      }
      return map[this.task.priority] || 'bg-secondary text-white'
    },
  },

  methods: {
    toggleDone() {
      this.$emit('toggle-done', this.index)
    },
    removeTask() {
      this.$emit('remove-task', this.index)
    },
  },
}
</script>
