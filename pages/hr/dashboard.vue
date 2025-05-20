<template>
  <div class="space-y-8">
    <h1 class="text-2xl font-bold mb-4">HR Admin Dashboard</h1>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <div class="stat-card">
        <div class="stat-title">Total Employees</div>
        <div class="stat-value">{{ stats.totalEmployees }}</div>
      </div>
      <div class="stat-card">
        <div class="stat-title">On Leave Today</div>
        <div class="stat-value">{{ stats.onLeaveToday }}</div>
      </div>
      <div class="stat-card">
        <div class="stat-title">Pending Leave Requests</div>
        <div class="stat-value">{{ stats.pendingLeaves }}</div>
      </div>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mt-8">
      <div class="bg-white rounded-xl p-6 shadow">
        <h2 class="text-lg font-semibold mb-4">Leave Requests by Status</h2>
        <Bar :data="leaveStatusData" :options="barOptions" />
      </div>
      <div class="bg-white rounded-xl p-6 shadow">
        <h2 class="text-lg font-semibold mb-4">Employees by Department</h2>
        <Pie :data="departmentData" :options="pieOptions" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import { Bar, Pie } from "vue-chartjs";
import {
  Chart,
  CategoryScale,
  LinearScale,
  BarElement,
  Title,
  Tooltip,
  Legend,
  ArcElement,
} from "chart.js";

Chart.register(
  CategoryScale,
  LinearScale,
  BarElement,
  Title,
  Tooltip,
  Legend,
  ArcElement
);

// Simulated stats (replace with API calls in real app)
const stats = ref({
  totalEmployees: 42,
  onLeaveToday: 3,
  pendingLeaves: 5,
});

const leaveStatusData = {
  labels: ["Approved", "Pending", "Rejected"],
  datasets: [
    {
      label: "Requests",
      data: [20, 5, 2],
      backgroundColor: ["#22c55e", "#fbbf24", "#ef4444"],
    },
  ],
};
const barOptions = {
  responsive: true,
  plugins: { legend: { display: false } },
};

const departmentData = {
  labels: ["Retail", "Corporate", "IT", "HR", "Operations"],
  datasets: [
    {
      label: "Employees",
      data: [12, 8, 7, 6, 9],
      backgroundColor: ["#2563eb", "#f59e42", "#10b981", "#a21caf", "#f43f5e"],
    },
  ],
};
const pieOptions = {
  responsive: true,
  plugins: { legend: { position: "bottom" } },
};
</script>

<style scoped>
.stat-card {
  background: #fff;
  border-radius: 1rem;
  box-shadow: 0 2px 8px #e0e7ef;
  padding: 2rem 1.5rem;
  text-align: center;
}
.stat-title {
  color: #64748b;
  font-size: 1rem;
  margin-bottom: 0.5rem;
}
.stat-value {
  font-size: 2.5rem;
  font-weight: bold;
  color: #2563eb;
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
