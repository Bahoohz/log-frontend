<template>
  <div>
    <b-button
      variant="success"
      class="float-right"
      @click="addNewUser"
      v-b-modal.modal-add-new-user
      >Add New</b-button
    >
    <b-modal
      id="modal-add-new-user"
      ref="modalAddNewUser"
      title="เพิ่มผู้ใช้ใหม่"
      @show="resetModal"
      @hidden="resetModal"
      @ok="handleOk"
    >
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <b-form-group label="Name" label-for="name">
          <b-form-input
            id="name-input"
            v-model="form.name"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group label="Gender:" label-for="gender">
          <b-form-select
            id="gender"
            v-model="form.gender"
            :options="genderOptions"
            required
          ></b-form-select>
        </b-form-group>
      </form>
      <b-card class="mt-3" header="Form Data Result">
        <pre class="m-0">{{ form }}</pre>
      </b-card>
    </b-modal>
    <b-table striped hover :items="userList" :fields="fields">
      <template v-slot:cell(operation)="data">
        <b-button @click="editUser(data)">Edit</b-button>
        &nbsp;
        <b-button variant="danger" @click="delUser(data)">Delete</b-button>
      </template>
    </b-table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      form: {
        id: -1,
        name: '',
        gender: null
      },
      genderOptions: [
        { text: 'Select One', value: null },
        { value: 'M', text: 'ผู้ชาย' },
        { value: 'F', text: 'ผู้หญิง' }
      ],
      userList: [
        { id: 1, name: 'Phatcharapol', gender: 'M' },
        { id: 2, name: 'Prasob', gender: 'M' }
      ],
      lastId: 3,
      fields: ['id', 'name', 'gender', 'operation']
    }
  },
  methods: {
    addNewUser () {
      console.log('Add new user')
    },
    editUser (user) {
      console.log('Edit ')
      console.log(user)
    },
    delUser (user) {
      console.log('Delete ')
      console.log(user)
    },
    resetModal () {},
    handleOk (bvModalEvt) {
      // Prevent modal from closing
      bvModalEvt.preventDefault()
      // Trigger submit handler
      this.handleSubmit()
    },
    handleSubmit () {
      // check Validate
      // Do when pass validation
      this.addUser(this.form)
      // Hide the modal manually
      this.$nextTick(() => {
        this.$bvModal.hide('modal-add-new-user')
      })
    },
    addUser (user) {
      user.id = this.lastId++
      this.userList.push(user)
    }
  }
}
</script>
