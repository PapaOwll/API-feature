<template>
  <div id="employee-table" class="w-75 container">
    <table class="table table-striped border-warning">
      <tbody>
<!--      table header start-->
      <tr>
        <td class="table-dark text-center">No.</td>
        <td class="table-primary text-center">Name</td>
        <td class="table-warning text-center">UserName</td>
        <td class="table-info text-center">Email</td>
        <td class="table-danger text-center">Action</td>
      </tr>
<!--          table header End-->
<!--  print user start-->
      <tr v-for="item in employees" :key="item.id">
<!--        user ID-->
        <td class="text-center">{{ item.id }}</td>
<!--        user NAME-->
        <td v-if="editing === item.id">
          <input class="form-control w-50" v-model="item.name" type="text">
        </td>
        <td v-else class="text-center">{{ item.name }}</td>
<!--      user USERNAME-->
        <td v-if="editing === item.id">
          <input class="form-control w-50" v-model="item.username" type="text">
        </td>
        <td v-else class="text-center">{{ item.username }}</td>
<!--    user Email-->
        <td v-if="editing === item.id">
          <input class="d-flex  form-control w-50" type="text" v-model="item.email">
        </td>
        <td v-else class="text-center ">{{ item.email }}</td>
<!--  submit and cancel edit's Buttons-->
        <td v-if="editing === item.id" class="d-flex justify-content-around">
          <button @click="userEdit(item)" class="btn btn-success  ">Save</button>
          <button class="btn btn-danger" @click="editing = null">
            Cancel
          </button>
        </td>
<!--delete and edit's buttons-->
        <td v-else class="d-flex justify-content-around">
          <button @click="$emit('deleteUser' , item.id)" class="btn btn-danger">Delete</button>
          <button @click="editMode(item.id)" class="btn btn-info">Edit</button>
        </td>
<!--end user print-->
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employeeTable",

  props: {
    employees: Array
  },
  data() {
    return {
      editing: null
    }
  },
  methods: {
    editMode(id) {
      this.editing = id
    },
    userEdit(user) {
      if (user.name === '' || user.email === '')
        return
      this.$emit('userEdit', user.id, user)
      this.editing = null
    }
  }
}
</script>

<style scoped>

</style>