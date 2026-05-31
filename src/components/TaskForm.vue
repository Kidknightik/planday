<template>
  <div class="form-panel">
    <h5 class="mb-3 text-md-start text-center">Новая задача</h5>

    <div class="mb-3">
      <input
        v-model="newTitle"
        type="text"
        class="form-control"
        placeholder="Название задачи"
      />
    </div>

    <div class="mb-3">
      <textarea
        v-model="newDescription"
        class="form-control"
        rows="2"
        placeholder="Описание задачи"
      ></textarea>
    </div>

    <div class="mb-3">
      <select v-model="newPriority" class="form-select">
        <option value="">Выберите приоритет</option>
        <option value="Высокий">Высокий</option>
        <option value="Средний">Средний</option>
        <option value="Низкий">Низкий</option>
      </select>
    </div>

    <div class="mb-3">
      <select v-model="newCategory" class="form-select">
        <option value="">Выберите категорию</option>
        <option v-for="cat in categories" :key="cat" :value="cat">{{ cat }}</option>
      </select>
    </div>

    <div v-if="errorMessage" class="alert alert-danger py-2 small">
      {{ errorMessage }}
    </div>

    <button class="btn btn-primary w-100" @click="submitTask">
      Добавить задачу
    </button>
  </div>
</template>

<script>
export default {
  name: 'TaskForm',

  props: {
    categories: {
      type: Array,
      required: true,
    },
  },

  data() {
    return {
      newTitle: '',
      newDescription: '',
      newPriority: '',
      newCategory: '',
      errorMessage: '',
    }
  },

  methods: {
    submitTask() {
      if (!this.newTitle.trim()) {
        this.errorMessage = 'Введите название задачи'
        return
      }
      if (!this.newPriority) {
        this.errorMessage = 'Выберите приоритет'
        return
      }
      if (!this.newCategory) {
        this.errorMessage = 'Выберите категорию'
        return
      }

      this.errorMessage = ''

      const task = {
        title: this.newTitle.trim(),
        description: this.newDescription.trim() || 'Без описания',
        priority: this.newPriority,
        category: this.newCategory,
        done: false,
        createdAt: new Date().toLocaleDateString('ru-RU'),
      }

      this.$emit('add-task', task)

      this.newTitle = ''
      this.newDescription = ''
      this.newPriority = ''
      this.newCategory = ''
    },
  },
}
</script>
