<template>
    <div class="dashboard">
        <h1>Welcome to HR ESS Dashboard</h1>
        <div class="dashboard-cards">
            <div class="card" v-for="item in dashboardItems" :key="item.title">
                <div class="card-icon">
                    <component :is="item.icon" />
                </div>
                <div class="card-content">
                    <h2>{{ item.title }}</h2>
                    <p>{{ item.description }}</p>
                    <router-link :to="item.link" class="card-link">Go to {{ item.title }}</router-link>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'

// Example icons (replace with your own or use a library)
const UserIcon = {
    template: '<span>👤</span>'
}
const LeaveIcon = {
    template: '<span>🗓️</span>'
}
const PayslipIcon = {
    template: '<span>💰</span>'
}

const dashboardItems = ref([
    {
        title: 'My Profile',
        description: 'View and update your personal information.',
        link: '/employee/profile',
        icon: UserIcon
    },
    {
        title: 'Leave Requests',
        description: 'Apply for leave and track your requests.',
        link: '/employee/leave',
        icon: LeaveIcon
    },
    {
        title: 'Payslips',
        description: 'Access your monthly payslips.',
        link: '/employee/payslip',
        icon: PayslipIcon
    }
])
</script>

<style scoped>
.dashboard {
    max-width: 900px;
    margin: 0 auto;
    padding: 2rem;
}
.dashboard-cards {
    display: flex;
    gap: 2rem;
    flex-wrap: wrap;
    margin-top: 2rem;
}
.card {
    background: #fff;
    border-radius: 10px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.06);
    padding: 1.5rem;
    flex: 1 1 250px;
    min-width: 250px;
    display: flex;
    align-items: center;
    gap: 1rem;
}
.card-icon {
    font-size: 2.5rem;
}
.card-content h2 {
    margin: 0 0 0.5rem 0;
}
.card-link {
    color: #1976d2;
    text-decoration: underline;
    font-size: 0.95rem;
}
</style>