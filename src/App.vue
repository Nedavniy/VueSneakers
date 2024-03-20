<script setup>
import { onMounted, ref, watch } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref({})

const sortBy = ref('')
const searchQuery = ref('')

const onChangeSelect = (event) => {
  sortBy.value = event.target.value
}
onMounted(async () => {
  try {
    const { data } = await axios.get('https://85c60f68c644cabb.mokky.dev/items')
    items.value = data
  } catch (err) {
    console.log(err)
  }
})

watch(sortBy, async () => {
  try {
    const { data } = await axios.get(
      'https://85c60f68c644cabb.mokky.dev/items?sortBy=' + sortBy.value
    )
    items.value = data
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <!-- <Drawer /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">All sneakers</h2>

        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">By name</option>
            <option value="price">By price (low to high)</option>
            <option value="-price">By price (high to low)</option>
          </select>

          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="Search" />
            <input
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
              type="text"
              placeholder="Search"
            />
          </div>
        </div>
      </div>

      <div class="mt-10">
        <CardList :items="items" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
