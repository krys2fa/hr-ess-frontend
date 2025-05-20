<template>
  <div class="space-y-8">
    <h1 class="text-2xl font-bold mb-2">
      Departments, Roles & Reporting Structure
    </h1>
    <!-- Departments Section -->
    <section>
      <div class="flex items-center justify-between mb-2">
        <h2 class="text-xl font-semibold">Departments</h2>
        <button class="action-btn add" @click="openAddDeptModal">
          <PlusIcon class="w-5 h-5" /> Add Department
        </button>
      </div>
      <div class="bg-white rounded-xl p-4 shadow">
        <div class="grid grid-cols-3 font-bold border-b pb-2 table-header">
          <span>Name</span><span>Description</span><span>Actions</span>
        </div>
        <div
          v-for="dept in departments"
          :key="dept.id"
          class="grid grid-cols-3 border-b py-2 items-center"
        >
          <span>{{ dept.name }}</span>
          <span>{{ dept.description }}</span>
          <span class="flex gap-2">
            <button class="action-btn edit" @click="openEditDeptModal(dept)">
              <PencilSquareIcon class="w-5 h-5" /> Edit
            </button>
            <button
              class="action-btn delete"
              @click="openDeleteDeptModal(dept)"
            >
              <TrashIcon class="w-5 h-5" /> Delete
            </button>
          </span>
        </div>
      </div>
    </section>

    <!-- Roles Section -->
    <section>
      <div class="flex items-center justify-between mb-2">
        <h2 class="text-xl font-semibold">Roles</h2>
        <button class="action-btn add" @click="openAddRoleModal">
          <PlusIcon class="w-5 h-5" /> Add Role
        </button>
      </div>
      <div class="bg-white rounded-xl p-4 shadow">
        <div class="grid grid-cols-4 font-bold border-b pb-2 table-header">
          <span>Title</span><span>Department</span><span>Description</span
          ><span>Actions</span>
        </div>
        <div
          v-for="role in roles"
          :key="role.id"
          class="grid grid-cols-4 border-b py-2 items-center"
        >
          <span>{{ role.title }}</span>
          <span>{{ getDeptName(role.departmentId) }}</span>
          <span>{{ role.description }}</span>
          <span class="flex gap-2">
            <button class="action-btn edit" @click="openEditRoleModal(role)">
              <PencilSquareIcon class="w-5 h-5" /> Edit
            </button>
            <button
              class="action-btn delete"
              @click="openDeleteRoleModal(role)"
            >
              <TrashIcon class="w-5 h-5" /> Delete
            </button>
          </span>
        </div>
      </div>
    </section>

    <!-- Reporting Structure Section -->
    <section>
      <div class="flex items-center justify-between mb-2">
        <h2 class="text-xl font-semibold">Reporting Structure</h2>
        <button class="action-btn add" @click="openAddReportModal">
          <PlusIcon class="w-5 h-5" /> Add Reporting
        </button>
      </div>
      <div class="bg-white rounded-xl p-4 shadow">
        <div class="grid grid-cols-3 font-bold border-b pb-2 table-header">
          <span>Role</span><span>Reports To</span><span>Actions</span>
        </div>
        <div
          v-for="report in reporting"
          :key="report.id"
          class="grid grid-cols-3 border-b py-2 items-center"
        >
          <span>{{ getRoleTitle(report.roleId) }}</span>
          <span>{{ getRoleTitle(report.reportsToId) }}</span>
          <span class="flex gap-2">
            <button
              class="action-btn edit"
              @click="openEditReportModal(report)"
            >
              <PencilSquareIcon class="w-5 h-5" /> Edit
            </button>
            <button
              class="action-btn delete"
              @click="openDeleteReportModal(report)"
            >
              <TrashIcon class="w-5 h-5" /> Delete
            </button>
          </span>
        </div>
      </div>
    </section>

    <!-- Department Modals -->
    <TransitionRoot as="template" :show="showDeptModal">
      <Dialog as="div" class="relative z-50" @close="closeDeptModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel
            class="bg-white rounded-lg p-6 w-full max-w-md shadow-lg"
          >
            <DialogTitle class="text-lg font-bold mb-4">{{
              isEditDept ? "Edit Department" : "Add Department"
            }}</DialogTitle>
            <form
              @submit.prevent="isEditDept ? updateDept() : addDept()"
              class="space-y-4"
            >
              <div>
                <label class="block mb-1">Name</label>
                <input
                  v-model="deptModalData.name"
                  class="w-full border rounded px-3 py-2"
                  required
                />
              </div>
              <div>
                <label class="block mb-1">Description</label>
                <input
                  v-model="deptModalData.description"
                  class="w-full border rounded px-3 py-2"
                />
              </div>
              <div class="flex justify-end gap-2 mt-4">
                <button
                  type="button"
                  class="px-4 py-2 bg-gray-200 rounded"
                  @click="closeDeptModal"
                >
                  Cancel
                </button>
                <button
                  type="submit"
                  class="px-4 py-2 bg-blue-600 text-white rounded"
                >
                  {{ isEditDept ? "Update" : "Add" }}
                </button>
              </div>
            </form>
          </DialogPanel>
        </div>
      </Dialog>
    </TransitionRoot>
    <TransitionRoot as="template" :show="showDeleteDeptModal">
      <Dialog as="div" class="relative z-50" @close="closeDeleteDeptModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel
            class="bg-white rounded-lg p-6 w-full max-w-sm shadow-lg"
          >
            <DialogTitle class="text-lg font-bold mb-4"
              >Delete Department</DialogTitle
            >
            <p>
              Are you sure you want to delete
              <strong>{{ deptModalData.name }}</strong
              >?
            </p>
            <div class="flex justify-end gap-2 mt-6">
              <button
                class="px-4 py-2 bg-gray-200 rounded"
                @click="closeDeleteDeptModal"
              >
                Cancel
              </button>
              <button
                class="px-4 py-2 bg-red-500 text-white rounded"
                @click="deleteDept"
              >
                Delete
              </button>
            </div>
          </DialogPanel>
        </div>
      </Dialog>
    </TransitionRoot>

    <!-- Role Modals -->
    <TransitionRoot as="template" :show="showRoleModal">
      <Dialog as="div" class="relative z-50" @close="closeRoleModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel
            class="bg-white rounded-lg p-6 w-full max-w-md shadow-lg"
          >
            <DialogTitle class="text-lg font-bold mb-4">{{
              isEditRole ? "Edit Role" : "Add Role"
            }}</DialogTitle>
            <form
              @submit.prevent="isEditRole ? updateRole() : addRole()"
              class="space-y-4"
            >
              <div>
                <label class="block mb-1">Title</label>
                <input
                  v-model="roleModalData.title"
                  class="w-full border rounded px-3 py-2"
                  required
                />
              </div>
              <div>
                <label class="block mb-1">Department</label>
                <select
                  v-model="roleModalData.departmentId"
                  class="w-full border rounded px-3 py-2"
                  required
                >
                  <option v-for="d in departments" :key="d.id" :value="d.id">
                    {{ d.name }}
                  </option>
                </select>
              </div>
              <div>
                <label class="block mb-1">Description</label>
                <input
                  v-model="roleModalData.description"
                  class="w-full border rounded px-3 py-2"
                />
              </div>
              <div class="flex justify-end gap-2 mt-4">
                <button
                  type="button"
                  class="px-4 py-2 bg-gray-200 rounded"
                  @click="closeRoleModal"
                >
                  Cancel
                </button>
                <button
                  type="submit"
                  class="px-4 py-2 bg-blue-600 text-white rounded"
                >
                  {{ isEditRole ? "Update" : "Add" }}
                </button>
              </div>
            </form>
          </DialogPanel>
        </div>
      </Dialog>
    </TransitionRoot>
    <TransitionRoot as="template" :show="showDeleteRoleModal">
      <Dialog as="div" class="relative z-50" @close="closeDeleteRoleModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel
            class="bg-white rounded-lg p-6 w-full max-w-sm shadow-lg"
          >
            <DialogTitle class="text-lg font-bold mb-4"
              >Delete Role</DialogTitle
            >
            <p>
              Are you sure you want to delete
              <strong>{{ roleModalData.title }}</strong
              >?
            </p>
            <div class="flex justify-end gap-2 mt-6">
              <button
                class="px-4 py-2 bg-gray-200 rounded"
                @click="closeDeleteRoleModal"
              >
                Cancel
              </button>
              <button
                class="px-4 py-2 bg-red-500 text-white rounded"
                @click="deleteRole"
              >
                Delete
              </button>
            </div>
          </DialogPanel>
        </div>
      </Dialog>
    </TransitionRoot>

    <!-- Reporting Modals -->
    <TransitionRoot as="template" :show="showReportModal">
      <Dialog as="div" class="relative z-50" @close="closeReportModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel
            class="bg-white rounded-lg p-6 w-full max-w-md shadow-lg"
          >
            <DialogTitle class="text-lg font-bold mb-4">{{
              isEditReport ? "Edit Reporting" : "Add Reporting"
            }}</DialogTitle>
            <form
              @submit.prevent="isEditReport ? updateReport() : addReport()"
              class="space-y-4"
            >
              <div>
                <label class="block mb-1">Role</label>
                <select
                  v-model="reportModalData.roleId"
                  class="w-full border rounded px-3 py-2"
                  required
                >
                  <option v-for="r in roles" :key="r.id" :value="r.id">
                    {{ r.title }}
                  </option>
                </select>
              </div>
              <div>
                <label class="block mb-1">Reports To</label>
                <select
                  v-model="reportModalData.reportsToId"
                  class="w-full border rounded px-3 py-2"
                  required
                >
                  <option v-for="r in roles" :key="r.id" :value="r.id">
                    {{ r.title }}
                  </option>
                </select>
              </div>
              <div class="flex justify-end gap-2 mt-4">
                <button
                  type="button"
                  class="px-4 py-2 bg-gray-200 rounded"
                  @click="closeReportModal"
                >
                  Cancel
                </button>
                <button
                  type="submit"
                  class="px-4 py-2 bg-blue-600 text-white rounded"
                >
                  {{ isEditReport ? "Update" : "Add" }}
                </button>
              </div>
            </form>
          </DialogPanel>
        </div>
      </Dialog>
    </TransitionRoot>
    <TransitionRoot as="template" :show="showDeleteReportModal">
      <Dialog as="div" class="relative z-50" @close="closeDeleteReportModal">
        <div class="fixed inset-0 bg-black/30" aria-hidden="true" />
        <div class="fixed inset-0 flex items-center justify-center p-4">
          <DialogPanel
            class="bg-white rounded-lg p-6 w-full max-w-sm shadow-lg"
          >
            <DialogTitle class="text-lg font-bold mb-4"
              >Delete Reporting</DialogTitle
            >
            <p>Are you sure you want to delete this reporting structure?</p>
            <div class="flex justify-end gap-2 mt-6">
              <button
                class="px-4 py-2 bg-gray-200 rounded"
                @click="closeDeleteReportModal"
              >
                Cancel
              </button>
              <button
                class="px-4 py-2 bg-red-500 text-white rounded"
                @click="deleteReport"
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

// Departments
const departments = ref([
  {
    id: 1,
    name: "Retail Banking",
    description: "Handles all retail banking operations.",
  },
  {
    id: 2,
    name: "Corporate Banking",
    description: "Manages corporate clients and services.",
  },
]);
const showDeptModal = ref(false);
const showDeleteDeptModal = ref(false);
const isEditDept = ref(false);
const deptModalData = reactive({
  id: null as number | null,
  name: "",
  description: "",
});
function openAddDeptModal() {
  isEditDept.value = false;
  Object.assign(deptModalData, { id: null, name: "", description: "" });
  showDeptModal.value = true;
}
function openEditDeptModal(dept: any) {
  isEditDept.value = true;
  Object.assign(deptModalData, dept);
  showDeptModal.value = true;
}
function closeDeptModal() {
  showDeptModal.value = false;
}
function addDept() {
  departments.value.push({
    id: Date.now(),
    name: deptModalData.name,
    description: deptModalData.description,
  });
  closeDeptModal();
}
function updateDept() {
  const idx = departments.value.findIndex((d) => d.id === deptModalData.id);
  if (idx !== -1) {
    departments.value[idx] = {
      id: deptModalData.id as number,
      name: deptModalData.name,
      description: deptModalData.description,
    };
  }
  closeDeptModal();
}
function openDeleteDeptModal(dept: any) {
  Object.assign(deptModalData, dept);
  showDeleteDeptModal.value = true;
}
function closeDeleteDeptModal() {
  showDeleteDeptModal.value = false;
}
function deleteDept() {
  departments.value = departments.value.filter(
    (d) => d.id !== deptModalData.id
  );
  closeDeleteDeptModal();
}

// Roles
const roles = ref([
  {
    id: 1,
    title: "Branch Manager",
    departmentId: 1,
    description: "Manages branch operations.",
  },
  {
    id: 2,
    title: "Relationship Officer",
    departmentId: 2,
    description: "Handles client relationships.",
  },
]);
const showRoleModal = ref(false);
const showDeleteRoleModal = ref(false);
const isEditRole = ref(false);
const roleModalData = reactive({
  id: null as number | null,
  title: "",
  departmentId: null as number | null,
  description: "",
});
function openAddRoleModal() {
  isEditRole.value = false;
  Object.assign(roleModalData, {
    id: null,
    title: "",
    departmentId: null,
    description: "",
  });
  showRoleModal.value = true;
}
function openEditRoleModal(role: any) {
  isEditRole.value = true;
  Object.assign(roleModalData, role);
  showRoleModal.value = true;
}
function closeRoleModal() {
  showRoleModal.value = false;
}
function addRole() {
  roles.value.push({
    id: Date.now(),
    title: roleModalData.title,
    departmentId: roleModalData.departmentId,
    description: roleModalData.description,
  });
  closeRoleModal();
}
function updateRole() {
  const idx = roles.value.findIndex((r) => r.id === roleModalData.id);
  if (idx !== -1) {
    roles.value[idx] = {
      id: roleModalData.id as number,
      title: roleModalData.title,
      departmentId: roleModalData.departmentId,
      description: roleModalData.description,
    };
  }
  closeRoleModal();
}
function openDeleteRoleModal(role: any) {
  Object.assign(roleModalData, role);
  showDeleteRoleModal.value = true;
}
function closeDeleteRoleModal() {
  showDeleteRoleModal.value = false;
}
function deleteRole() {
  roles.value = roles.value.filter((r) => r.id !== roleModalData.id);
  closeDeleteRoleModal();
}
function getDeptName(id: number | null) {
  const d = departments.value.find((d) => d.id === id);
  return d ? d.name : "";
}

// Reporting Structure
const reporting = ref([{ id: 1, roleId: 2, reportsToId: 1 }]);
const showReportModal = ref(false);
const showDeleteReportModal = ref(false);
const isEditReport = ref(false);
const reportModalData = reactive({
  id: null as number | null,
  roleId: null as number | null,
  reportsToId: null as number | null,
});
function openAddReportModal() {
  isEditReport.value = false;
  Object.assign(reportModalData, { id: null, roleId: null, reportsToId: null });
  showReportModal.value = true;
}
function openEditReportModal(report: any) {
  isEditReport.value = true;
  Object.assign(reportModalData, report);
  showReportModal.value = true;
}
function closeReportModal() {
  showReportModal.value = false;
}
function addReport() {
  reporting.value.push({
    id: Date.now(),
    roleId: reportModalData.roleId,
    reportsToId: reportModalData.reportsToId,
  });
  closeReportModal();
}
function updateReport() {
  const idx = reporting.value.findIndex((r) => r.id === reportModalData.id);
  if (idx !== -1) {
    reporting.value[idx] = {
      id: reportModalData.id as number,
      roleId: reportModalData.roleId,
      reportsToId: reportModalData.reportsToId,
    };
  }
  closeReportModal();
}
function openDeleteReportModal(report: any) {
  Object.assign(reportModalData, report);
  showDeleteReportModal.value = true;
}
function closeDeleteReportModal() {
  showDeleteReportModal.value = false;
}
function deleteReport() {
  reporting.value = reporting.value.filter((r) => r.id !== reportModalData.id);
  closeDeleteReportModal();
}
function getRoleTitle(id: number | null) {
  const r = roles.value.find((r) => r.id === id);
  return r ? r.title : "";
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
