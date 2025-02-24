<template>
  <q-page class="q-pa-md">
    <q-toolbar>
      <q-toolbar-title>Gestion de tâche</q-toolbar-title>
    </q-toolbar>

    <q-list bordered class="q-mt-md">
      <q-item v-for="task in tasks" :key="task.id" clickable>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section side>
          <q-btn icon="delete" color="red" @click.stop="deleteTask(task.id)" />
        </q-item-section>
      </q-item>
    </q-list>

    <q-input
      v-model="newTask"
      @keyup.enter="addTask"
      label="Nouvelle tâche"
      placeholder="Ajouter une tâche"
      class="q-mt-md"
    />
    <q-btn label="Ajouter" color="primary" @click="addTask" class="q-mt-md" />
  </q-page>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

const tasks = ref([])
const newTask = ref('')

onMounted(() => {
  tasks.value = JSON.parse(localStorage.getItem('tasks')) || []
})

watch(tasks, (newTasks) => {
  localStorage.setItem('tasks', JSON.stringify(newTasks))
}, { deep: true })

const addTask = () => {
  tasks.value.push({ id: tasks.value.length + 1, title: newTask.value })
  newTask.value = ''
}

const deleteTask = (id) => {
  tasks.value = tasks.value.filter((task) => task.id !== id)
}
</script>
