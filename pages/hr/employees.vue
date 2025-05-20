<template>
  <div class="space-y-6">
    <h1 class="text-2xl font-bold mb-2">Employee Management</h1>
    <button class="action-btn add mb-4" @click="openAddModal">
      <PlusIcon class="w-5 h-5" /> Add Employee
    </button>
    <div class="bg-white rounded-xl p-4 shadow">
      <div class="grid grid-cols-8 font-bold border-b pb-2 table-header">
        <span>Name</span><span>Staff ID</span><span>Branch</span
        ><span>Department</span><span>Role</span><span>Email</span
        ><span>Status</span><span>Actions</span>
      </div>
      <div
        v-for="emp in employees"
        :key="emp.id"
        class="grid grid-cols-8 border-b py-2 items-center"
      >
        <span>{{ emp.name }}</span>
        <span>{{ emp.staffId }}</span>
        <span>{{ emp.branch }}</span>
        <span>{{ emp.department }}</span>
        <span>{{ emp.role }}</span>
        <span>{{ emp.email }}</span>
        <span
          ><span :class="getStatusClass(emp.status)">{{
            emp.status
          }}</span></span
        >
        <span class="flex gap-2">
          <button class="action-btn edit" @click="openEditModal(emp)">
            <PencilSquareIcon class="w-5 h-5" /> Edit
          </button>
          <button class="action-btn delete" @click="openDeleteModal(emp)">
            <TrashIcon class="w-5 h-5" /> Delete
          </button>
        </span>
      </div>
    </div>

    <!-- Add/Edit Modal -->
    <TransitionRoot as="template" :show="showModal">
      <Dialog as="div" class="relative z-50" @close="closeModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel
            class="bg-white rounded-lg p-6 w-full max-w-lg shadow-lg"
          >
            <DialogTitle class="text-lg font-bold mb-4">{{
              isEdit ? "Edit Employee" : "Add Employee"
            }}</DialogTitle>
            <form
              @submit.prevent="isEdit ? updateEmployee() : addEmployee()"
              class="space-y-4"
            >
              <div class="grid grid-cols-2 gap-4">
                <div>
                  <label class="block mb-1">Name</label>
                  <input
                    v-model="modalData.name"
                    class="w-full border rounded px-3 py-2"
                    required
                  />
                </div>
                <div>
                  <label class="block mb-1">Staff ID</label>
                  <input
                    v-model="modalData.staffId"
                    class="w-full border rounded px-3 py-2"
                    required
                  />
                </div>
                <div>
                  <label class="block mb-1">Branch</label>
                  <select
                    v-model="modalData.branch"
                    class="w-full border rounded px-3 py-2"
                    required
                  >
                    <option v-for="b in branches" :key="b" :value="b">
                      {{ b }}
                    </option>
                  </select>
                </div>
                <div>
                  <label class="block mb-1">Department</label>
                  <select
                    v-model="modalData.departmentId"
                    class="w-full border rounded px-3 py-2"
                    required
                  >
                    <option v-for="d in departments" :key="d.id" :value="d.id">
                      {{ d.name }}
                    </option>
                  </select>
                </div>
                <div>
                  <label class="block mb-1">Role</label>
                  <select
                    v-model="modalData.roleId"
                    class="w-full border rounded px-3 py-2"
                    required
                  >
                    <option v-for="r in roles" :key="r.id" :value="r.id">
                      {{ r.title }}
                    </option>
                  </select>
                </div>
                <div>
                  <label class="block mb-1">Email</label>
                  <input
                    v-model="modalData.email"
                    type="email"
                    class="w-full border rounded px-3 py-2"
                    required
                  />
                </div>
                <div>
                  <label class="block mb-1">Phone</label>
                  <input
                    v-model="modalData.phone"
                    class="w-full border rounded px-3 py-2"
                    required
                  />
                </div>
                <div>
                  <label class="block mb-1">Status</label>
                  <select
                    v-model="modalData.status"
                    class="w-full border rounded px-3 py-2"
                  >
                    <option v-for="s in statusOptions" :key="s" :value="s">
                      {{ s }}
                    </option>
                  </select>
                </div>
              </div>
              <div class="flex justify-end gap-2 mt-4">
                <button
                  type="button"
                  class="px-4 py-2 bg-gray-200 rounded"
                  @click="closeModal"
                >
                  Cancel
                </button>
                <button
                  type="submit"
                  class="px-4 py-2 bg-blue-600 text-white rounded"
                >
                  {{ isEdit ? "Update" : "Add" }}
                </button>
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
          <DialogPanel
            class="bg-white rounded-lg p-6 w-full max-w-sm shadow-lg"
          >
            <DialogTitle class="text-lg font-bold mb-4"
              >Delete Employee</DialogTitle
            >
            <p>
              Are you sure you want to delete
              <strong>{{ modalData.name }}</strong> ({{ modalData.staffId }})?
            </p>
            <div class="flex justify-end gap-2 mt-6">
              <button
                class="px-4 py-2 bg-gray-200 rounded"
                @click="closeDeleteModal"
              >
                Cancel
              </button>
              <button
                class="px-4 py-2 bg-red-500 text-white rounded"
                @click="deleteEmployee"
              >
                Delete
              </button>
            </div>
          </DialogPanel>
        </div>
      </Dialog>
    </TransitionRoot>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from "vue";
import {
  Dialog,
  DialogPanel,
  DialogTitle,
  TransitionRoot,
} from "@headlessui/vue";
import {
  PencilSquareIcon,
  TrashIcon,
  PlusIcon,
} from "@heroicons/vue/24/outline";

const branches = [
  "Accra Main",
  "Kumasi",
  "Takoradi",
  "Tamale",
  "Tema",
  "Ho",
  "Sunyani",
  "Cape Coast",
  "Koforidua",
  "Wa",
  "Bolgatanga",
  "NIB HQ",
];
const statusOptions = ["Active", "Inactive", "Suspended"];

// Fetch departments and roles from the structure page (simulate API or shared store)
const departments = ref([
  { id: 1, name: "Retail Banking" },
  { id: 2, name: "Corporate Banking" },
]);
const roles = ref([
  { id: 1, title: "Branch Manager", departmentId: 1 },
  { id: 2, title: "Relationship Officer", departmentId: 2 },
]);

const employees = ref([
  {
    id: 1,
    name: "Ama Mensah",
    staffId: "NIB001",
    branch: "Accra Main",
    department: "Retail Banking",
    role: "Branch Manager",
    email: "ama.mensah@nibghana.com",
    phone: "+233201234567",
    status: "Active",
  },
]);

const showModal = ref(false);
const showDeleteModal = ref(false);
const isEdit = ref(false);
const modalData = reactive({
  id: null as number | null,
  name: "",
  staffId: "",
  branch: "",
  departmentId: null as number | null,
  roleId: null as number | null,
  email: "",
  phone: "",
  status: "Active",
});

function openAddModal() {
  isEdit.value = false;
  Object.assign(modalData, {
    id: null,
    name: "",
    staffId: "",
    branch: "",
    departmentId: null,
    roleId: null,
    email: "",
    phone: "",
    status: "Active",
  });
  showModal.value = true;
}

function openEditModal(emp: any) {
  isEdit.value = true;
  Object.assign(modalData, {
    ...emp,
    departmentId:
      departments.value.find((d) => d.name === emp.department)?.id ?? null,
    roleId: roles.value.find((r) => r.title === emp.role)?.id ?? null,
  });
  showModal.value = true;
}

function closeModal() {
  showModal.value = false;
}

function addEmployee() {
  employees.value.push({
    id: Date.now(),
    name: modalData.name,
    staffId: modalData.staffId,
    branch: modalData.branch,
    department:
      departments.value.find((d) => d.id === modalData.departmentId)?.name ||
      "",
    role: roles.value.find((r) => r.id === modalData.roleId)?.title || "",
    email: modalData.email,
    phone: modalData.phone,
    status: modalData.status,
  });
  closeModal();
}

function updateEmployee() {
  const idx = employees.value.findIndex((e) => e.id === modalData.id);
  if (idx !== -1) {
    employees.value[idx] = {
      id: modalData.id as number,
      name: modalData.name,
      staffId: modalData.staffId,
      branch: modalData.branch,
      department:
        departments.value.find((d) => d.id === modalData.departmentId)?.name ||
        "",
      role: roles.value.find((r) => r.id === modalData.roleId)?.title || "",
      email: modalData.email,
      phone: modalData.phone,
      status: modalData.status,
    };
  }
  closeModal();
}

function openDeleteModal(emp: any) {
  Object.assign(modalData, emp);
  showDeleteModal.value = true;
}

function closeDeleteModal() {
  showDeleteModal.value = false;
}

function deleteEmployee() {
  employees.value = employees.value.filter((e) => e.id !== modalData.id);
  closeDeleteModal();
}

function getStatusClass(status: string) {
  if (status === "Active") return "text-green-500 font-semibold";
  if (status === "Suspended") return "text-yellow-500 font-semibold";
  return "text-gray-400 font-semibold";
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
  .space-y-4,
  .space-y-6 {
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
  .rounded-xl,
  .rounded-lg {
    border-radius: 0.5rem;
  }
}
</style>
