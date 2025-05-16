<template>
    <div class="signup-container">
        <h1>Sign Up</h1>
        <form @submit.prevent="handleSignup">
            <div class="form-group">
                <label for="name">Full Name</label>
                <input v-model="form.name" type="text" id="name" required />
            </div>
            <div class="form-group">
                <label for="email">Email Address</label>
                <input v-model="form.email" type="email" id="email" required />
            </div>
            <div class="form-group">
                <label for="password">Password</label>
                <input v-model="form.password" type="password" id="password" required />
            </div>
            <div class="form-group">
                <label for="confirmPassword">Confirm Password</label>
                <input v-model="form.confirmPassword" type="password" id="confirmPassword" required />
            </div>
            <div v-if="error" class="error-message">{{ error }}</div>
            <button type="submit">Create Account</button>
        </form>
        <p>
            Already have an account?
            <nuxt-link to="/login">Login here</nuxt-link>
        </p>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const form = ref({
    name: '',
    email: '',
    password: '',
    confirmPassword: ''
})
const error = ref('')

const handleSignup = async () => {
    error.value = ''
    if (form.value.password !== form.value.confirmPassword) {
        error.value = 'Passwords do not match.'
        return
    }
    try {
        // Replace with your actual signup API call
        // await api.signup(form.value)
        // Simulate success:
        router.push('/login')
    } catch (e) {
        error.value = 'Signup failed. Please try again.'
    }
}
</script>

<style scoped>
.signup-container {
    max-width: 400px;
    margin: 40px auto;
    padding: 2rem;
    border: 1px solid #eee;
    border-radius: 8px;
    background: #fff;
}
h1 {
    text-align: center;
    margin-bottom: 1.5rem;
}
.form-group {
    margin-bottom: 1rem;
}
label {
    display: block;
    margin-bottom: 0.3rem;
}
input {
    width: 100%;
    padding: 0.5rem;
    box-sizing: border-box;
}
button {
    width: 100%;
    padding: 0.7rem;
    background: #007bff;
    color: #fff;
    border: none;
    border-radius: 4px;
    font-size: 1rem;
}
.error-message {
    color: #d32f2f;
    margin-bottom: 1rem;
    text-align: center;
}
p {
    text-align: center;
    margin-top: 1rem;
}
</style>