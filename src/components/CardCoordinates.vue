<script setup>
defineProps(['lat', 'long'])
</script>
<template>
  <v-card elevation="1" class="h-full">
    <v-card-item>
      <div className="flex flex-row justify-between">
        <div className="flex flex-col m-2 gap-2">
          <h3 className="text-xl font-medium">LatLong</h3>
          <!-- <Typography
              fontSize={"48px"}
              fontWeight={"bold"}
              color={"violet.main"}
            > -->
          <h1>{{ parseFloat(lat).toFixed(1) }}, {{ parseFloat(long).toFixed(1) }}</h1>
          <!-- </Typography> -->
        </div>
        <img src="/globe.svg" className="h-64 w-auto -mr-4 -mb-72" />
      </div>
    </v-card-item>
  </v-card>
</template>
