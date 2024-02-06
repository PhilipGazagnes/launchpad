<script lang="ts" setup>
import padJson from '../data/padData.json'
import { ref } from 'vue'

const filters = ref({
  'hh-triangle': false,
  'hh-ride': false,
  'hh-shaker': false,
  'hh-bell': false,
  'hh-tambourine': false,
  'hh-accent-counter': false,
  'ternary': false,
  'chabada': false,
  'tempo-x-2': false,
})

const filterClass = active => `
  p-2 flex-auto border-2 border-white bg-none uppercase text-lg font-bold
  ${active ? 'bg-white text-black' : 'text-white'}
`

const touchClass = (color, tags) => {
  const classes = []
  if (color === 'red') classes.push('fill-red-400')
  if (color === 'blue') classes.push('fill-blue-400')
  if (color === 'yellow') classes.push('fill-yellow-400')
  // tags: a b c d
  let active = true

  Object.entries(filters.value).forEach(([key, val]) => {
    if (val && tags.indexOf(key) === -1) {
      active = false
    }
  })
  if (!active) classes.push('opacity-30')
  return classes
}

// Collect unique tag values
const uniqueTags = [...new Set(padJson.flatMap(obj => obj.tags.split(' ')))];

console.log(uniqueTags);

</script>

<template>
  <div class="h-screen flex justify-center items-center">
    <div class="flex gap-4">
      <div class="grid grid-rows-8 grid-cols-8 bg-black w-[600px] p-4">
        <div
          v-for="(touch, index) in padJson"
          :style="{ gridColumn: touch.x, gridRow: touch.y }"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 10 10"
            :class="touchClass(touch.color, touch.tags)"
          >
            <polygon v-if="touch.var === 'a'" points="0.5,0.5 9,0.5 0.5,9" />
            <polygon v-else points="9.5,9.5 9.5,1 1,9.5" />
          </svg>
        </div>
      </div>
      <div class="flex flex-col gap-1 w-[200px]">
        <button
          v-for="(filter, index) in Object.keys(filters)"
          :key="index" @click="filters[filter] = !filters[filter]"
          :class="filterClass(filters[filter])"
        >
          {{ filter }}
        </button>
      </div>
    </div>
  </div>
</template>