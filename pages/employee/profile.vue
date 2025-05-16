<template>
    <div class="employee-profile">
        <h1>Employee Profile</h1>
        <div v-if="loading" class="loading">Loading...</div>
        <div v-else>
            <div class="profile-section">
                <img :src="employee.avatarUrl" alt="Profile Picture" class="profile-avatar" />
                <div class="profile-details">
                    <h2>{{ employee.name }}</h2>
                    <p><strong>Position:</strong> {{ employee.position }}</p>
                    <p><strong>Email:</strong> {{ employee.email }}</p>
                    <p><strong>Phone:</strong> {{ employee.phone }}</p>
                    <p><strong>Department:</strong> {{ employee.department }}</p>
                </div>
            </div>
            <div class="profile-actions">
                <button @click="editProfile">Edit Profile</button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const loading = ref(true)
const employee = ref({
    name: '',
    position: '',
    email: '',
    phone: '',
    department: '',
    avatarUrl: ''
})

function fetchEmployeeProfile() {
    // Simulate API call
    setTimeout(() => {
        employee.value = {
            name: 'Jane Doe',
            position: 'Software Engineer',
            email: 'jane.doe@example.com',
            phone: '+1 234 567 890',
            department: 'Engineering',
            avatarUrl: 'https://randomuser.me/api/portraits/women/44.jpg'
        }
        loading.value = false
    }, 1000)
}

function editProfile() {
    // Placeholder for edit action
    alert('Edit profile clicked!')
}

onMounted(fetchEmployeeProfile)
</script>

<style scoped>
.employee-profile {
    max-width: 600px;
    margin: 40px auto;
    padding: 24px;
    background: #fff;
    border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.08);
}
.profile-section {
    display: flex;
    align-items: center;
    gap: 24px;
}
.profile-avatar {
    width: 120px;
    height: 120px;
    border-radius: 50%;
    object-fit: cover;
    border: 2px solid #eee;
}
.profile-details h2 {
    margin: 0 0 8px 0;
}
.profile-details p {
    margin: 4px 0;
}
.profile-actions {
    margin-top: 24px;
    text-align: right;
}
button {
    padding: 8px 20px;
    border: none;
    background: #1976d2;
    color: #fff;
    border-radius: 6px;
    cursor: pointer;
    font-size: 16px;
}
button:hover {
    background: #1565c0;
}
.loading {
    text-align: center;
    font-size: 18px;
    color: #888;
}
</style>