<template>
  <div class="d-flex justify-content-center m-3">
    <h1>Employees</h1>
  </div>
  <employee-from @addUser="addEmployee"/>
  <employee-table :employees="employees" @deleteUser="deleteUser" @userEdit="editUser" />
</template>

<script>
import employeeTable from "@/components/employeeTable";
import employeeFrom from "@/components/EmployeeForm";

export default {
  name: 'App',
  components: {
    employeeTable,
    employeeFrom,
  },
  data() {
    return {
      employees: null,

      async getData(){
        try {
          const response = await  fetch('https://jsonplaceholder.typicode.com/users')
          this.employees =await response.json()
        }
        catch (error){
          console.log(error)
        }
      },
    }
  },
  methods:{
    addEmployee(employee){
      const lastId =
          this.employees.length > 0
              ? this.employees[this.employees.length - 1].id
              : 0;
      const id = lastId + 1;
      const newEmployee = { ...employee, id };
      this.employees = [...this.employees, newEmployee];
    },

    async deleteUser(id){
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {method:"DELETE"});
        this.employees=this.employees.filter(employee => employee.id !== id);
      }
      catch (error){
        console.log(error)
      }
    },

    editUser(id , editUser){
      this.employees = this.employees.map(user => user.id === id ? editUser : user)
    },
    async updateData(id,updatedEmployee){
      try{
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`,
            {method:'PUT',
            body:JSON.stringify(updatedEmployee),
            headers:{'Content-type': 'application/json; charset=UTF-8'}
            })
        const data = await response.json()
        this.employees=this.employees.map(employee => (employee.id === id ? data : employee))
      }
      catch (error){
        console.log(error)
      }

    }
  },
  mounted() {
    this.getData()
    this.editUser()
    this.updateData()
  }

}
</script>

<style>

</style>
