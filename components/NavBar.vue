<template>
  <nav class="navbar">
    <ul class="nav-list">
      <li class="nav-section">Employee ESS</li>
      <li><NuxtLink to="/">Dashboard</NuxtLink></li>
      <li><NuxtLink to="/employee/profile">Profile</NuxtLink></li>
      <li><NuxtLink to="/employee/leave">Leave</NuxtLink></li>
      <li><NuxtLink to="/employee/payslip">Payslip</NuxtLink></li>
      <li><NuxtLink to="/employee/attendance">Attendance</NuxtLink></li>
      <li class="nav-section">HR Admin</li>
      <li><NuxtLink to="/hr/leave">HR Leave</NuxtLink></li>
      <li><NuxtLink to="/hr/payroll">HR Payroll</NuxtLink></li>
      <li><NuxtLink to="/hr/employees">Employees</NuxtLink></li>
      <li><NuxtLink to="/hr/structure">Departments & Roles</NuxtLink></li>
      <li><NuxtLink to="/hr/attendance-qr">Attendance QR</NuxtLink></li>
      <li><NuxtLink to="/hr/dashboard">HR Dashboard</NuxtLink></li>
    </ul>
  </nav>
</template>

<script setup>
// No script needed for static links
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
