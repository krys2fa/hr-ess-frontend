<template>
  <div class="max-w-md mx-auto mt-16 p-6 bg-white rounded shadow">
    <h1 class="text-2xl font-bold mb-4">Login</h1>
    <form @submit.prevent="login">
      <input
        v-model="form.email"
        type="email"
        placeholder="Email"
        class="input"
        required
      />
      <input
        v-model="form.password"
        type="password"
        placeholder="Password"
        class="input"
        required
      />
      <button type="submit" class="btn-primary w-full">Login</button>
    </form>
    <div v-if="message" class="mt-2 text-blue-700">{{ message }}</div>
    <div class="mt-4 text-sm text-center">
      Don't have an account?
      <NuxtLink to="/signup" class="text-blue-600 underline">Register</NuxtLink>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from "vue";
import { useRouter } from "vue-router";
import { useUserStore } from "~/stores/user";

const form = reactive({
  email: "",
  password: "",
});
const message = ref("");
const router = useRouter();
const userStore = useUserStore();

function login() {
  // Simulate login (in real app, call API)
  if (form.email && form.password) {
    // Demo: admin if email contains 'admin', else regular user
    const role = form.email.includes("admin") ? "admin" : "employee";
    userStore.setUser({ id: 1, name: form.email.split("@")[0], role });
    message.value = "Login successful! Redirecting...";
    setTimeout(() => {
      router.push("/");
    }, 1000);
  } else {
    message.value = "Please enter your email and password.";
  }
}
</script>

<style scoped>
.input {
  display: block;
  width: 100%;
  margin-bottom: 1rem;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 0.375rem;
}
.btn-primary {
  background-color: #1e40af;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  font-weight: 600;
  margin-top: 0.5rem;
}
</style>
