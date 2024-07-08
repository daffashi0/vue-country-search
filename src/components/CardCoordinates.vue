<script setup>
import { onMounted } from 'vue'

const props = defineProps(['latlng'])
</script>
<template>
  <v-card elevation="1" class="h-full">
    <v-card-item class="h-full">
      <div className="flex flex-row justify-between h-full">
        <div className="flex flex-col m-2 gap-2 ">
          <h3 className="text-4xl font-medium text-purple-500">LatLong</h3>
          <!-- <Typography
              fontSize={"48px"}
              fontWeight={"bold"}
              color={"violet.main"}
            > -->
          <h1 class="text-2xl">
            {{ parseFloat(props.latlng[0]).toFixed(1) }},
            {{ parseFloat(props.latlng[1]).toFixed(1) }}
          </h1>
          <!-- </Typography> -->
        </div>
        <img src="@/assets/globe.svg" className="h-64 w-auto -mr-4 -mb-72" />
      </div>
    </v-card-item>
  </v-card>
</template>
