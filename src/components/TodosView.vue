<script setup>
import { reactive } from 'vue';
import InputNewView from './InputNewView.vue';

let boards = reactive([
  {
    id: crypto.randomUUID(),
    name: 'Board 1',
    items: [
      {
        id: crypto.randomUUID(),
        title: 'Feature de archivos'
      },
      {
        id: crypto.randomUUID(),
        title: 'resolver bugs'
      },
    ]
  },
  {
    id: crypto.randomUUID(),
    name: 'Board 2',
    items: [
      {
        id: crypto.randomUUID(),
        title: 'mandar report'
      },
      {
        id: crypto.randomUUID(),
        title: 'code review'
      },
    ]
  },
])

const handleNewItem = (text, board) => {
  board.items.push({
    id: crypto.randomUUID(),
    title: text,
  })
}

const handleNewBoard = () => {
  const name = prompt('Name of the board')
  if (!!name) {
    boards.push({
      id: crypto.randomUUID(),
      name,
      items: []
    })
  }
}

function startDrag(evt, board, item) {
  // Colocamos la info del drag, vamos a enviar la data que recibimos en el onDrop
  evt.dataTransfer.setData(
    "text/plain",
    JSON.stringify({ boardId: board.id, itemId: item.id })
  );
}

function onDrop(evt, dest) {
  // Recogemos la info del drag y la pasamos a la nueva board
  const { boardId, itemId } = JSON.parse(evt.dataTransfer.getData("text/plain"))
  const originBoard = boards.find(item => item.id === boardId)
  const originItem = originBoard.items.find(item => item.id === itemId)

  dest.items.push({ ...originItem })
  originBoard.items.splice(originBoard.items.indexOf(originItem), 1)
}
</script>

<template>
  <nav>
    <ul>
      <li>
        <a href="#" @click.prevent="handleNewBoard">Create board</a>
      </li>
    </ul>
  </nav>

  <div class="boards-container">
    <div class="boards">
      <div class="board" @drop="onDrop($event, board)" @dragover.prevent @dragenter.prevent v-for="(board) in boards"
        :key="board.id">
        <div>{{ board.name }}</div>

        <InputNewView @on-new-item="(text) => handleNewItem(text, board)" />
        <div class="items">
          <div class="item" draggable="true" @dragstart="startDrag($event, board, item)" v-for="item in board
            .items" :key="item.id">
            {{ item.title }}
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<style scoped>
nav {
  background: #181818;
  margin-bottom: 10px;
}

nav ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
}

nav ul li a {
  display: block;
  padding: 10px;
  color: white;
}

.boards {
  display: flex;
  gap: 10px;
}

.board {
  /* background: #efefef; */
  padding: 10px;
  border: 1px solid gray;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.items {
  display: flex;
  flex-direction: column;
  gap: 5px;
  /* border: 1px solid gray; */
}

.item {
  padding: 10px;
  box-sizing: border-box;
  cursor: pointer;
  border: 1px solid #ccc;
}

form {
  margin-bottom: 10px;
}

input {
  width: 100%;
  box-sizing: border-box;
}
</style>