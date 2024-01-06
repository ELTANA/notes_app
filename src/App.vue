<script setup>
import { ref } from 'vue'

const showModal = ref(false)
const newNote = ref('')
const errorMessage = ref('')
const notes = ref([])

const getRandomColor = () => {
  let color = 'hsl(' + Math.random() * 360 + ', 100%, 75%)'
  return color
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = 'Text must be at least 10 characters long')
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  })
  newNote.value = ''
  errorMessage.value = ''
  showModal.value = false
}
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <button @click="showModal = false" class="close" type="button">
          &times;
        </button>

        <div class="textField">
          <textarea
            v-model.trim="newNote"
            name="note"
            id="note"
            cols="note"
            rows="10"
          ></textarea>
          <p v-if="errorMessage" class="errorMessage">{{ errorMessage }}</p>
        </div>

        <button @click="addNote">Add Note</button>
      </div>
    </div>

    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>

      <div class="card-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString('en-US') }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100dvh;
  width: 100%;
  background-color: antiquewhite;
  color: midnightblue;
}

.container {
  width: 100%;
  max-width: 1200px;
  padding: 1rem;
  margin: 0 auto;
}

header {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

h1 {
  font-weight: bold;
  margin-block-end: 1.5rem;
  font-size: 5rem;
}

textarea {
  width: 100%;
}

button {
  cursor: pointer;
  background-color: #00008bcc;
  color: whitesmoke;
  font-size: 2rem;
  border-radius: 0.5rem;
  border: transparent;
  outline: transparent;
  padding-block: 1rem;
  padding-inline: 2rem;
  transition: all 300ms ease-in;
}

button:hover {
  background-color: darkblue;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 1.2rem;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(0, 0, 255);
  padding: 0.8rem;
  border-radius: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 1.2rem;
}

.date {
  font-size: 0.8rem;
  font-weight: bold;
}

.overlay {
  width: 100%;
  height: 100%;
  position: absolute;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2;
}

.modal {
  width: 70%;
  max-width: 750px;
  background-color: white;
  border-radius: 0.8rem;
  padding: 2rem;
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}
.modal > button {
  font-size: 1.5rem;
}

.close {
  cursor: pointer;
  width: 40px;
  height: 40px;
  background-color: whitesmoke;
  color: red;
  font-size: 2rem;
  padding: 0;
  padding-block-end: 0.2rem;
  align-self: last baseline;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 100%;
  border: transparent;
  outline: transparent;
  transition: all 350ms ease-in;
}

.close:hover {
  background-color: red;
  color: white;
}

.textField {
  width: 100%;
}

.errorMessage {
  background-color: rgba(255, 0, 0, 0.2);
  color: red;
  font-size: 0.8rem;
  padding-block: 0.5rem;
  padding-inline: 1rem;
  margin-block: 0.5rem;
  border-radius: 0.5rem;
}
</style>
