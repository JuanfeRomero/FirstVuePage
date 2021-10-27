<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <employee-form  @add:employee="addEmployee"/>
    <employee-table :employees="employees" @edit:employee="editEmployee" @delete:employee="deleteEmployee" @deleteAll:employee="deleteAll"/>
  </div>
</template>

<script>
import EmployeeForm from './components/EmployeeForm.vue'
import EmployeeTable from './components/EmployeeTable.vue'

export default {
  name: 'App',
  components: {
    EmployeeTable,
    EmployeeForm,
  },
  data() {
    return {
      employees: [],
    }
  },
  mounted() {
    this.getEmployees();
  },
  methods: {
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        const data = await response.json();
        this.employees = data;
      } catch (error) {
        console.error(error);
      }
    },
    async addEmployee(employee){
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: { 'Content-type': 'application/json; charset=UTF-8'},
        });
        const data = await response.json();
        this.employees = [...this.employees, data];
      } catch (error) {
        console.error(error);
      }
      // const lastId = this.employees.length > 0 ? this.employees[this.employees.length-1].id : 0;
      // const id = lastId +1;
      // const newEmployee = {...employee, id};
      // this.employees = [...this.employees, newEmployee];
    },
    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: {'Content-type': 'application/json; charset=UTF-8'},
        });
        const data = await response.json();
        this.employees  = this.employees.map(employee => (employee.id === id ? data : employee));
      } catch (error) {
        console.error(error);
      }
      // this.employees = this.employees.map( employee=>
      //   employee.id === id ? updatedEmployee : employee 
      //);
    },
    async deleteEmployee(id){
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'DELETE'
        });
        this.employees = this.employees.filter(employee => employee.id !== id);
      } catch (error) {
        console.error(error);
      }
      // this.employees = this.employees.filter(
      //   employee => employee.id !== id
      // );
    },
    async deleteAll(){
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/`, {
          method: 'DELETE'
        });
      } catch (error) {
        console.error(error);
      }
      this.employees = [];
    },
  }
}
</script>

<style>
body{
  background-color: #272727;
  color: rgb(204, 204, 204);
}
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container{
    max-width: 680px;
  }
    form {
        margin-bottom: 2rem;
    }
    input {
        color: rgb(204, 204, 204);
    }
</style>
