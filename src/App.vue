<script setup>
import { ref } from 'vue'
import employees from './data'
import Employees from './components/Employees.vue'
import AddEmployee from './components/AddEmployee.vue'

let showAddEmployee = ref(false)

const newEmployeeAdded = newEmployee => {
  employees.push(newEmployee)
  console.log(employees, newEmployee)


  showAddEmployee.value = false
}

</script>

<template>
  <main>
    <nav>
      <button v-on:click="showAddEmployee = !showAddEmployee">{{!showAddEmployee ? 'Dodaj pracownika' : 'Szukaj pracowników'}}</button>
    </nav>
  
    <section v-if="showAddEmployee">
      <AddEmployee v-bind:employees="employees"  v-on:newEmployeeAdded="newEmployeeAdded" />
    </section>
  
    <section v-if="!showAddEmployee">
      <Employees v-bind:employees="employees" />
    </section>
  </main>
</template>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+Display:wght@400&display=swap');

  main {
    padding: calc(100vh/12) calc(100vw/12);
    font-family: 'Noto Sans Display', sans-serif;
  }
  input, textarea, button {
    padding: calc(.6vh) calc(.6vw);
  }
</style>