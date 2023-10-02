<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);

function getRandomColor() {
  let color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  return color;
}

let message = ref("");

const addNode = () => {
  if (newNote.value.length < 10) {
    return message.value = "Minimum 10 Charactars";
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date().toLocaleDateString("de-DE"),
    color: getRandomColor(),
  });
  showModal.value = false;
  newNote.value = "";
  message.value = ""
};
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <p v-if="message">{{ message }}</p>
        <textarea v-model.trim="newNote" name="note" id="node" cols="30" rows="10"></textarea>
        <button @click="addNode">Add Note</button>
        <button class="close" @click="showModal = false">x</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = !showModal">+</button>
      </header>
      <div class="card-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{ backgroundColor: note.color }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: 700;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(53, 52, 52);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: aqua;
  padding: 10px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-bottom: 20px;
  margin-right: 20px;
}

.date {
  font-size: medium;
  font-weight: bold;
}

.container .card-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: rgb(167, 91, 243);
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 10px;
}

.modal .close {
  background-color: rgb(230, 59, 59);
  position: absolute;
  top: -37px;
  right: -16px;
  width: 50px;
  height: 50px;
  border-radius: 100%;
}

.main-text {
  width: 100%;
  height: 80%;
  word-wrap: break-word;
}
</style>
