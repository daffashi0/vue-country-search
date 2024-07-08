<script setup>
import { computed, onMounted, ref, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import axios from 'axios'
import { mdiArrowLeft } from '@mdi/js'
import CardCoordinates from '@/components/CardCoordinates.vue'
import CardGeography from '@/components/CardGeography.vue'

const route = useRoute()
const router = useRouter()
const countryData = ref({})
const callingCodeCountry = ref([])
const currencyCountry = ref([])

const filterCallingCode = (idd) => {
  const length = idd.root.length
  const root = idd.root.substr(1, length - 1)
  let callingCode = root
  idd.suffixes.map((v) => {
    callingCode = callingCode.concat(v)
  })

  return callingCode
}

const onBack = () => {
  router.replace('/')
}

const callingCode = ref(0)
const currency = ref(0)

const getCallingCodeCountry = async (callingCodeNumber) => {
  try {
    const response = await axios.get(
      `https://restcountries.com/v2/callingcode/${callingCodeNumber}`
    )
    callingCodeCountry.value = response.data
  } catch (error) {
    console.error(error)
    callingCodeCountry.value = []
  }
}

const getCurrencyCountry = async (currencyCountryNumber) => {
  try {
    const response = await axios.get(
      `https://restcountries.com/v2/currency/${currencyCountryNumber}`
    )
    currencyCountry.value = response.data
  } catch (error) {
    console.error(error)
    currencyCountry.value = []
  }
}
onMounted(() => {
  const name = route.path.substring(1)
  const fetch = async () => {
    try {
      const response = await axios.get(`https://restcountries.com/v3.1/name/${name}?fullText=true`)
      countryData.value = response.data[0]
      callingCode.value = filterCallingCode(response.data[0].idd)
      currency.value = Object.keys(response.data[0]?.currencies)[0]
      await getCallingCodeCountry(callingCode.value)
      await getCurrencyCountry(currency.value)
    } catch (error) {
      console.error(error)
      countryData.value = null
    }
  }
  fetch()
})
</script>
<template>
  <div className="flex flex-col w-full h-screen p-16" v-if="countryData">
    <div>
      <v-btn color="deep-purple-darken-1" @click="onBack()" variant="elevated"
        >Back To Homepage</v-btn
      >
    </div>
    <div className="flex flex-row gap-2 mt-10 items-center">
      <h1 className="text-5xl font-bold">{{ countryData?.name?.common }}</h1>
      <img :src="countryData?.flags?.png" className="w-auto h-10" />
    </div>
    <div className="flex flex-row gap-2 items-center mt-2">
      <v-chip
        v-for="(v, i) in countryData.altSpellings"
        :key="i"
        class="font-medium px-1 py-2"
        color="blue-lighten-1"
        >{{ v }}</v-chip
      >
    </div>
    <div className="flex flex-row gap-6 mt-4 mr-20">
      <div className="flex-1" v-if="Object.keys(countryData)[0]">
        <CardCoordinates :latlng="countryData.latlng" />
      </div>
      <div className="flex-1" v-if="Object.keys(countryData)[0]">
        <CardGeography
          :capital="countryData.capital"
          :region="countryData.region"
          :subregion="countryData.subregion"
        />
      </div>
    </div>
    <div className="flex flex-row gap-6 mt-4 mr-20">
      <div className="flex-1">
        <h3 className="text-xl font-medium">Calling Code</h3>
        <h1 className="text-4xl font-medium underline text-purple-500">
          {{ callingCode }}
        </h1>
        <div className="inline-block">
          <v-tooltip location="top">
            <template v-slot:activator="{ props }">
              <h3 class="text-lg font-bold underline text-purple-500 inline-block" v-bind="props">
                {{
                  callingCodeCountry.length > 1
                    ? `${callingCodeCountry.length} countries `
                    : `${callingCodeCountry.length} country `
                }}
              </h3>
            </template>
            <span v-for="(v, i) in callingCodeCountry" :key="i">
              {{ v.name }}
              <br v-if="i !== callingCodeCountry.length - 1" />
            </span>
          </v-tooltip>
          <p className="inline-block text-lg font-medium">&ensp;with this calling code</p>
        </div>
      </div>
      <div className="flex-1">
        <h3 className="text-xl font-medium">Currency</h3>
        <h1 className="text-4xl font-medium underline text-purple-500">
          {{ currency }}
        </h1>
        <div className="inline-block">
          <v-tooltip location="top">
            <template v-slot:activator="{ props }">
              <h3 class="text-lg font-bold underline text-purple-500 inline-block" v-bind="props">
                {{
                  currencyCountry.length > 1
                    ? `${currencyCountry.length} countries `
                    : `${currencyCountry.length} country `
                }}
              </h3>
            </template>
            <span v-for="(v, i) in currencyCountry" :key="i">
              {{ v.name }}
              <br v-if="i !== currencyCountry.length - 1" />
            </span>
          </v-tooltip>
          <p className="inline-block text-lg font-medium">&ensp;with this calling code</p>
        </div>
      </div>
    </div>
  </div>
</template>
