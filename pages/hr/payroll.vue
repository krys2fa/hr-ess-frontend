<template>
  <div class="space-y-4">
    <h1 class="text-2xl font-bold mb-2">Payroll</h1>
    <button class="action-btn add mb-4" @click="openAddModal">
      <PlusIcon class="w-5 h-5" /> Add Payroll
    </button>
    <div class="bg-white rounded-xl p-4 shadow">
      <div class="grid grid-cols-5 font-bold border-b pb-2 table-header">
        <span>Name</span><span>Month</span><span>Gross</span><span>Net</span><span>Actions</span>
      </div>
      <div v-for="pay in payrolls" :key="pay.id" class="grid grid-cols-5 border-b py-2 items-center">
        <span>{{ pay.name }}</span>
        <span>{{ pay.month }}</span>
        <span>${{ pay.gross }}</span>
        <span>${{ pay.net }}</span>
        <span class="flex gap-2">
          <button class="action-btn edit" @click="openEditModal(pay)"><PencilSquareIcon class="w-5 h-5" /> Edit</button>
          <button class="action-btn delete" @click="openDeleteModal(pay)"><TrashIcon class="w-5 h-5" /> Delete</button>
        </span>
      </div>
    </div>

    <!-- Add/Edit Modal -->
    <TransitionRoot as="template" :show="showModal">
      <Dialog as="div" class="relative z-50" @close="closeModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel class="bg-white rounded-lg p-6 w-full max-w-md shadow-lg">
            <DialogTitle class="text-lg font-bold mb-4">{{ isEdit ? 'Edit Payroll' : 'Add Payroll' }}</DialogTitle>
            <form @submit.prevent="isEdit ? updatePayroll() : addPayroll()" class="space-y-4">
              <div>
                <label class="block mb-1">Name</label>
                <input v-model="modalData.name" class="w-full border rounded px-3 py-2" required />
              </div>
              <div>
                <label class="block mb-1">Month</label>
                <input v-model="modalData.month" class="w-full border rounded px-3 py-2" required />
              </div>
              <div>
                <label class="block mb-1">Gross</label>
                <input v-model.number="modalData.gross" type="number" class="w-full border rounded px-3 py-2" required />
              </div>
              <div>
                <label class="block mb-1">Net</label>
                <input v-model.number="modalData.net" type="number" class="w-full border rounded px-3 py-2" required />
              </div>
              <div class="flex justify-end gap-2 mt-4">
                <button type="button" class="px-4 py-2 bg-gray-200 rounded" @click="closeModal">Cancel</button>
                <button type="submit" class="px-4 py-2 bg-blue-600 text-white rounded">{{ isEdit ? 'Update' : 'Add' }}</button>
              </div>
            </form>
          </DialogPanel>
        </div>
      </Dialog>
    </TransitionRoot>

    <!-- Delete Modal -->
    <TransitionRoot as="template" :show="showDeleteModal">
      <Dialog as="div" class="relative z-50" @close="closeDeleteModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel class="bg-white rounded-lg p-6 w-full max-w-sm shadow-lg">
            <DialogTitle class="text-lg font-bold mb-4">Delete Payroll</DialogTitle>
            <p>Are you sure you want to delete <strong>{{ modalData.name }}</strong>'s payroll for <strong>{{ modalData.month }}</strong>?</p>
            <div class="flex justify-end gap-2 mt-6">
              <button class="px-4 py-2 bg-gray-200 rounded" @click="closeDeleteModal">Cancel</button>
              <button class="px-4 py-2 bg-red-500 text-white rounded" @click="deletePayroll">Delete</button>
            </div>
          </DialogPanel>
        </div>
      </Dialog>
    </TransitionRoot>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
import { Dialog, DialogPanel, DialogTitle, TransitionRoot } from '@headlessui/vue'
import { PencilSquareIcon, TrashIcon, PlusIcon } from '@heroicons/vue/24/outline'

const payrolls = ref([
  { id: 1, name: 'Chris A.', month: 'May 2025', gross: 3000, net: 2500 },
])

const showModal = ref(false)
const showDeleteModal = ref(false)
const isEdit = ref(false)
const modalData = reactive({
  id: null as number | null,
  name: '',
  month: '',
  gross: 0,
  net: 0,
})

function openAddModal() {
  isEdit.value = false
  Object.assign(modalData, { id: null, name: '', month: '', gross: 0, net: 0 })
  showModal.value = true
}

function openEditModal(pay: any) {
  isEdit.value = true
  Object.assign(modalData, pay)
  showModal.value = true
}

function closeModal() {
  showModal.value = false
}

function addPayroll() {
  payrolls.value.push({
    id: Date.now(),
    name: modalData.name,
    month: modalData.month,
    gross: modalData.gross,
    net: modalData.net,
  })
  closeModal()
}

function updatePayroll() {
  const idx = payrolls.value.findIndex(p => p.id === modalData.id)
  if (idx !== -1) {
    payrolls.value[idx] = {
      id: modalData.id as number, // Ensure id is number
      name: modalData.name,
      month: modalData.month,
      gross: modalData.gross,
      net: modalData.net,
    }
  }
  closeModal()
}

function openDeleteModal(pay: any) {
  Object.assign(modalData, pay)
  showDeleteModal.value = true
}

function closeDeleteModal() {
  showDeleteModal.value = false
}

function deletePayroll() {
  payrolls.value = payrolls.value.filter(p => p.id !== modalData.id)
  closeDeleteModal()
}
</script>

<style scoped>
.table-header {
  background: linear-gradient(90deg, #e0e7ff 0%, #f0fdfa 100%);
}
.action-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.25rem;
  font-size: 0.95rem;
  font-weight: 500;
  border-radius: 6px;
  padding: 0.25rem 0.75rem;
  transition: background 0.2s;
}
.action-btn.edit {
  background: #fbbf24;
  color: #fff;
}
.action-btn.delete {
  background: #ef4444;
  color: #fff;
}
.action-btn.add {
  background: #2563eb;
  color: #fff;
}
.action-btn:hover {
  filter: brightness(0.95);
}

@media (max-width: 768px) {
  .space-y-4, .space-y-6 {
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
  .rounded-xl, .rounded-lg {
    border-radius: 0.5rem;
  }
}
</style>
