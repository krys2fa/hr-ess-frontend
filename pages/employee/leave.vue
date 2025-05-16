<template>
  <div class="max-w-lg mx-auto">
    <h1 class="text-xl font-bold mb-4">Apply for Leave</h1>
    <form @submit.prevent="submitLeave">
      <input v-model="form.type" placeholder="Leave Type" class="input" />
      <input type="date" v-model="form.start" class="input" />
      <input type="date" v-model="form.end" class="input" />
      <button type="submit" class="btn-primary">Submit</button>
    </form>
  </div>
</template>

<script setup lang="ts">
const form = reactive({
  type: '',
  start: '',
  end: '',
})

const submitLeave = () => {
  alert(JSON.stringify(form))
}
</script>

<style scoped>
.input {
  display: block;
  width: 100%;
  margin-bottom: 1rem;
  padding: 0.5rem;
  border: 1px solid #ccc;
}
.btn-primary {
  background-color: #1E40AF;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
}
</style>
