<template>
<div class="container">
  <employee-form @add:emp="addEmployee" />
<br/>
  <employee-table 
  v-bind:employee="employeeData" 
  @delete:employee="deleteEmployee"
  @edit:employee="editEmployee"
  />
</div>
</template>

<script>
import employeeTable from './components/employeeTable.vue';
import employeeForm from './components/employeeForm.vue';

export default {
  name: 'App',
  components: {
    employeeTable,
    employeeForm
  },
  data(){
    return{
      employeeData:[]
    }
  },
  methods:{
    async addEmployee(e){
      const resp = await fetch('https://jsonplaceholder.typicode.com/users', 
                {
                  method:'POST',
                  body: JSON.stringify(e),
                  headers:{ 'Content-type': 'application/json; charset=UTF-8'}
                });
      const data = await resp.json();
      this.employeeData = [...this.employeeData,data]
    },
    async deleteEmployee(id){
      await fetch('https://jsonplaceholder.typicode.com/users/'+id ,
      {
        method:'DELETE',
      });
      this.employeeData = this.employeeData.filter((data)=>{
        return data.id != id;
      })
    },
    async editEmployee(id,updatedEmp){
      const resp = await fetch('https://jsonplaceholder.typicode.com/users/'+id ,
                {
                  method:'PUT',
                  body: JSON.stringify(updatedEmp),
                  headers:{ 'Content-type': 'application/json; charset=UTF-8'}
                });
      const data = await resp.json();
      this.employeeData.map((employee)=>{
        return employee.id === id ? data: employee;
      })
    },
    async getData(){
      const resp = await fetch('https://jsonplaceholder.typicode.com/users', {method:'GET'});
      const data = await resp.json();
      this.employeeData = data;
    }
  },
  mounted(){
    this.getData();
  }
}
</script>

<style>
.container{
  display: grid;
  justify-content: center;
}
</style>
