<template>
  <div>
    <b-form @submit="checkForm">
      <b-form-group id="input-group-1" label="Username:" label-for="input-1">
        <b-form-input
          id="input-1"
          placeholder="Enter username"
          required
          v-model="form.username"
        >
        </b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="Password:" label-for="input-2">
        <b-form-input
          id="input-2"
          type="password"
          placeholder="Enter password"
          required
          v-model="form.password"
        >
        </b-form-input>
      </b-form-group>
      <b-button type="submit" variant="primary">Submit</b-button>
      <b-button type="reset" variant="danger">Submit</b-button>
    </b-form>
    <div>
      {{ errors }}
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      errors: [],
      form: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    checkForm: function (e) {
      this.errors = []
      if (this.form.username.length < 3) {
        this.errors.push('Username ต้องมีความยาวมากกว่า 3 ตัวอักษร')
      }

      if (this.form.password.length < 3) {
        this.errors.push('Password ต้องมีความยาวมากกว่า 3 ตัวอักษร')
      }

      if (this.form.username.localeCompare('admin')) {
        this.errors.push('Username ไม่ถูกต้อง')
      }

      if (this.form.password.localeCompare('password')) {
        this.errors.push('Password ไม่ถูกต้อง')
      }
      if (!this.errors.length) {
        return true
      }
      e.preventDefault()
    }
  },
  computed: {
    getData: function () {
      return this.form
    }
  }
}
</script>
