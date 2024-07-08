<script setup>
import axios from 'axios'
import { watch } from 'vue'
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const inputValue = ref('')
const options = ref([])
const isLoading = ref(false)
const search = ref('')

const onInputChange = (event) => {
  search.value = event.target.value
}

watch(search, async (newVal, oldVal) => {
  if (newVal == '') {
    options.value = []
  } else {
    try {
      const response = await axios.get(`https://restcountries.com/v3.1/name/` + newVal)
      if (response.data.length > 5) {
        options.value = response.data.slice(0, 5)
      } else {
        options.value = response.data
      }
    } catch (error) {
      console.error(error)
      options.value = []
    }
  }
})

watch(inputValue, (newVal, oldVal) => {
  router.push('/' + newVal)
})
</script>

<template>
  <main>
    <v-container class="w-screen h-full">
      <v-row justify="center">
        <v-col>
          <div class="text-center">
            <h1 className="text-5xl font-bold mb-4">Country</h1>
            <v-autocomplete
              v-model="inputValue"
              :items="options"
              item-title="name.common"
              item-value="name.common"
              :loading="isLoading"
              label="Search"
              placeholder="Start typing to Search"
              variant="outlined"
              @input="onInputChange($event)"
            ></v-autocomplete>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </main>
</template>
