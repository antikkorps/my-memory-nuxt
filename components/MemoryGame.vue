<script setup lang="ts">
import { ref, onMounted } from "vue"

interface Card {
  id: number
  image: string
  flipped: boolean
  matched: boolean
}

const images = [
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
let selectedCards: Card[] = []
let isProcessing = false
const counter = ref(0)

const startGame = () => {
  counter.value = 0
  const doubledImages = [...images, ...images]
  const shuffledImages = doubledImages.sort(() => Math.random() - 0.5)

  cards.value = shuffledImages.map((image, index) => ({
    id: index,
    image: image,
    flipped: false,
    matched: false,
  }))
}

const resetGame = () => {
  cards.value = []
  startGame()
}

const flipCard = (card: Card) => {
  if (isProcessing || card.flipped) return

  card.flipped = true
  selectedCards.push(card)

  if (selectedCards.length === 2) {
    isProcessing = true
    counter.value++
    console.log("nombre de coups", counter.value)
    const [firstCard, secondCard] = selectedCards

    if (firstCard.image !== secondCard.image) {
      setTimeout(() => {
        firstCard.flipped = false
        secondCard.flipped = false
        selectedCards = []
        isProcessing = false
      }, 1000)
    } else {
      firstCard.matched = true
      secondCard.matched = true
      selectedCards = []
      isProcessing = false

      if (cards.value.every((card) => card.matched)) {
        showModal()
      }
    }
  }
}

const showModal = () => {
  const modal = document.getElementById("my_modal_5") as HTMLDialogElement

  if (modal) {
    modal.showModal()
  }
}

onMounted(() => {
  startGame()
})
</script>
<template>
  <div>
    <h1 class="text-center font-bold text-4xl my-4">Memory Game</h1>

    <div class="flex justify-center mx-auto my-5">
      <button @click="startGame" class="btn btn-outline btn-success mr-2">
        Commencer une partie
      </button>
      <button @click="resetGame" class="btn btn-warning">Recommencer</button>
    </div>
    <div class="flex justify-center items-center relative top-20 sm:top-16 mx-auto">
      <div
        class="grid grid-cols-4 grid-rows-4 md:grid-cols-2 lg:grid-cols-4 lg:grid-rows-4 sm:w-4/5 gap-2 mx-auto justify-items-center justify-self-center"
      >
        <div v-for="card in cards" :key="card.id" class="w-30 h-30 sm:w-50 sm:h-50">
          <div @click="flipCard(card)" class="w-16 h-16 sm:w-48 sm:h-48">
            <NuxtImg
              v-if="card.flipped || card.matched"
              :src="card.image"
              class="object-cover w-full h-full"
            />
            <div v-else class="bg-gray-400 w-16 h-16 sm:w-48 sm:h-48"></div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <dialog id="my_modal_5" class="modal modal-bottom sm:modal-middle">
    <div class="modal-box">
      <h3 class="font-bold text-lg">Félicitation !</h3>
      <p class="py-4">Vous avez finalisé la grille en {{ counter }} coups</p>
      <form method="dialog">
        <!-- if there is a button in form, it will close the modal -->
        <button class="btn">fermer</button>
      </form>
    </div>
  </dialog>
</template>
