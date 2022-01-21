<template>
  <div class="mt-4 flex justify-center">
    <button
      v-for="(type, index) in types"
      :key="index"
      class="bg-green-500 px-6 py-2 mx-2 rounded font-bold"
      :class="{ 'text-white bg-green-600': speed == type }"
      @click="speed = type"
    >{{ type }}</button>
  </div>
  <div class="mt-4 flex justify-center items-center">
    <button v-if="isShuffled" @click="initialDeck" class="px-6 py-2 mx-2 rounded font-bold outline outline-green-500">
      Reset
    </button>
    <button @click="shuffle" class="bg-green-500 px-6 py-2 mx-2 rounded font-bold text-white">
      Shuffle
    </button>
  </div>
  <transition-group :name="`shuffle-${speed}`" tag="div" class="mt-4">
    <div v-for="card in cards" :key="card.id" class="w-[100px] h-[120px] float-left mr-5 mb-5 rounded bg-white shadow-md" :class="suitColor[card.suit]">
      <span class="flex justify-start px-3 py-2 text-lg">{{ card.suit }}</span>
      <span class="flex justify-center items-center text-lg font-bold">{{ card.rank }}</span>
      <span class="flex justify-start px-3 py-2 text-lg transform rotate-180">{{ card.suit }}</span>
    </div>
  </transition-group>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  setup(props) {
    const ranks = ['A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K']
    const suits = ['♥', '♦', '♠', '♣']
    const suitColor = {
      '♠': 'text-black',
      '♣': 'text-black',
      '♦': 'text-red-600',
      '♥': 'text-red-600',
    }
    const cards = ref([])
    const speed = ref('medium')
    const types = ref(['slow', 'medium', 'fast'])
    const isShuffled = ref(false)

    function initialDeck() {
      cards.value = [];
      let index = 1

      suits.forEach(suit => {
        ranks.forEach((rank) => {
          cards.value.push({
            id: index,
            rank: rank,
            suit: suit
          })

          index++
        })
      })

      isShuffled.value = false;
    }
    function shuffle(array) {
      const length = cards.value.length
      const lastIndex = length - 1
      let index = -1

      while (++index < length) {
        const rand = index + Math.floor(Math.random() * (lastIndex - index + 1))
        const value = cards.value[rand]
        cards.value[rand] = cards.value[index]
        cards.value[index] = value
      }

      isShuffled.value = true;
      return cards
    }

    onMounted(() => initialDeck())
    

    return {
      ranks,
      suits,
      suitColor,
      cards,
      speed,
      types,
      isShuffled,
      initialDeck,
      shuffle,
    }
  }
}
</script>

<style scoped >
.shuffle-slow-move {
  transition: transform 2s;
}

.shuffle-medium-move {
  transition: transform 1s;
}

.shuffle-fast-move {
  transition: transform 0.5s;
}
</style>