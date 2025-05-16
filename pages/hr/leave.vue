<template>
  <div>
    <h1 class="text-xl font-bold mb-4">Leave Management</h1>
    <table class="w-full text-left border">
      <thead>
        <tr>
          <th class="p-2">Employee</th>
          <th>Type</th>
          <th>Start</th>
          <th>End</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="leave in leaves" :key="leave.id">
          <td class="p-2">{{ leave.employee }}</td>
          <td>{{ leave.type }}</td>
          <td>{{ leave.start }}</td>
          <td>{{ leave.end }}</td>
          <td>
            <span :class="getStatusClass(leave.status)">{{ leave.status }}</span>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
const leaves = ref([
  { id: 1, employee: 'Chris A.', type: 'Annual', start: '2025-05-01', end: '2025-05-05', status: 'Approved' },
])

const getStatusClass = (status: string) => {
  return status === 'Approved' ? 'text-green-500' : 'text-yellow-500'
}
</script>
