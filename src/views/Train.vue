<template>
  <div>
    <h3 class="float-left">รายการอบรม</h3>
    <b-modal
      id="modal-form-user"
      ref="modal-form-user"
      :title="title"
      @hidden="resetModal"
      @ok="handleOk"
    >
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <b-form-group
          label="ชื่องาน :"
          label-for="ชื่องาน"
          :valid-feedback="validFeedbackName"
        >
          <b-form-input
            id="ชื่องาน-input"
            v-model="form.ชื่องาน"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="วันที่ :"
          label-for="วันที่"
          :valid-feedback="validFeedbackName"
        >
          <b-form-input
            id="วันที่-input"
            v-model="form.วันที่"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="เวลาเริ่ม :"
          label-for="เวลาเริ่ม"
          :valid-feedback="validFeedbackName"
        >
          <b-form-input
            id="เวลาเริ่ม-input"
            v-model="form.เวลาเริ่ม"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="เวลาสิ้นสุด :"
          label-for="เวลาสิ้นสุด"
          :valid-feedback="validFeedbackName"
        >
          <b-form-input
            id="เวลาสิ้นสุด-input"
            v-model="form.เวลาสิ้นสุด"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="ประเทภ :"
          label-for="ประเทภ"
          :valid-feedback="validFeedbackName"
        >
          <b-form-input
            id="ประเภท-input"
            v-model="form.ประเภท"
            required
          ></b-form-input>
          <b-form-group
          label="บริษัท :"
          label-for="บริษัท"
          :valid-feedback="validFeedbackName"
        >
          <b-form-input
            id="บริษัท-input"
            v-model="form.บริษัท"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="รายละเอียด :"
          label-for="รายละเอียด"
          :valid-feedback="validFeedbackName"
        >
        <b-form-textarea
          id="รายละเอียด-input"
          v-model="form.รายละเอียด"
          rows="3"
          max-rows="6"
        ></b-form-textarea>

        <pre class="mt-3 mb-0">{{ รายละเอียด }}</pre>
        </b-form-group>
        </b-form-group>
      </form>
      <!-- <b-card class="mt-3" header="Form Data Result">
        <pre class="m-0">{{ form }}</pre>
      </b-card> -->
    </b-modal>
    <b-table striped hover :items="userList" :fields="fields">
      <template v-slot:cell(operation)="data">
        <b-button size="sm" @click="detailUser(data.item)">ดู</b-button>
        &nbsp;
        <b-button size="sm"  @click="registerUser(data.item)">ลงทะเบียน</b-button
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
      title: '',
      รายละเอียด: '',
      form: {
        ชื่องาน: '',
        วันที่: '',
        เวลาเริ่ม: '',
        เวลาสิ้นสุด: '',
        ประเภท: '',
        บริษัท: '',
        รายละเอียด: ''
      },

      userList: [
        { ชื่องาน: 'CDG', วันที่: '01-01-2020', เวลาเริ่ม: '13:00', เวลาสิ้นสุด: '16:00', ประเภท: 'เตรียมความพร้อม', บริษัท: 'CDG', รายละเอียด: 'จำกัด 200 คน ห้ามเล่นมือถือหรือหลับ' },
        { ชื่องาน: 'CDG', วันที่: '02-01-2020', เวลาเริ่ม: '13:00', เวลาสิ้นสุด: '16:00', ประเภท: 'วิชาการ', บริษัท: 'CDG', รายละเอียด: 'จำกัด 200 คน ห้ามเล่นมือถือหรือหลับ' },
        { ชื่องาน: '', วันที่: '', เวลาเริ่ม: '', เวลาสิ้นสุด: '', ประเภท: '', บริษัท: '', รายละเอียด: 'จำกัด 200 คน ห้ามเล่นมือถือหรือหลับ' },
        { ชื่องาน: '', วันที่: '', เวลาเริ่ม: '', เวลาสิ้นสุด: '', ประเภท: '', บริษัท: '', รายละเอียด: 'จำกัด 200 คน ห้ามเล่นมือถือหรือหลับ' }
      ],
      lastId: 7,
      fields: ['ชื่องาน', 'วันที่', 'เวลาเริ่ม', 'เวลาสิ้นสุด', 'ประเภท', 'บริษัท', 'operation']
    }
  },
  methods: {
    linkGen (pageNum) {
      return pageNum === 1 ? '?' : `?page=${pageNum}`
    },
    detailUser (user) {
      console.log('Detail ')
      console.log(user)
      this.title = 'รายละเอียด'
      this.form = { ...user }
      this.$bvModal.show('modal-form-user')
    },
    registerUser (user) {
      console.log('Register ')
      console.log(user)
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
    regisUser (user) {
      const index = this.userList.findIndex(item => item.id === user.id)
      this.userList.splice(index, 1)
    },
    detailsUser (user) {
      const index = this.userList.findIndex(item => item.id === user.id)
      this.userList.splice(index, 1, user)
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
