<template>
    <div class="login-container">
        <form class="login-form" @submit.prevent="handleLogin">
            <h2>Login</h2>
            <div class="form-group">
                <label for="username">Username</label>
                <input
                    id="username"
                    v-model="username"
                    type="text"
                    required
                    autocomplete="username"
                />
            </div>
            <div class="form-group">
                <label for="password">Password</label>
                <input
                    id="password"
                    v-model="password"
                    type="password"
                    required
                    autocomplete="current-password"
                />
            </div>
            <button type="submit" :disabled="loading">
                {{ loading ? 'Logging in...' : 'Login' }}
            </button>
            <p v-if="error" class="error">{{ error }}</p>
        </form>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const username = ref('')
const password = ref('')
const loading = ref(false)
const error = ref('')

const handleLogin = async () => {
    error.value = ''
    loading.value = true
    try {
        // Replace with your actual login logic/API call
        if (username.value === 'admin' && password.value === 'password') {
            // Redirect or set auth state here
            alert('Login successful!')
        } else {
            throw new Error('Invalid username or password')
        }
    } catch (err) {
        error.value = err.message
    } finally {
        loading.value = false
    }
}
</script>

<style scoped>
.login-container {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #f5f6fa;
}
.login-form {
    background: #fff;
    padding: 2rem 2.5rem;
    border-radius: 8px;
    box-shadow: 0 2px 12px rgba(0,0,0,0.08);
    width: 320px;
    display: flex;
    flex-direction: column;
    gap: 1rem;
}
.form-group {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
}
input[type="text"],
input[type="password"] {
    padding: 0.5rem;
    border: 1px solid #dcdde1;
    border-radius: 4px;
    font-size: 1rem;
}
button {
    padding: 0.5rem;
    background: #007bff;
    color: #fff;
    border: none;
    border-radius: 4px;
    font-size: 1rem;
    cursor: pointer;
}
button:disabled {
    background: #a5b1c2;
    cursor: not-allowed;
}
.error {
    color: #e74c3c;
    font-size: 0.95rem;
    margin-top: 0.5rem;
}
</style>