<template>
  <div>
    <!-- Шапка -->
    <header class="app-header text-center">
      <h1 class="fw-bold">Планировщик задач</h1>
      <p class="mb-0 opacity-75">Организуй свой день эффективно</p>
    </header>

    <div class="container">
      <!-- Статистика -->
      <div class="row mb-4">
        <div class="col-12 col-md-4 mb-3 mb-md-0">
          <div class="stats-block text-center">
            <div class="fs-2 fw-bold text-primary">{{ totalTasks }}</div>
            <div class="text-muted small">Всего задач</div>
          </div>
        </div>
        <div class="col-12 col-md-4 mb-3 mb-md-0">
          <div class="stats-block text-center">
            <div class="fs-2 fw-bold text-success">{{ doneTasks }}</div>
            <div class="text-muted small">Выполнено</div>
          </div>
        </div>
        <div class="col-12 col-md-4">
          <div class="stats-block text-center">
            <div class="fs-2 fw-bold text-warning">{{ pendingTasks }}</div>
            <div class="text-muted small">В работе</div>
          </div>
        </div>
      </div>

      <div class="row">
        <!-- Форма добавления + фильтр -->
        <div class="col-12 col-md-6 col-lg-4 mb-4">
          <TaskForm
            :categories="categories"
            @add-task="addTask"
          />

          <div class="form-panel mt-3">
            <h6 class="mb-2 text-md-start text-center">Фильтр</h6>
            <select v-model="filterCategory" class="form-select mb-2">
              <option value="">Все категории</option>
              <option v-for="cat in categories" :key="cat" :value="cat">{{ cat }}</option>
            </select>
            <select v-model="filterStatus" class="form-select">
              <option value="">Все статусы</option>
              <option value="active">В работе</option>
              <option value="done">Выполненные</option>
            </select>
          </div>

          <div class="mt-3 d-none d-md-block">
            <button class="btn btn-outline-danger w-100" @click="clearDone">
              Удалить выполненные
            </button>
          </div>
        </div>

        <!-- Список задач -->
        <div class="col-12 col-md-6 col-lg-8">
          <div v-if="filteredTasks.length === 0" class="empty-state">
            <div class="fs-1">📋</div>
            <p>Задач нет. Добавьте первую!</p>
          </div>

          <div class="row" v-else>
            <div
              v-for="(task, index) in filteredTasks"
              :key="task.id"
              class="col-12 col-md-6 col-lg-6"
            >
              <TaskCard
                :task="task"
                :index="getOriginalIndex(task)"
                @toggle-done="toggleDone"
                @remove-task="removeTask"
              />
            </div>
          </div>

          <div class="d-md-none mt-3">
            <button class="btn btn-outline-danger w-100" @click="clearDone">
              Удалить выполненные
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TaskCard from './components/TaskCard.vue'
import TaskForm from './components/TaskForm.vue'

export default {
  name: 'App',

  components: {
    TaskCard,
    TaskForm,
  },

  data() {
    return {
      filterCategory: '',
      filterStatus: '',
      categories: ['Работа', 'Учёба', 'Личное', 'Покупки', 'Здоровье'],
      tasks: [
        {
          id: 1,
          title: 'Сдать проект по Vue',
          description: 'Создать полноценное SPA-приложение с компонентами',
          priority: 'Высокий',
          category: 'Учёба',
          done: false,
          createdAt: '30.05.2026',
        },
        {
          id: 2,
          title: 'Купить продукты',
          description: 'Молоко, хлеб, яйца, овощи для ужина',
          priority: 'Средний',
          category: 'Покупки',
          done: false,
          createdAt: '31.05.2026',
        },
        {
          id: 3,
          title: 'Зарядка по утрам',
          description: 'Минимум 15 минут упражнений каждое утро',
          priority: 'Низкий',
          category: 'Здоровье',
          done: true,
          createdAt: '28.05.2026',
        },
        {
          id: 4,
          title: 'Прочитать документацию Vue',
          description: 'Изучить раздел Composition API и lifecycle hooks',
          priority: 'Средний',
          category: 'Учёба',
          done: false,
          createdAt: '29.05.2026',
        },
        {
          id: 5,
          title: 'Написать отчёт',
          description: 'Ежемесячный отчёт по проделанной работе для руководства',
          priority: 'Высокий',
          category: 'Работа',
          done: false,
          createdAt: '31.05.2026',
        },
        {
          id: 6,
          title: 'Позвонить другу',
          description: 'Договориться о встрече на выходных',
          priority: 'Низкий',
          category: 'Личное',
          done: true,
          createdAt: '27.05.2026',
        },
      ],
      nextId: 7,
    }
  },

  computed: {
    totalTasks() {
      return this.tasks.length
    },
    doneTasks() {
      return this.tasks.filter(t => t.done).length
    },
    pendingTasks() {
      return this.tasks.filter(t => !t.done).length
    },
    filteredTasks() {
      return this.tasks.filter(task => {
        const matchCategory = !this.filterCategory || task.category === this.filterCategory
        const matchStatus =
          !this.filterStatus ||
          (this.filterStatus === 'done' && task.done) ||
          (this.filterStatus === 'active' && !task.done)
        return matchCategory && matchStatus
      })
    },
  },

  methods: {
    addTask(task) {
      task.id = this.nextId++
      this.tasks.push(task)
    },
    removeTask(index) {
      this.tasks.splice(index, 1)
    },
    toggleDone(index) {
      this.tasks[index].done = !this.tasks[index].done
    },
    clearDone() {
      this.tasks = this.tasks.filter(t => !t.done)
    },
    getOriginalIndex(task) {
      return this.tasks.findIndex(t => t.id === task.id)
    },
  },
}
</script>
