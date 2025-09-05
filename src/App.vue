<script setup>
import { ref, computed } from 'vue'

const tasks = ref([
  { text: 'Rasen mähen', done: false, high_priority: false, hours: 2 },
  { text: 'Französisch lernen', done: false, high_priority: true, hours: 1 },
  { text: 'Einkaufen', done: false, high_priority: true, hours: 5 },
  { text: 'Abfall rausbringen', done: false, high_priority: true, hours: 4 },
])

const Text = ref('')
const Prio = ref(false)
const Hours = ref(0)

function addTask() {
  if (!Text.value) return
  tasks.value.push({
    text: Text.value,
    done: false,
    high_priority: Prio.value,
    hours: Hours.value
  })
  Text.value = ''
  Prio.value = false
  Hours.value = 0
}

function deleteTask(index) {
  tasks.value.splice(index, 1)
}

const sortedTasks = computed(() => {
  return [...tasks.value].sort((a, b) => b.high_priority - a.high_priority)
})

const doneCount = computed(() => tasks.value.filter(t => t.done).length)
const openHours = computed(() =>
    tasks.value.filter(t => !t.done).reduce((sum, t) => sum + t.hours, 0)
)
</script>

<template>
  <div class="container">
    <h2>Aufgabenliste</h2>

    <p>{{ doneCount }} von {{ tasks.length }} Tasks sind erledigt, {{ openHours }} h Aufwand offen</p>

    <input v-model="newTaskText" placeholder="Task eingeben...">
    <input type="checkbox" v-model="newTaskPrio"> Hohe Priorität
    <input type="number" v-model.number="newTaskHours" placeholder="Stunden">
    <button @click="addTask">Hinzufügen</button>

    <ul>
      <li v-for="(task, index) in sortedTasks" :key="index"
          :class="{ 'high-priority': task.high_priority, 'is-done': task.done }">
        <input type="checkbox" v-model="task.done"> ca. {{ task.hours }}h {{ task.text }}
        <button @click="deleteTask(index)">Löschen</button>
      </li>
    </ul>
  </div>
</template>

<style>
.is-done {
  text-decoration: line-through;
}

.high-priority {
  font-weight: bold;
  color: red;
}

.form-group {
  display: block;
}

.form-group label {
  display: block;
  margin-bottom: 2px;
}

.form-control {
  width: 100%;
  padding: 2px 5px;
  height: 32px;
  margin-bottom: 5px;
}

.form-actions {
  display: block;
  margin-top: 1rem;
  margin-bottom: 2rem;
}

.container {
  margin: 20px auto;
  max-width: 400px;
  width: 100%;
}
</style>