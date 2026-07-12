<script setup>
import { ref } from 'vue'

const initialFormData = () => ({
  username: '',
  password: '',
  isAustralian: false,
  gender: '',
  reason: '',
})

const formData = ref(initialFormData())
const submittedCards = ref([])

const submitForm = () => {
  submittedCards.value.push({
    ...formData.value,
  })
}

const clearForm = () => {
  formData.value = initialFormData()
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
                />
              </div>

              <div class="col-12 col-md-6">
                <label for="password" class="form-label">Password</label>
                <input
                  id="password"
                  v-model="formData.password"
                  type="password"
                  class="form-control"
                />
              </div>

              <div class="col-12 col-md-6">
                <div class="form-check resident-check">
                  <input
                    id="isAustralian"
                    v-model="formData.isAustralian"
                    type="checkbox"
                    class="form-check-input"
                  />
                  <label for="isAustralian" class="form-check-label">
                    Australian Resident?
                  </label>
                </div>
              </div>

              <div class="col-12 col-md-6">
                <label for="gender" class="form-label">Gender</label>
                <select id="gender" v-model="formData.gender" class="form-select">
                  <option value="">Please select one</option>
                  <option value="Female">Female</option>
                  <option value="Male">Male</option>
                  <option value="Other">Other</option>
                </select>
              </div>

              <div class="col-12">
                <label for="reason" class="form-label">Reason for joining</label>
                <textarea
                  id="reason"
                  v-model="formData.reason"
                  class="form-control"
                  rows="3"
                ></textarea>
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

    <section v-if="submittedCards.length" class="submitted-section mt-5">
      <div class="row g-3">
        <div
          v-for="(card, index) in submittedCards"
          :key="index"
          class="col-12 col-sm-6 col-lg-4"
        >
          <article class="card h-100 user-card">
            <div class="card-header">User Information</div>
            <ul class="list-group list-group-flush">
              <li class="list-group-item">
                <strong>Username:</strong> {{ card.username }}
              </li>
              <li class="list-group-item">
                <strong>Password:</strong> {{ card.password }}
              </li>
              <li class="list-group-item">
                <strong>Australian Resident:</strong>
                {{ card.isAustralian ? 'Yes' : 'No' }}
              </li>
              <li class="list-group-item"><strong>Gender:</strong> {{ card.gender }}</li>
              <li class="list-group-item">
                <strong>Reason:</strong> {{ card.reason }}
              </li>
            </ul>
          </article>
        </div>
      </div>
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

.user-card {
  border: 1px solid #dee2e6;
  border-radius: 8px;
  box-shadow: 0 0.35rem 1rem rgba(33, 37, 41, 0.08);
  overflow: hidden;
}

.user-card .card-header {
  background: #e9f2ff;
  color: #0d3b66;
  font-weight: 700;
}

.list-group-item {
  overflow-wrap: anywhere;
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
