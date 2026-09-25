<script setup>
import { ref } from 'vue'
import User from './components/User.vue'
import ClockManager from './components/ClockManager.vue'
import WorkingTimes from './components/WorkingTimes.vue'
import WorkingTime from './components/WorkingTime.vue'
import ChartManager from './components/ChartManager.vue'

const currentUser = ref(null)
const workingTimesRef = ref(null)
const chartManagerRef = ref(null)

function refreshAll() {
  workingTimesRef.value?.getWorkingTimes()
  chartManagerRef.value?.loadData()
}

async function handleDelete(id) {
  await fetch(`http://localhost:4000/api/workingtime/${id}`, { method: 'DELETE' })
  refreshAll()
}
</script>

<template>
  <div id="app">
    <h1>Time Manager</h1>

    <User v-model="currentUser" />

    <div v-if="currentUser">
      <ClockManager :userId="currentUser.id" />
      <WorkingTime :userId="currentUser.id" @saved="refreshAll" />
      <WorkingTimes ref="workingTimesRef" :userId="currentUser.id" @delete-clicked="handleDelete" />
      <ChartManager ref="chartManagerRef" :userId="currentUser.id" />
    </div>
  </div>
</template>

<style scoped>
</style>