<script setup>
import { ref, defineProps, toRefs, watchEffect } from 'vue'
import Employee from './Employee.vue'

const props = defineProps({
  employees: Array
})

const { employees } = toRefs(props)

let departments = ref([])
let employeesGrupedByDepartment = ref([])
let salariesByDepartment = ref([])
let salariesTotal = ref(0)
let salariesAll = ref([])
let salariesMin = ref(0)
let salariesMax = ref(0)

let searchedEmployee = ref('')
let searchedDepartment = ref(departments.value)
let searchedSalaryMin = ref(0)
let searchedSalaryMax = ref(0)

let employeesFiltered = ref(employees.value)


// split array of employees into employees gruped by department
employeesGrupedByDepartment.value = employees.value.reduce((a, b) => {
  if (!a[b['dzial']]) { a[b['dzial']] = [] }
       a[b['dzial']].push(parseFloat(b.wynagrodzenieKwota))
  return a
}, {})

Object.keys(employeesGrupedByDepartment.value).map(department => {
  let salaryByDepartment = employeesGrupedByDepartment.value[department].reduce((a, b) => a + b)
  salariesByDepartment.value.push({ dzial: department, wynagrodzenieKwota: salaryByDepartment })
  salariesTotal.value += salaryByDepartment
  departments.value.push(department)
  salariesAll.value.push(...employeesGrupedByDepartment.value[department])
  salariesMin.value = Math.min(...salariesAll.value)
  salariesMax.value = Math.max(...salariesAll.value)
  searchedSalaryMin.value = salariesMin.value
  searchedSalaryMax.value = salariesMax.value
})

watchEffect(() => {
  employeesFiltered.value = employees.value
  if (searchedEmployee.value) {
    employeesFiltered.value = employeesFiltered.value.filter(employee =>
      // (employee.nazwisko.toString().toLowerCase().includes(searchedEmployee.value.toLowerCase())) || (employee.imie.toString().toLowerCase().includes(searchedEmployee.value.toLowerCase()))
     (employee.nazwisko + ' ' + employee.imie).toLowerCase().includes(searchedEmployee.value.toLowerCase())
    )
  }
  if (searchedDepartment.value) {
    employeesFiltered.value = employeesFiltered.value.filter(employee =>
      (searchedDepartment.value.includes(employee.dzial))
    )
  }
  if (searchedSalaryMin.value || searchedSalaryMax.value) {
    employeesFiltered.value = employeesFiltered.value.filter(employee =>
      (((employee.wynagrodzenieKwota * 1) >= searchedSalaryMin.value) && ((employee.wynagrodzenieKwota * 1) <= searchedSalaryMax.value))
    )
  }
})

</script>

<template>
  <section class="searchEmployee">
    <form>
      <div></div>
      <div>
        <label for="">
          <input type="text" v-model="searchedEmployee" placeholder="Szukaj...">
        </label>
      </div>
      <div>
        <div v-for="department, i in departments" v-bind:key="i">
          <label>
            <input type="checkbox" v-bind:value="department" v-model="searchedDepartment"> {{department}}
          </label>
          <br>
        </div>
      </div>
      <div>
        <label for="">
          <input type="number" step="10" v-bind:min="salariesMin" v-bind:max="salariesMax" v-model.number="searchedSalaryMin">
          - 
          <input type="number" step="10" v-bind:min="salariesMin" v-bind:max="salariesMax" v-model.number="searchedSalaryMax">
        </label>
      </div>
    </form>
  
    <br>
  
    <div class="table-header">
      <h3>Imię:</h3>
      <h3>Nazwisko:</h3>
      <h3>Dział:</h3>
      <h3>Wynagrodzenie:</h3>
    </div>
    <div v-for="employee in employeesFiltered">
      <Employee v-bind:employee="employee" />
    </div>
    <br>
  
    <h3>Podsumowanie:</h3>
    <div v-for="salaries in salariesByDepartment">
      <span>{{salaries.dzial}}: {{salaries.wynagrodzenieKwota.toFixed(2)}} PLN</span>
    </div>
    <div>
      <span>Całkowita suma wynagrodzeń: {{salariesTotal.toFixed(2)}} PLN</span>
    </div>
  
    <br>
    
    <hr>
    <div>{{searchedEmployee || '&nbsp;'}}</div>
    <div>{{searchedDepartment}}</div>
    <div>{{searchedSalaryMin}}-{{searchedSalaryMax}}</div>
    <pre>{{employeesFiltered}}</pre>
  </section>
</template>

<style scoped>
  form, .table-header {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    align-items: end;
  }
</style>