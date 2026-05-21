<template>
  <div id="app" class="container">
    <header>
      <h1>Job Agency Portal</h1>
      <nav class="tabs">
        <button 
          :class="{ active: currentTab === 'applicant' }" 
          @click="currentTab = 'applicant'"
        >
          Applicant Form
        </button>
        <button 
          :class="{ active: currentTab === 'admin' }" 
          @click="currentTab = 'admin'"
        >
          Administrator Dashboard ({{ applications.length }})
        </button>
      </nav>
    </header>
    <main>
      <section v-if="currentTab === 'applicant'" class="tab-content">
        <h2>Submit Your Application</h2>
        
        <div v-if="showSuccess" class="success-message">
          ✓ Application submitted successfully!
        </div>

        <form @submit.prevent="handleSubmit">
          <div class="form-group row">
            <div class="col">
              <label for="firstname">First Name:</label>
              <input type="text" id="firstname" v-model="form.firstname" required />
            </div>
            <div class="col">
              <label for="surname">Surname:</label>
              <input type="text" id="surname" v-model="form.surname" required />
            </div>
          </div>

          <div class="form-group">
            <label for="dob">Date of Birth (DOB):</label>
            <input type="date" id="dob" v-model="form.dob" required />
          </div>

          <div class="form-group">
            <label for="address">Address:</label>
            <textarea id="address" v-model="form.address" rows="3" required></textarea>
          </div>

          <div class="form-group">
            <label for="phone">Phone Number:</label>
            <input type="tel" id="phone" v-model="form.phone" required />
          </div>

          <div class="form-group">
            <label>Gender:</label>
            <div class="radio-group">
              <label><input type="radio" value="Male" v-model="form.gender" required /> Male</label>
              <label><input type="radio" value="Female" v-model="form.gender" /> Female</label>
              <label><input type="radio" value="Other" v-model="form.gender" /> Other</label>
            </div>
          </div>

          <div class="form-group">
            <label for="nationalId">National ID:</label>
            <input type="text" id="nationalId" v-model="form.nationalId" required />
          </div>

          <button type="submit" class="submit-btn">Submit Application</button>
        </form>
      </section>

      <section v-if="currentTab === 'admin'" class="tab-content">
        <h2>Submitted Applications</h2>
        
        <div v-if="applications.length === 0" class="no-data">
          No applicants yet.
        </div>

        <div v-else class="table-responsive">
          <table>
            <thead>
              <tr>
                <th>ID</th>
                <th>Full Name</th>
                <th>DOB</th>
                <th>Phone</th>
                <th>Gender</th>
                <th>National ID</th>
                <th>Address</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(app, index) in applications" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ app.firstname }} {{ app.surname }}</td>
                <td>{{ app.dob }}</td>
                <td>{{ app.phone }}</td>
                <td>{{ app.gender }}</td>
                <td>{{ app.nationalId }}</td>
                <td class="address-cell">{{ app.address }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
const currentTab = ref('applicant')

const applications = ref([])

const showSuccess = ref(false)

const initialFormState = {
  firstname: '',
  surname: '',
  dob: '',
  address: '',
  phone: '',
  gender: '',
  nationalId: ''
}
const form = reactive({ ...initialFormState })


const handleSubmit = () => {
  applications.value.push({ ...form })
  
  showSuccess.value = true
  setTimeout(() => {
    showSuccess.value = false
  }, 4000)
  Object.assign(form, initialFormState)
}
</script>

<style scoped>
.container {
  max-width: 900px;
  margin: 2rem auto;
  padding: 0 1rem;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  color: #333;
}

header {
  text-align: center;
  margin-bottom: 2rem;
}

h1 {
  color: #2c3e50;
  margin-bottom: 1.5rem;
}
.tabs {
  display: flex;
  justify-content: center;
  gap: 10px;
  border-bottom: 2px solid #e2e8f0;
  padding-bottom: 2px;
}

.tabs button {
  padding: 10px 20px;
  font-size: 1rem;
  cursor: pointer;
  background: transparent;
  border: none;
  border-bottom: 3px solid transparent;
  color: #718096;
  font-weight: 600;
  transition: all 0.2s ease;
}

.tabs button:hover {
  color: #3182ce;
}

.tabs button.active {
  color: #3182ce;
  border-bottom-color: #3182ce;
}

.tab-content {
  background: #ffffff;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.form-group {
  margin-bottom: 1.25rem;
}

.row {
  display: flex;
  gap: 15px;
}

.col {
  flex: 1;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
  font-size: 0.9rem;
}

input[type="text"],
input[type="date"],
input[type="tel"],
textarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #cbd5e0;
  border-radius: 5px;
  box-sizing: border-box;
  font-size: 1rem;
}

input:focus, textarea:focus {
  outline: none;
  border-color: #3182ce;
  box-shadow: 0 0 0 3px rgba(66, 153, 225, 0.5);
}

.radio-group {
  display: flex;
  gap: 20px;
  padding: 5px 0;
}

.radio-group label {
  display: flex;
  align-items: center;
  gap: 5px;
  cursor: pointer;
}

.submit-btn {
  background-color: #3182ce;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  font-weight: 600;
  width: 100%;
  margin-top: 1rem;
}

.submit-btn:hover {
  background-color: #2b6cb0;
}

.success-message {
  background-color: #c6f6d5;
  color: #22543d;
  padding: 12px;
  border-radius: 5px;
  margin-bottom: 1.5rem;
  font-weight: 500;
  text-align: center;
}

.no-data {
  text-align: center;
  padding: 3rem;
  color: #a0aec0;
  font-style: italic;
  border: 2px dashed #e2e8f0;
  border-radius: 6px;
}

.table-responsive {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1rem;
  text-align: left;
}

th, td {
  padding: 12px 15px;
  border-bottom: 1px solid #e2e8f0;
  font-size: 0.95rem;
}

th {
  background-color: #f7fafc;
  color: #4a5568;
  font-weight: 600;
}

tr:hover {
  background-color: #f8fafc;
}

.address-cell {
  max-width: 200px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>