<template>
  <div>
    <h3 class="float-left">ข้อมูลสถานประกอบการ</h3>
    <b-navbar-nav class="float-right">
      <b-nav-form>
        <b-button size="sm" variant="success" class="mr-sm-2" @click="addNewUser">เพิ่ม</b-button>
        <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
        <b-button size="sm" class="my-2 my-sm-0" type="submit">ค้นหา</b-button>
      </b-nav-form>
    </b-navbar-nav>
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
        >
          <b-form-input
            id="ชื่อสถานประกอบการ-input"
            v-model="form.ชื่อสถานประกอบการ"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="ตำแหน่งที่สามารถสมัคร :"
          label-for="ตำแหน่งที่สามารถสมัคร"
          :valid-feedback="validFeedbackName"
        >
          <b-form-input
            id="ตำแหน่งที่สามารถสมัคร-input"
            v-model="form.ตำแหน่งที่สามารถสมัคร"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="จำนวนที่รับสมัคร :"
          label-for="จำนวนที่รับสมัคร"
          :valid-feedback="validFeedbackName"
        >
          <b-form-input
            id="จำนวนที่รับสมัคร-input"
            v-model="form.จำนวนที่รับสมัคร"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="ประวัติโดยย่อ :"
          label-for="ประวัติโดยย่อ"
          :valid-feedback="validFeedbackName"
        >
          <b-form-textarea
          id="ประวัติโดยย่อ-input"
          v-model="form.ประวัติโดยย่อ"
          rows="3"
          max-rows="6"
        ></b-form-textarea>

        <pre class="mt-3 mb-0">{{ รายละเอียด }}</pre>
        </b-form-group>
        <b-form-group
          label="เงินเดือนที่ได้ :"
          label-for="เงินเดือนที่ได้"
          :valid-feedback="validFeedbackName"
        >
          <b-form-input
            id="เงินเดือนที่ได้-input"
            v-model="form.เงินเดือนที่ได้"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="email :"
          label-for="email"
          :valid-feedback="validFeedbackName"
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
        <b-button size="sm" @click="detailUser(data.item)">รายละเอียด</b-button>
        &nbsp;
        <b-button size="sm" @click="editUser(data.item)">แก้ไข</b-button>

        &nbsp;
        <b-button size="sm" variant="danger" @click="delUser(data.item)"
          >ลบ</b-button
        >
      </template>
    </b-table>
    <div class="overflow-auto">
      <!-- <b-pagination
        v-model="currentPage"
        :total-rows="row"
        :per-page="perPage"
        aria-controls="my-table"
      ></b-pagination> -->
      <b-pagination-nav :link-gen="linkGen" :number-of-pages="10" use-router></b-pagination-nav>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      perPage: 3,
      currentPage: 1,
      ประวัติโดยย่อ: '',
      title: 'เพิ่มข้อมูลสถานประกอบการ',
      form: {
        ลำดับ: -1,
        ชื่อสถานประกอบการ: '',
        ตำแหน่งที่สามารถสมัคร: '',
        จำนวนที่รับ: '',
        ประวัติโดยย่อ: '',
        เงินเดือนที่ได้: '',
        email: ''
      },

      userList: [
        { ลำดับ: 1, ชื่อสถานประกอบการ: 'A-Host', ตำแหน่งที่สามารถสมัคร: 'Programmer', จำนวนที่รับสมัคร: '2', ประวัติโดยย่อ: '', เงินเดือนที่ได้: '25000', email: '' },
        { ลำดับ: 2, ชื่อสถานประกอบการ: 'AHEAD', ตำแหน่งที่สามารถสมัคร: 'Programmer', จำนวนที่รับสมัคร: '5', ประวัติโดยย่อ: '', เงินเดือนที่ได้: '25000', email: '' }
      ],
      lastId: 3,
      fields: ['ลำดับ', 'ชื่อสถานประกอบการ', 'operation']
    }
  },
  methods: {
    linkGen (pageNum) {
      return pageNum === 1 ? '?' : `?page=${pageNum}`
    },
    addNewUser () {
      console.log('Add new user')
      this.$bvModal.show('modal-form-user')
    },
    detailUser (user) {
      console.log('Detail ')
      console.log(user)
      this.title = 'รายละเอียด'
      this.form = { ...user }
      this.$bvModal.show('modal-form-user')
    },
    editUser (user) {
      console.log('Edit ')
      console.log(user)
      this.title = 'แก้ไขสถานประกอบการ'
      this.form = { ...user }
      this.$bvModal.show('modal-form-user')
    },
    delUser (user) {
      console.log('Delete ')
      console.log(user)
      this.$bvModal
        .msgBoxConfirm('ท่านต้องการลบ ' + user.ชื่อสถานประกอบการ + ' หรือไม่?', {
          title: 'กรุณายืนยัน',
          size: 'sm',
          buttonSize: 'sm',
          okVariant: 'danger',
          okTitle: 'ตกลง',
          cancelTitle: 'ยกเลิก',
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
        ลำดับ: -1,
        ชื่อสถานประกอบการ: '',
        ตำแหน่งที่สามารถสมัคร: '',
        จำนวนที่รับ: '',
        ประวัติโดยย่อ: '',
        เงินเดือนที่ได้: '',
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
      if (this.form.ลำดับ > 0) {
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
      user.ลำดับ = this.lastId++
      this.userList.push(user)
    },
    updateUser (user) {
      const index = this.userList.findIndex(item => item.ลำดับ === user.ลำดับ)
      this.userList.splice(index, 1, user)
    },
    deletUser (user) {
      const index = this.userList.findIndex(item => item.ลำดับ === user.ลำดับ)
      this.userList.splice(index, 1)
    },
    detailsUser (user) {
      const index = this.userList.findIndex(item => item.ลำดับ === user.ลำดับ)
      this.userList.splice(index, 1, user)
    }
  },
  computed: {
    stateName () {
      return this.form.name.length >= 2
    },
    rows () {
      return this.items.length
    }
  }
}
</script>
