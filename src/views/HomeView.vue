<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { cn } from '@/lib/utils';

const current = ref<number>(0)
const interval = ref(0)

const stages = [
  { title: 'Title 1', description: 'Description 1' },
  { title: 'Title 2', description: 'Description 2' },
  { title: 'Title 3', description: 'Description 3' },
]

function clampAndRestart(num: typeof current, max: number): number {
  if (num.value >= max) {
    return num.value = 0 // go back to 0 if next value excedes max
  }
  return num.value++
}

function setStage(stage: number) {
  clearInterval(interval.value)
  current.value = stage
}

onMounted(() => {
  interval.value = setInterval(() => clampAndRestart(current, 2), 8_000)
})
</script>

<template>
  <main>
    <h1>Playground</h1>

    <section class="flex justify-between">
      <div>
        <p 
          @click="setStage(i)" 
          :class="cn(`text-xl opacity-50 cursor-pointer hover:opacity-80`, current == i && 'text-red-500 font-bold opacity-100')" 
          v-for="stage, i in stages"
        >
          {{ stage.title }}
        </p>
      </div>
      <div>
        <div v-for="stage, i in stages" >
            <p v-if="current === i">
              {{ stage.description }}
            </p>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped>
p {
  transition: color .8s ease-out
}

</style>
