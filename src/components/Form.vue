<script setup>
import { ref } from 'vue'
import Column from 'primevue/column'
import DataTable from 'primevue/datatable'

const initialFormData = () => ({
  username: '',
  password: '',
  isAustralian: false,
  gender: '',
  reason: '',
})

const formData = ref(initialFormData())
const submittedUsers = ref([])
const errors = ref({
  username: null,
  password: null,
  resident: null,
  gender: null,
  reason: null,
})

const resetErrors = () => {
  errors.value = {
    username: null,
    password: null,
    resident: null,
    gender: null,
    reason: null,
  }
}

const validateName = () => {
  const username = formData.value.username.trim()
  errors.value.username = username.length >= 3 ? null : 'Name must be at least 3 characters.'
  return !errors.value.username
}

const validatePassword = () => {
  const password = formData.value.password

  if (password.length < 8) {
    errors.value.password = 'Password must be at least 8 characters long.'
  } else if (!/[A-Z]/.test(password)) {
    errors.value.password = 'Password must contain at least one uppercase letter.'
  } else if (!/[a-z]/.test(password)) {
    errors.value.password = 'Password must contain at least one lowercase letter.'
  } else if (!/\d/.test(password)) {
    errors.value.password = 'Password must contain at least one number.'
  } else if (!/[^A-Za-z0-9]/.test(password)) {
    errors.value.password = 'Password must contain at least one special character.'
  } else {
    errors.value.password = null
  }

  return !errors.value.password
}

const validateResident = () => {
  errors.value.resident = formData.value.isAustralian
    ? null
    : 'You must confirm that you are an Australian resident.'
  return !errors.value.resident
}

const validateGender = () => {
  errors.value.gender = formData.value.gender ? null : 'Please select a gender.'
  return !errors.value.gender
}

const validateReason = () => {
  const reasonLength = formData.value.reason.trim().length

  if (!reasonLength || reasonLength < 10) {
    errors.value.reason = 'Reason must be at least 10 characters.'
  } else if (reasonLength > 200) {
    errors.value.reason = 'Reason must not exceed 200 characters.'
  } else {
    errors.value.reason = null
  }

  return !errors.value.reason
}

const validateForm = () => [
  validateName(),
  validatePassword(),
  validateResident(),
  validateGender(),
  validateReason(),
].every(Boolean)

const submitForm = () => {
  if (!validateForm()) {
    return
  }

  submittedUsers.value = [
    ...submittedUsers.value,
    {
      username: formData.value.username.trim(),
      password: '********',
      isAustralian: formData.value.isAustralian,
      gender: formData.value.gender,
      reason: formData.value.reason.trim(),
    },
  ]

  clearForm()
}

const clearForm = () => {
  formData.value = initialFormData()
  resetErrors()
}
</script>

<template>
  <div class="container mt-5 mb-5">
    <div class="row">
      <div class="col-12 col-md-8 offset-md-2">
        <section class="form-panel">
          <h1 class="text-center mb-4">User Information Form</h1>

          <form @submit.prevent="submitForm">
            <div class="row g-3">
              <div class="col-12 col-md-6">
                <label for="username" class="form-label">Username</label>
                <input
                  id="username"
                  v-model="formData.username"
                  type="text"
                  class="form-control"
                  :class="{ 'is-invalid': errors.username }"
                  @blur="validateName"
                />
                <div v-if="errors.username" class="text-danger mt-1">
                  {{ errors.username }}
                </div>
              </div>

              <div class="col-12 col-md-6">
                <label for="password" class="form-label">Password</label>
                <input
                  id="password"
                  v-model="formData.password"
                  type="password"
                  class="form-control"
                  :class="{ 'is-invalid': errors.password }"
                  @blur="validatePassword"
                />
                <div v-if="errors.password" class="text-danger mt-1">
                  {{ errors.password }}
                </div>
              </div>

              <div class="col-12 col-md-6">
                <div class="form-check resident-check">
                  <input
                    id="isAustralian"
                    v-model="formData.isAustralian"
                    type="checkbox"
                    class="form-check-input"
                    :class="{ 'is-invalid': errors.resident }"
                    @change="validateResident"
                  />
                  <label for="isAustralian" class="form-check-label">
                    Australian Resident?
                  </label>
                  <div v-if="errors.resident" class="text-danger mt-1">
                    {{ errors.resident }}
                  </div>
                </div>
              </div>

              <div class="col-12 col-md-6">
                <label for="gender" class="form-label">Gender</label>
                <select
                  id="gender"
                  v-model="formData.gender"
                  class="form-select"
                  :class="{ 'is-invalid': errors.gender }"
                  @change="validateGender"
                >
                  <option value="">Please select one</option>
                  <option value="Female">Female</option>
                  <option value="Male">Male</option>
                  <option value="Other">Other</option>
                </select>
                <div v-if="errors.gender" class="text-danger mt-1">
                  {{ errors.gender }}
                </div>
              </div>

              <div class="col-12">
                <label for="reason" class="form-label">Reason for joining</label>
                <textarea
                  id="reason"
                  v-model="formData.reason"
                  class="form-control"
                  rows="3"
                  :class="{ 'is-invalid': errors.reason }"
                  @blur="validateReason"
                ></textarea>
                <div v-if="errors.reason" class="text-danger mt-1">
                  {{ errors.reason }}
                </div>
              </div>

              <div class="col-12">
                <div class="d-flex justify-content-center gap-3 flex-wrap">
                  <button type="submit" class="btn btn-primary">Submit</button>
                  <button type="button" class="btn btn-secondary" @click="clearForm">
                    Clear
                  </button>
                </div>
              </div>
            </div>
          </form>
        </section>
      </div>
    </div>

    <section v-if="submittedUsers.length" class="submitted-section mt-5">
      <h2 class="mb-3">Submitted User Information</h2>
      <DataTable
        :value="submittedUsers"
        dataKey="username"
        paginator
        :rows="5"
        responsiveLayout="scroll"
        stripedRows
        tableStyle="min-width: 50rem"
      >
        <Column field="username" header="Username" />
        <Column field="password" header="Password" />
        <Column header="Australian Resident">
          <template #body="{ data }">
            {{ data.isAustralian ? 'Yes' : 'No' }}
          </template>
        </Column>
        <Column field="gender" header="Gender" />
        <Column field="reason" header="Reason" />
      </DataTable>
    </section>
  </div>
</template>

<style scoped>
.form-panel {
  background: #ffffff;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  box-shadow: 0 0.5rem 1.25rem rgba(33, 37, 41, 0.08);
  padding: 1.5rem;
}

.form-panel h1 {
  color: #1f2937;
  font-size: clamp(1.75rem, 3vw, 2.25rem);
  font-weight: 700;
}

.resident-check {
  padding-top: 2.25rem;
}

.submitted-section h2 {
  color: #1f2937;
  font-size: 1.5rem;
  font-weight: 700;
}

@media (max-width: 767.98px) {
  .form-panel {
    padding: 1rem;
  }

  .resident-check {
    padding-top: 0;
  }
}
</style>
