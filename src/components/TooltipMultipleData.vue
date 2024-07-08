<script setup>
defineProps(['capital', 'region', 'subregion'])
</script>
<template>
  <div>
    <span v-for="(v, i) in countries" :key="i">
      {{ v.name }}
      <br v-if="i !== countries.length - 1" />
    </span>
  </div>
</template>
