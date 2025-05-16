<template>
  <div class="space-y-4">
    <h1 class="text-xl font-bold">Payroll</h1>
    <div class="bg-white rounded-xl p-4 shadow">
      <div class="grid grid-cols-4 font-bold border-b pb-2">
        <span>Name</span><span>Month</span><span>Gross</span><span>Net</span>
      </div>
      <div v-for="pay in payrolls" :key="pay.id" class="grid grid-cols-4 border-b py-2">
        <span>{{ pay.name }}</span>
        <span>{{ pay.month }}</span>
        <span>${{ pay.gross }}</span>
        <span>${{ pay.net }}</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const payrolls = ref([
  { id: 1, name: 'Chris A.', month: 'May 2025', gross: 3000, net: 2500 },
])
</script>
