<template>
  <div class="space-y-4">
    <h1 class="text-2xl font-bold mb-2">Leave Management</h1>
    <button class="action-btn add mb-4" @click="openAddModal">
      <PlusIcon class="w-5 h-5" /> Add Leave
    </button>
    <div class="bg-white rounded-xl p-4 shadow">
      <div class="grid grid-cols-8 font-bold border-b pb-2 table-header">
        <span>Employee</span><span>Type</span><span>Start</span><span>End</span><span>Status</span><span>Leave Entitled</span><span>Leave Taken</span><span>Leave Available</span><span>Actions</span>
      </div>
      <div v-for="leave in leaves" :key="leave.id" class="grid grid-cols-8 border-b py-2 items-center">
        <span>{{ leave.employee }}</span>
        <span>{{ leave.type }}</span>
        <span>{{ leave.start }}</span>
        <span>{{ leave.end }}</span>
        <span><span :class="getStatusClass(leave.status)">{{ leave.status }}</span></span>
        <span>{{ getEntitled(leave.employee) }}</span>
        <span>{{ getTaken(leave.employee) }}</span>
        <span>{{ getLeaveAvailable(leave.employee) }}</span>
        <span class="flex gap-2">
          <button class="action-btn edit" @click="openEditModal(leave)"><PencilSquareIcon class="w-5 h-5" /> Edit</button>
          <button class="action-btn delete" @click="openDeleteModal(leave)"><TrashIcon class="w-5 h-5" /> Delete</button>
          <button class="action-btn add" @click="showEmployeeLeaves(leave.employee)">View All</button>
        </span>
      </div>
    </div>

    <!-- Add/Edit Modal -->
    <TransitionRoot as="template" :show="showModal">
      <Dialog as="div" class="relative z-50" @close="closeModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel class="bg-white rounded-lg p-6 w-full max-w-md shadow-lg">
            <DialogTitle class="text-lg font-bold mb-4">{{ isEdit ? 'Edit Leave' : 'Add Leave' }}</DialogTitle>
            <form @submit.prevent="isEdit ? updateLeave() : addLeave()" class="space-y-4">
              <div>
                <label class="block mb-1">Employee</label>
                <input v-model="modalData.employee" class="w-full border rounded px-3 py-2" required />
              </div>
              <div>
                <label class="block mb-1">Type</label>
                <input v-model="modalData.type" class="w-full border rounded px-3 py-2" required />
              </div>
              <div>
                <label class="block mb-1">Start</label>
                <input v-model="modalData.start" type="date" class="w-full border rounded px-3 py-2" required />
              </div>
              <div>
                <label class="block mb-1">End</label>
                <input v-model="modalData.end" type="date" class="w-full border rounded px-3 py-2" required />
              </div>
              <div>
                <label class="block mb-1">Status</label>
                <select v-model="modalData.status" class="w-full border rounded px-3 py-2">
                  <option v-for="status in statusOptions" :key="status" :value="status">{{ status }}</option>
                </select>
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
            <DialogTitle class="text-lg font-bold mb-4">Delete Leave</DialogTitle>
            <p>Are you sure you want to delete <strong>{{ modalData.employee }}</strong>'s leave ({{ modalData.type }})?</p>
            <div class="flex justify-end gap-2 mt-6">
              <button class="px-4 py-2 bg-gray-200 rounded" @click="closeDeleteModal">Cancel</button>
              <button class="px-4 py-2 bg-red-500 text-white rounded" @click="deleteLeave">Delete</button>
            </div>
          </DialogPanel>
        </div>
      </Dialog>
    </TransitionRoot>

    <!-- Employee Leave Days Modal -->
    <TransitionRoot as="template" :show="showEmployeeLeavesModal">
      <Dialog as="div" class="relative z-50" @close="closeEmployeeLeavesModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel class="bg-white rounded-lg p-6 w-full max-w-lg shadow-lg">
            <DialogTitle class="text-lg font-bold mb-4">Leave Days for {{ selectedEmployeeName }}</DialogTitle>
            <div v-if="employeeLeaves.length === 0" class="text-gray-500">No leave records found.</div>
            <table v-else class="w-full text-left border mt-2">
              <thead>
                <tr>
                  <th class="p-2">Type</th>
                  <th>Start</th>
                  <th>End</th>
                  <th>Status</th>
                  <th>Days</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="l in employeeLeaves" :key="l.id">
                  <td class="p-2">{{ l.type }}</td>
                  <td>{{ l.start }}</td>
                  <td>{{ l.end }}</td>
                  <td>{{ l.status }}</td>
                  <td>{{ calcLeaveDays(l.start, l.end) }}</td>
                </tr>
              </tbody>
            </table>
            <div class="flex justify-end gap-2 mt-4">
              <button class="px-4 py-2 bg-gray-200 rounded" @click="closeEmployeeLeavesModal">Close</button>
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

const leaves = ref([
  { id: 1, employee: 'Chris A.', type: 'Annual', start: '2025-05-01', end: '2025-05-05', status: 'Approved' },
])

const showModal = ref(false)
const showDeleteModal = ref(false)
const showEmployeeLeavesModal = ref(false)
const isEdit = ref(false)
const modalData = reactive({
  id: null as number | null,
  employee: '',
  type: '',
  start: '',
  end: '',
  status: 'Pending',
})
const selectedEmployeeName = ref('')
const employeeLeaves = ref<any[]>([])

// Define leave entitlements by rank/role
const leaveEntitlements: Record<string, number> = {
  'Branch Manager': 30,
  'Relationship Officer': 25,
  'Clerk': 20,
  'Teller': 18,
  'Default': 15,
}

// Example employees with roles/ranks
const employees = [
  { id: 1, name: 'Chris A.', role: 'Branch Manager' },
  { id: 2, name: 'Ama Mensah', role: 'Relationship Officer' },
  // ...add more as needed
]

// Track leave taken per employee (by employee id)
const leaveTaken = ref<Record<number, number>>({})

function openAddModal() {
  isEdit.value = false
  Object.assign(modalData, { id: null, employee: '', type: '', start: '', end: '', status: 'Pending' })
  showModal.value = true
}

function openEditModal(leave: any) {
  isEdit.value = true
  Object.assign(modalData, leave)
  showModal.value = true
}

function closeModal() {
  showModal.value = false
}

function addLeave() {
  const emp = employees.find(e => e.name === modalData.employee)
  if (emp) {
    const days = calcLeaveDays(modalData.start, modalData.end)
    const entitled = leaveEntitlements[emp.role] || leaveEntitlements['Default']
    const taken = leaveTaken.value[emp.id] || 0
    const available = entitled - taken
    if (days > available) {
      window.alert('Cannot approve leave: not enough available leave days.')
      return
    }
    leaveTaken.value[emp.id] = taken + days
  }
  leaves.value.push({
    id: Date.now(),
    employee: modalData.employee,
    type: modalData.type,
    start: modalData.start,
    end: modalData.end,
    status: modalData.status,
  })
  closeModal()
}

function updateLeave() {
  const idx = leaves.value.findIndex(l => l.id === modalData.id)
  if (idx !== -1) {
    const emp = employees.find(e => e.name === modalData.employee)
    if (emp) {
      const days = calcLeaveDays(modalData.start, modalData.end)
      const entitled = leaveEntitlements[emp.role] || leaveEntitlements['Default']
      const taken = leaveTaken.value[emp.id] || 0
      const available = entitled - taken
      if (days > available) {
        window.alert('Cannot approve leave: not enough available leave days.')
        return
      }
    }
    leaves.value[idx] = {
      id: modalData.id as number,
      employee: modalData.employee,
      type: modalData.type,
      start: modalData.start,
      end: modalData.end,
      status: modalData.status,
    }
  }
  closeModal()
}

function openDeleteModal(leave: any) {
  Object.assign(modalData, leave)
  showDeleteModal.value = true
}

function closeDeleteModal() {
  showDeleteModal.value = false
}

function deleteLeave() {
  const idx = leaves.value.findIndex(l => l.id === modalData.id)
  if (idx !== -1) {
    const emp = employees.find(e => e.name === leaves.value[idx].employee)
    if (emp) {
      const days = calcLeaveDays(leaves.value[idx].start, leaves.value[idx].end)
      leaveTaken.value[emp.id] = (leaveTaken.value[emp.id] || 0) - days
    }
    leaves.value.splice(idx, 1)
  }
  closeDeleteModal()
}

function showEmployeeLeaves(employeeName: string) {
  selectedEmployeeName.value = employeeName
  const emp = employees.find(e => e.name === employeeName)
  if (emp) {
    employeeLeaves.value = leaves.value.filter(l => l.employee === employeeName)
  }
  showEmployeeLeavesModal.value = true
}

function closeEmployeeLeavesModal() {
  showEmployeeLeavesModal.value = false
  employeeLeaves.value = []
}

// Utility functions
function calcLeaveDays(start: string, end: string) {
  const startDate = new Date(start)
  const endDate = new Date(end)
  const timeDiff = endDate.getTime() - startDate.getTime()
  const dayDiff = Math.ceil(timeDiff / (1000 * 3600 * 24)) + 1
  return dayDiff
}

function getEntitled(employeeName: string) {
  const emp = employees.find(e => e.name === employeeName)
  return emp ? (leaveEntitlements[emp.role] || leaveEntitlements['Default']) : 0
}

function getTaken(employeeName: string) {
  const emp = employees.find(e => e.name === employeeName)
  return emp ? (leaveTaken.value[emp.id] || 0) : 0
}

function getLeaveAvailable(employeeName: string) {
  const emp = employees.find(e => e.name === employeeName)
  if (emp) {
    const entitled = leaveEntitlements[emp.role] || leaveEntitlements['Default']
    const taken = leaveTaken.value[emp.id] || 0
    return entitled - taken
  }
  return 0
}

function getStatusClass(status: string) {
  switch (status) {
    case 'Approved':
      return 'text-green-500'
    case 'Pending':
      return 'text-yellow-500'
    case 'Rejected':
      return 'text-red-500'
    default:
      return ''
  }
}
</script>

<style scoped>
.table-header {
  background-color: #f9fafb;
}
.action-btn {
  display: inline-flex;
  align-items: center;
  padding: 0.75rem 1.25rem;
  font-size: 0.95rem;
  font-weight: 600;
  border-radius: 0.375rem;
}
.action-btn.add {
  background: #2563eb;
  color: #fff;
}
.action-btn.edit {
  background: #fbbf24;
  color: #fff;
}
.action-btn.delete {
  background: #ef4444;
  color: #fff;
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
