<template>
  <div>
    <h3 class="float-left">ข้อมูลสถานประกอบการ</h3>
    <b-navbar-nav class="float-right">
      <b-nav-form>
        <b-form-input
          size="sm"
          class="mr-sm-2"
          placeholder="Search"
        ></b-form-input>
        <b-button size="sm" class="my-2 my-sm-0" type="submit">ค้นหา</b-button>
      </b-nav-form>
    </b-navbar-nav>
    <b-button
      size="sm"
      variant="success"
      class="float-right"
      @click="addNewUser"
      >เพิ่ม</b-button
    >
    &nbsp;
    <b-modal
      id="modal-form-user"
      ref="modal-form-user"
      :title="title"
      @hidden="resetModal"
      @ok="handleOk"
    >
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <b-form-group
          label="ชื่อสถานประกอบการ :"
          label-for="name"
          :valid-feedback="validFeedbackName"
          :state="stateName"
        >
          <b-form-input
            id="name-input"
            v-model="form.name"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="ตำแหน่งที่สามารถสมัคร :"
          label-for="position"
          :valid-feedback="validFeedbackName"
          :state="stateName"
        >
          <b-form-input
            id="position-input"
            v-model="form.position"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="ประวัติโดยย่อ :"
          label-for="history"
          :valid-feedback="validFeedbackName"
          :state="stateName"
        >
          <b-form-input
            id="history-input"
            v-model="form.history"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="เงินเดือนที่ได้ :"
          label-for="salary"
          :valid-feedback="validFeedbackName"
          :state="stateName"
        >
          <b-form-input
            id="salary-input"
            v-model="form.salary"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="email :"
          label-for="email"
          :valid-feedback="validFeedbackName"
          :state="stateName"
        >
          <b-form-input
            id="email-input"
            v-model="form.email"
            required
          ></b-form-input>
        </b-form-group>
      </form>
      <!-- <b-card class="mt-3" header="Form Data Result">
        <pre class="m-0">{{ form }}</pre>
      </b-card> -->
    </b-modal>
    <b-table striped hover :items="userList" :fields="fields">
      <template v-slot:cell(operation)="data">
        <b-button size="sm" @click="editUser(data.item)">แก้ไข</b-button>

        &nbsp;
        <b-button size="sm" variant="danger" @click="delUser(data.item)"
          >ลบ</b-button
        >
      </template>
    </b-table>
    <div class="overflow-auto">
      <b-pagination
        v-model="currentPage"
        :total-rows="row"
        :per-page="perPage"
        aria-controls="my-table"
      ></b-pagination>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      perPage: 3,
      currentPage: 1,
      title: 'เพิ่มข้อมูลสถานประกอบการ',
      form: {
        id: -1,
        name: '',
        position: '',
        history: '',
        salary: '',
        email: ''
      },

      userList: [
        { id: 1, name: 'A-Host', position: 'Programmer', history: '', salary: '25000', email: '' },
        { id: 2, name: 'AHEAD', position: 'Programmer', history: '', salary: '25000', email: '' }
      ],
      lastId: 3,
      fields: ['id', 'name', 'operation']
    }
  },
  methods: {
    addNewUser () {
      console.log('Add new user')
      this.$bvModal.show('modal-form-user')
    },
    editUser (user) {
      console.log('Edit ')
      console.log(user)
      this.title = 'แก้ไขผู้ใช้'
      this.form = { ...user }
      this.$bvModal.show('modal-form-user')
    },
    delUser (user) {
      console.log('Delete ')
      console.log(user)
      this.$bvModal
        .msgBoxConfirm('ท่านต้องการลบ ' + user.name + ' หรือไม่?', {
          title: 'กรุณายืนยัน',
          size: 'sm',
          buttonSize: 'sm',
          okVariant: 'danger',
          okTitle: 'YES',
          cancelTitle: 'NO',
          footerClass: 'p-2',
          hideHeaderClose: false,
          centered: true
        })
        .then(value => {
          if (value) {
            this.deletUser(user)
          }
        })
    },
    resetModal () {
      this.form = {
        id: -1,
        name: '',
        position: '',
        history: '',
        salary: '',
        email: ''
      }
    },
    handleOk (bvModalEvt) {
      // Prevent modal from closing
      bvModalEvt.preventDefault()
      // Trigger submit handler
      this.handleSubmit()
    },
    handleSubmit () {
      // check Validate
      if (this.stateName === false) return
      if (this.form.id > 0) {
        // Do when pass validation
        this.updateUser(this.form)
      } else {
        this.addUser(this.form)
      }
      // Hide the modal manually
      this.$nextTick(() => {
        this.$bvModal.hide('modal-form-user')
      })
    },
    addUser (user) {
      user.id = this.lastId++
      this.userList.push(user)
    },
    updateUser (user) {
      const index = this.userList.findIndex(item => item.id === user.id)
      this.userList.splice(index, 1, user)
    },
    deletUser (user) {
      const index = this.userList.findIndex(item => item.id === user.id)
      this.userList.splice(index, 1)
    }
  },
  computed: {
    stateName () {
      return this.form.name.length >= 2
    },
    invalidFeedbackName () {
      if (this.form.name.length > 2) {
        return ''
      } else if (this.form.name.length > 0) {
        return 'ชื่อต้องมีขนาดอย่างน้อย 2 ตัวอักษร'
      } else {
        return 'ต้องใส่ชื่อ'
      }
    },
    rows () {
      return this.items.length
    }
  }
}
</script>
