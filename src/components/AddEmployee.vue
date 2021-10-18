<script setup>
import { ref, defineEmits } from 'vue'

const emit = defineEmits(['newEmployeeAdded'])

let error = ref('')
let name = ref('')
let surname = ref('')
let department = ref('')
let salary = ref('')
let currency = ref('PLN')

let addNewEmployee = () => {
  try {
    if (name.value === '') throw new Error('Nie podano imienia')
    if (surname.value === '') throw new Error('Nie podano nazwiska')
    if (department.value === '') throw new Error('Nie podano działu')
    if (salary.value === '') throw new Error('Nie podano wynagrodzenia')
  
    let newEmployee = {
      imie: name.value,
      nazwisko: surname.value,
      dzial: department.value,
      wynagrodzenieKwota: salary.value,
      wynagrodzenieWaluta: currency.value
    }

    emit('newEmployeeAdded', newEmployee)
  } catch (err) {
    error.value = err.message
  }
}

</script>

<template>
  <section class="addEmployee">
    <br>
    <form v-on:submit.prevent="addNewEmployee">
      <label>
        <input type="text" placeholder="Imię..." v-model.trim="name">
      </label>
      <label>
        <input type="text" placeholder="Nazwisko..." v-model.trim="surname">
      </label>
      <label>
        <input type="text" placeholder="Dział..." v-model.trim="department">
      </label>
      <label>
        <input type="number" placeholder="Wynagrodzenie..." v-model.trim="salary">
      </label>
      <label>
        <input type="text" placeholder="Waluta..." v-model.trim="currency">
      </label>
      <br><br>
      <input type="submit" value="Dodaj pracownika">
    </form>
    <br>
    <div>{{error}}</div>
    <hr>
    <div>{{name}}</div>
    <div>{{surname}}</div>
    <div>{{department}}</div>
    <div>{{salary}}</div>
  </section>
</template>

<style scoped>
  label {
    display: block;
    margin: calc(1vh) 0;
  }
</style>