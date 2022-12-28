<template>

    <div id="employee-from" class="container col-sm-12 col-md-8">

    <form  @click.prevent class="form-control m-3 p-3">
<!--      name input-->
      <label class="form-label">Name:</label>
      <input
          required
          ref="first"
          @focus="clearstatus"
          @keypress="clearstatus"
          :class="{'has-error' :  invalidName && submitted}"
          v-model="employee.name" class="form-control" type="text" placeholder="Enter Employee Name"><br>
<!--          email input-->
      <label class="form-label">Email: </label>
      <input
          required
          @focus="clearstatus"
          :class="{'has-error' : invalidEmail && submitted}"
          v-model="employee.email" class="form-control" type="email" placeholder="Enter Employee Email" /><br>
<!--      username input-->
      <label class="form-label">Username: </label>
      <input
          required
          @focus="clearstatus"
          :class="{'has-error' : invalidUsername && submitted}"
          v-model="employee.username" class="form-control" type="text" placeholder="Enter Employee Username"/>
<!--      submit buttons-->
      <button @click="formSubmit" class="btn btn-success p-2 my-4" type="submit">
        Add To List
      </button>
<!--      Error Or Success Message-->
      <p v-if="error && submitted" class="error-message">
        Please fill out all required fields ❗
      </p>
      <p v-if="success" class="success-message">
        Employee Successfully Added ✅
      </p>
<!--      end-->
    </form>
  </div>
</template>

<script>
export default {
  name: "EmployeeForm",

  data() {
    return {
      success: false ,
      error : false ,
      submitted : false ,
      employee: {
        name: '',
        email: '',
        username:'',
      },

    }
  },
  computed:{
    invalidName(){
      return this.employee.name === ''
    },
    invalidEmail () {
      return this.employee.email === ''
    },
    invalidUsername () {
      return this.employee.username === ''
    },
  },
  methods:{
    formSubmit(){
      this.submitted=true
      this.clearstatus()
      if (this.invalidEmail || this.invalidName || this.invalidUsername){
        this.error = true
        return
      }
      this.$emit('addUser',this.employee)
      this.$refs.first.focus()
      this.employee = {
        name : '',
        email: '',
        username:'',
      }
      this.error = false
      this.success=true
      this.submitted=true
    },
    clearstatus(){
      this.success =false
      this.error=false
    },

  }
}
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}
[class*='-message'] {
  font-weight: 500;
}
.error-message {
  color: #d33c40;
}
.success-message {
  color: #32a95d;
}
</style>