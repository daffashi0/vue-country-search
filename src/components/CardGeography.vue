<script setup>
const props = defineProps(['capital', 'region', 'subregion'])
</script>
<template>
  <v-card elevation="1" class="h-full">
    <v-card-item>
      <div className="flex flex-col m-2 gap-2 justify-center">
        <h3 className="text-xl">
          Capital:
          <p className="font-medium inline">{{ props.capital[0] }}</p>
        </h3>
        <h3 className="text-xl">
          Region:
          <p className="font-medium inline">{{ props.region }}</p>
        </h3>
        <h3 className="text-xl">
          Subregion:
          <p className="font-medium inline">{{ props.subregion }}</p>
        </h3>
      </div>
    </v-card-item>
  </v-card>
</template>
