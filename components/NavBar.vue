<template>
  <nav class="navbar">
    <ul class="nav-list">
      <li class="nav-section">Employee ESS</li>
      <li v-for="link in essLinks" :key="link.to">
        <NuxtLink :to="link.to">{{ link.label }}</NuxtLink>
      </li>
      <template v-if="user && user.role === 'admin'">
        <li class="nav-section">HR Admin</li>
        <li v-for="link in hrLinks" :key="link.to">
          <NuxtLink :to="link.to">{{ link.label }}</NuxtLink>
        </li>
      </template>
    </ul>
  </nav>
</template>

<script setup>
import { useUserStore } from '~/stores/user'
import { storeToRefs } from 'pinia'

const userStore = useUserStore()
const { user } = storeToRefs(userStore)

const essLinks = [
  { to: '/', label: 'Dashboard' },
  { to: '/employee/profile', label: 'Profile' },
  { to: '/employee/leave', label: 'Leave' },
  { to: '/employee/payslip', label: 'Payslip' },
  { to: '/employee/attendance', label: 'Attendance' },
]
const hrLinks = [
  { to: '/hr/leave', label: 'HR Leave' },
  { to: '/hr/payroll', label: 'HR Payroll' },
  { to: '/hr/employees', label: 'Employees' },
  { to: '/hr/structure', label: 'Departments & Roles' },
  { to: '/hr/attendance-qr', label: 'Attendance QR' },
  { to: '/hr/dashboard', label: 'HR Dashboard' },
]
</script>

<style scoped>
.navbar {
  background: #f5f5f5;
  padding: 1rem 2rem;
  border-bottom: 1px solid #e0e0e0;
}
.nav-list {
  display: flex;
  gap: 1.5rem;
  align-items: center;
  list-style: none;
  margin: 0;
  padding: 0;
}
.nav-section {
  font-weight: bold;
  margin-right: 1rem;
  color: #1976d2;
}
.nav-list li {
  margin: 0;
}
.nav-list a {
  color: #333;
  text-decoration: none;
  font-weight: 500;
}
.nav-list a:hover {
  color: #1976d2;
}
@media (max-width: 768px) {
  .navbar {
    padding: 0.5rem 1rem;
  }
  .nav-list {
    flex-direction: column;
    gap: 0.5rem;
    align-items: flex-start;
  }
}
</style>
