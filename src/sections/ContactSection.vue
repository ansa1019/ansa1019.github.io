<template>
  <section :id="id" class="section contact">
    <h2 class="title">Contact</h2>
    <div class="contact-container">
      <div class="contact-info">
        <div class="contact-item">
          <i class="fas fa-phone"></i>
          <a href="tel:+886900152125">0900-152-125</a>
        </div>
        <div class="contact-item">
          <i class="fas fa-envelope"></i>
          <a href="mailto:ansa1019@gmail.com">ansa1019@gmail.com</a>
        </div>
        <div class="contact-item">
          <i class="fab fa-linkedin"></i>
          <a href="https://www.linkedin.com/in/ansa1019" target="_blank">LinkedIn</a>
        </div>
        <div class="contact-item">
          <i class="fab fa-github"></i>
          <a href="https://github.com/ansa1019" target="_blank">GitHub</a>
        </div>
      </div>

      <form class="contact-form" @submit.prevent="submitForm">
        <div class="form-row">
          <div class="form-group">
            <label for="name">Name</label>
            <input v-model="form.name" type="text" id="name" name="name" required />
          </div>
          <div class="form-group">
            <label for="email">Email</label>
            <input v-model="form.email" type="email" id="email" name="email" required />
          </div>
        </div>

        <div class="form-group">
          <label for="message">Message</label>
          <textarea v-model="form.message" id="message" name="message" rows="5" required></textarea>
        </div>

        <button type="submit" :disabled="loading">
          {{ loading ? "Sending..." : "Send" }}
        </button>

        <p v-if="status === 'success'" class="success-msg">✅ Message sent successfully!</p>
        <p v-if="status === 'error'" class="error-msg">❌ Something went wrong. Please try again.</p>
      </form>
    </div>
  </section>
</template>

<script setup lang="ts">
import { reactive, ref } from "vue"
defineProps<{ id: string }>()

const form = reactive({
  name: "",
  email: "",
  message: ""
})

const status = ref<"idle" | "success" | "error">("idle")
const loading = ref(false)

const submitForm = async () => {
  loading.value = true
  status.value = "idle"

  try {
    const response = await fetch("https://formspree.io/f/movkjlvz", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(form)
    })

    if (response.ok) {
      status.value = "success"
      form.name = ""
      form.email = ""
      form.message = ""
    } else {
      status.value = "error"
    }
  } catch (e) {
    status.value = "error"
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.contact-container {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 2rem;
  max-width: 900px;
  margin: 0 auto;
  align-items: start;
}

.contact-info {
  display: flex;
  flex-direction: column;
  padding-top: 1rem;
  gap: 2rem;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  color: var(--primary);
}

.contact-item a {
  text-decoration: none;
}

.contact-item i {
  font-size: 1.2rem;
}

.contact-form {
  display: flex;
  flex-direction: column;
  gap: 1.2rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.contact-form input,
.contact-form textarea {
  padding: 0.8rem;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 1rem;
}

.contact-form button {
  background: var(--primary);
  color: white;
  border: none;
  padding: 0.8rem;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s ease;
}

.contact-form button:hover {
  background: var(--primary);
}

.contact-form button:disabled {
  background: #aaa;
  cursor: not-allowed;
}

.success-msg {
  color: green;
  font-size: 0.9rem;
}

.error-msg {
  color: red;
  font-size: 0.9rem;
}

@media (max-width: 768px) {
  .contact-container {
    width: 80%;
    grid-template-columns: 1fr;
  }

  .form-row {
    grid-template-columns: 1fr;
  }
}
</style>
