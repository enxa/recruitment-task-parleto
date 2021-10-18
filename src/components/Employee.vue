<script setup>
import { defineProps, toRefs } from 'vue'

const props = defineProps({
  employee: Object
})

const { employee } = toRefs(props)

</script>

<template >
  <section>
    <div>{{employee.imie}}</div>
    <div>{{employee.nazwisko}}</div>
    <div>{{employee.dzial}}</div>
    <div>{{employee.wynagrodzenieKwota}} {{employee.wynagrodzenieWaluta}}</div>
  </section>
</template>

<style scoped>
  section {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
  }
</style>