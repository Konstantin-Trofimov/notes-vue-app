<script setup lang="ts">
  import  { Ref, ref } from 'vue'

  const isModalShow = ref(false)
  const newNote = ref('')
  const errorMessage = ref('')

  interface Note {
    id: number,
    text: string,
    date: Date,
    backgroundColor: string
  }

  const arr: Note[] = []

  const notes = ref(arr) as Ref<Note[]>

  function handleAddNote(): void | string {
    if (newNote.value.length < 9) {
      return errorMessage.value = 'Note needs to be 10 characters or more'
    }

    notes.value.push({
      id: Math.floor(Math.random() * 1_000_000),
      text: newNote.value,
      date: new Date(),
      backgroundColor: getRandomColor()
    })

    isModalShow.value = false
    newNote.value = ''
  }

  function getRandomColor(): string {
    const color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
    return color;
  }
</script>

<template>
  <main> 
    <div v-if="isModalShow" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="handleAddNote">Add Note</button>
        <button @click="isModalShow = false" class="close">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes {{ isModalShow }}</h1>
        <button @click="isModalShow = true">+</button>
      </header>
      <div class="card-container">
        <div v-for="{id, text, date, backgroundColor} in notes" :key="id" :style="{backgroundColor}" class="card">
          <p class="main-text">{{ text }}</p>
          <p class="date">{{ date.toLocaleDateString('en-Us') }}</p>
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
  align-items: center;
  justify-content: space-between;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(20, 20, 21);
  border-radius: 100%;
  font-size: 22px;
  color: white;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .77);
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

.modal textarea {
  padding: 10px;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.modal .close {
  margin-top: 7px;
  background-color: rgb(187, 56, 56);
}

.modal {
  color:  red;
}
</style>