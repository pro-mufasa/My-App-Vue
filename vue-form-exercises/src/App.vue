<script setup>
import { ref } from 'vue'

const inputName = ref('')
const displayName = ref('')

function handleFormSubmit() {
  displayName.value = inputName.value
}
</script>

<template>
  <div class="form-container">
    <form @submit.prevent="handleFormSubmit">
      <h2>User Information Form</h2>
      
      <div class="form-group">
        <label for="username">Full Name</label>
        <input 
          id="username"
          v-model.trim="inputName" 
          type="text" 
          placeholder="Full Name"
          required
        />
      </div>
      
      <button type="submit">View Name</button>
    </form>

    <div class="output-section">
      <p v-if="displayName">
        Submitted Name: <strong>{{ displayName }}</strong>
      </p>
    </div>
  </div>
</template>

<style scoped>
.form-container {
  font-family: Arial, sans-serif;
  max-width: 350px;
  margin: 30px auto;
  padding: 25px;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
}
h2 { 
margin-top: 0; 
color: #333; 
font-size: 1.4rem; 
}
.form-group { 
margin-bottom: 15px; 
display: flex;
flex-direction: column; 
}
label { 
margin-bottom: 5px; 
font-weight: bold; 
color: #555; 
font-size: 0.9rem; 
}
input { 
padding: 10px; 
border: 1px solid #ccc; 
border-radius: 4px; 
font-size: 1rem; 
}
input:focus { 
border-color: #42b883; 
outline: none; 
}
button { 
width: 100%; 
padding: 10px; 
background-color: #42b883; 
color: white; border: none; 
border-radius: 4px; 
font-size: 1rem; 
font-weight: bold; 
cursor: pointer; 
}
button:hover { 
background-color: #35495e; 
}
.output-section { 
margin-top: 20px; 
border-top: 1px dashed #ccc; 
padding-top: 15px; 
min-height: 40px; 
}
</style>