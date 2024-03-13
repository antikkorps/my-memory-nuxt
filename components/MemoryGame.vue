<script setup lang="ts">
import { ref, onMounted } from "vue"

interface Card {
  id: number
  image: string
  flipped: boolean
  matched: boolean
}

const image = [
  "https://images.unsplash.com/photo-1708355048964-1b52e054a3bd?q=80&w=2080&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1708982553355-794739c6693e?q=80&w=2125&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1578305872374-2c938aea61ed?q=80&w=2154&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1709431511239-e238335fa6ca?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1702591539493-e8baaab87f66?q=80&w=2088&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1533891244820-1d2dbcf93f56?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1517167685284-96a27681ad75?q=80&w=1974&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1708548593339-81421f769f0e?q=80&w=1965&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
]

const cards = ref<Card[]>([])

const startGame = () => {
  cards.value = image.map((img, index) => ({
    id: index,
    image: img,
    flipped: false,
    matched: false,
  }))
}

const resetGame = () => {
  cards.value = []
}

const flipCard = (card: Card) => {
  card.flipped = !card.flipped
}

onMounted(() => {
  startGame()
})
</script>
<template>
  <div>
    <h1>Memory Game</h1>
    <NuxtImg
      src="https://images.unsplash.com/photo-1708355048964-1b52e054a3bd?q=80&w=2080&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
      title="Memory Game"
      width="200"
      height="200"
    />
    <div>
      <button @click="startGame">Start Game</button>
      <button @click="resetGame">Reset Game</button>
    </div>
    <div class="grid grid-cols-3 gap-1 border-2 h-full">
      <div v-for="card in cards" :key="card.id" class="w-50 h-50">
        <div @click="flipCard(card)" class="w-50 h-50 bg-red-400">
          <NuxtImg
            v-if="card.flipped || card.matched"
            :src="card.image"
            class="object-cover w-full h-full"
          />
          <div v-else class="bg-gray-300 w-full h-full"></div>
        </div>
      </div>
    </div>
  </div>
</template>
