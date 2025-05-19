<template>
    <div class="payslip-container">
        <h1>Employee Payslip</h1>
        <div class="payslip-form">
            <label for="month">Select Month:</label>
            <select v-model="selectedMonth" id="month">
                <option v-for="month in months" :key="month.value" :value="month.value">
                    {{ month.text }}
                </option>
            </select>
            <button @click="fetchPayslip" :disabled="loading">
                {{ loading ? 'Loading...' : 'View Payslip' }}
            </button>
        </div>

        <div v-if="error" class="error">{{ error }}</div>

        <div v-if="payslip" class="payslip-details">
            <h2>Payslip for {{ payslip.month }} {{ payslip.year }}</h2>
            <table>
                <tr>
                    <th>Employee Name</th>
                    <td>{{ payslip.employeeName }}</td>
                </tr>
                <tr>
                    <th>Employee ID</th>
                    <td>{{ payslip.employeeId }}</td>
                </tr>
                <tr>
                    <th>Basic Salary</th>
                    <td>{{ payslip.basicSalary | currency }}</td>
                </tr>
                <tr>
                    <th>Allowances</th>
                    <td>{{ payslip.allowances | currency }}</td>
                </tr>
                <!-- <tr>
                    <th>Deductions</th>
                    <td>{{ payslip.deductions | currency }}</td>
                </tr> -->
                <tr>
                    <th>Net Pay</th>
                    <td><strong>{{ payslip.netPay | currency }}</strong></td>
                </tr>
            </table>
            <button @click="downloadPayslip">Download PDF</button>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const months = [
    { value: '2024-06', text: 'June 2024' },
    { value: '2024-05', text: 'May 2024' },
    { value: '2024-04', text: 'April 2024' },
    // Add more months as needed
]

const selectedMonth = ref(months[0].value)
const payslip = ref(null)
const loading = ref(false)
const error = ref('')

function fetchPayslip() {
    loading.value = true
    error.value = ''
    payslip.value = null

    // Simulate API call
    setTimeout(() => {
        // Replace this with actual API call
        if (selectedMonth.value) {
            payslip.value = {
                month: months.find(m => m.value === selectedMonth.value).text.split(' ')[0],
                year: months.find(m => m.value === selectedMonth.value).text.split(' ')[1],
                employeeName: 'John Doe',
                employeeId: 'EMP12345',
                basicSalary: 50000,
                allowances: 10000,
                deductions: 5000,
                netPay: 55000,
            }
        } else {
            error.value = 'Payslip not found for the selected month.'
        }
        loading.value = false
    }, 1000)
}

function downloadPayslip() {
    // Placeholder for PDF download logic
    alert('Payslip PDF download not implemented.')
}

// export default {
    // name: 'EmployeePayslip',
    // No options API logic needed
// }


// import { createApp } from 'vue'

// const app = createApp({})
// app.config.globalProperties.$filters = {
//     currency(value) {
//         if (typeof value !== "number") {
//             return value
//         }
//         return '₦' + value.toLocaleString()
//     }
// }


function currency(value) {
    if (typeof value !== "number") {
        return value
    }
    return '₦' + value.toLocaleString()
}

</script>

<style scoped>
.payslip-container {
    max-width: 600px;
    margin: 40px auto;
    padding: 24px;
    background: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 8px #eee;
}
.payslip-form {
    display: flex;
    align-items: center;
    gap: 16px;
    margin-bottom: 24px;
}
.payslip-details {
    margin-top: 24px;
}
table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 16px;
}
th, td {
    text-align: left;
    padding: 8px;
    border-bottom: 1px solid #eee;
}
.error {
    color: #d32f2f;
    margin-top: 16px;
}
button {
    background: #1976d2;
    color: #fff;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
}
button:disabled {
    background: #90caf9;
    cursor: not-allowed;
}

@media (max-width: 768px) {
  .payslip-container {
    padding: 12px;
  }
  table, th, td {
    font-size: 0.95rem;
  }
}
</style>
