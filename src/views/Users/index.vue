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
        <b-form-group
          :state="nameState"
          label="Name"
          label-for="name-input"
          invalid-feedback="Name is required"
        >
          <b-form-input
            id="name-input"
            v-model="name"
            :state="nameState"
            required
          ></b-form-input>
        </b-form-group>
      </form>
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
      this.submittedNames.push(this.name)
      // Hide the modal manually
      this.$nextTick(() => {
        this.$bvModal.hide('modal-prevent-closing')
      })
    }
  }
}
</script>
