<template>
  <div class="space-y-6">
    <h1 class="text-2xl font-bold mb-4">Generate Attendance QR Code</h1>
    <form @submit.prevent="generateQR" class="flex flex-col gap-4 max-w-md">
      <div>
        <label class="block mb-1">Branch</label>
        <select
          v-model="branch"
          class="w-full border rounded px-3 py-2"
          required
        >
          <option v-for="b in branches" :key="b" :value="b">{{ b }}</option>
        </select>
      </div>
      <div>
        <label class="block mb-1">Date</label>
        <input
          v-model="date"
          type="date"
          class="w-full border rounded px-3 py-2"
          required
        />
      </div>
      <button class="action-btn add self-start" type="submit">
        Generate QR Code
      </button>
    </form>
    <div v-if="qrValue" class="mt-6 flex flex-col items-center gap-2">
      <qrcode-vue
        :value="qrValue"
        :size="220"
        background="#fff"
        foreground="#2563eb"
      />
      <div class="text-gray-500 text-sm">
        Scan this QR code to check in for {{ branch }} on {{ date }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import QrcodeVue from "qrcode.vue";

const branches = [
  "Accra Main",
  "Kumasi",
  "Takoradi",
  "Tamale",
  "Tema",
  "Ho",
  "Sunyani",
  "Cape Coast",
  "Koforidua",
  "Wa",
  "Bolgatanga",
  "NIB HQ",
];
const branch = ref(branches[0]);
const date = ref(new Date().toISOString().slice(0, 10));
const qrValue = ref("");

function generateQR() {
  // QR value can include branch and date for validation
  qrValue.value = `NIB-ATTENDANCE|${branch.value}|${date.value}`;
}
</script>

<style scoped>
.action-btn.add {
  background: #2563eb;
  color: #fff;
  border-radius: 6px;
  padding: 0.5rem 1.25rem;
  font-weight: 500;
  font-size: 1rem;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
}
.action-btn.add:hover {
  filter: brightness(0.95);
}
@media (max-width: 768px) {
  .space-y-4,
  .space-y-6 {
    padding: 0 8px;
  }
  .grid {
    grid-template-columns: 1fr !important;
    gap: 0.5rem;
  }
  .flex {
    flex-direction: column;
    gap: 0.5rem;
  }
  .rounded-xl,
  .rounded-lg {
    border-radius: 0.5rem;
  }
}
</style>
