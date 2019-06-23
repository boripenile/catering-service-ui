<template>
  <transition name="el-zoom-in-top">
      <el-form :rules="rules" :model="registration" ref="registrationForm" status-icon>
        <div class="vld-parent">
          <loading :active.sync="isLoading" 
          :is-full-page="true"></loading></div>
          <div class="box">
            <div class="box-body">
              <el-link @click="loginPage()" type="info" icon="el-icon-d-arrow-left">Back to Login</el-link> 
              <h3><b>Start Registration</b></h3><br/>
              <el-alert v-if="success"
                v-bind:title="title"
                v-bind:type="type"
                v-bind:description="message"
                show-icon>
              </el-alert>
              <h4><b>Basic Information</b></h4>
              <p>Please complete all fields expect optional ones. Ensure that you use a valid email address and phone number</p>
              <div class="row">
                <div class="col-lg-3">
                  <p>First Name</p>
                  <el-form-item prop="firstName">
                    <el-input v-model="registration.firstName" placeholder="First name"></el-input>
                  </el-form-item>
                </div>
                <div class="col-lg-3">
                  <p>Last Name</p>
                  <el-form-item prop="lastName">
                    <el-input v-model="registration.lastName" placeholder="Last name"></el-input>
                  </el-form-item>
                </div>
                <div class="col-lg-3">
                  <p>Email Address</p>
                  <el-form-item prop="emailAddress">
                    <el-input v-model="registration.emailAddress" placeholder="Enter a valid Email Address"></el-input>
                  </el-form-item>
                </div>
                <div class="col-lg-3">
                  <p>Phone Number</p>
                  <el-form-item prop="phoneNumber">
                    <el-input v-model.number="registration.phoneNumber" placeholder="Enter a valid phone number" type="number"></el-input>
                  </el-form-item>
                </div>
              </div>
              <br/>
              <h4><b>Login Information</b></h4>
              <p>Please complete all fields expect optional ones</p>
              <div class="row">
                <div class="col-lg-4">
                  <p>Username</p>
                  <el-form-item prop="username">
                    <el-input v-model="registration.username" placeholder="Choose a username"></el-input>
                  </el-form-item>
                </div>
                <div class="col-lg-4">
                  <p>Password</p>
                  <el-form-item prop="password">
                    <el-input v-model="registration.password" placeholder="Enter password" type="password"></el-input>
                  </el-form-item>
                </div>
                <div class="col-lg-4">
                  <p>Confirm Password</p>
                  <el-form-item prop="confirmPassword">
                    <el-input v-model="registration.confirmPassword" placeholder="Confirm password" type="password"></el-input>
                  </el-form-item>
                </div>
              </div><br/>
              <div class="pull-right">

                <el-button type="primary" round class="btn btn btn-primary left-margin" 
          @click.prevent="validateRegistration('registrationForm')">Proceed with Registration  <i class="el-icon-d-arrow-right"></i></el-button>
                <el-button round class="btn btn" 
          @click.prevent="resetRegistrationForm('registrationForm')">Reset</i></el-button>
              </div>              
            </div>
          </div>
    </el-form>
  </transition>
</template>
<script>
import Loading from 'vue-loading-overlay'
import 'vue-loading-overlay/dist/vue-loading.css'
import { mapMutations, mapGetters } from 'vuex'
import sweetalert from 'sweetalert'

export default {
  data: function () {
    var validateEmailAddress = (rule, value, callback) => {
      if (value === '') {
        return callback(new Error('Provide a valid email address'))
      } else if (value !== '') {
        if (this.regex.test(value) === false) {
          callback(new Error('Invalid email address. Please check it'))
        } else {
          callback()
        }
      }
    }
    var validatePhoneNumber = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('Provide a valid phone number'))
      } else if (!Number.isInteger(value)) {
        callback(new Error('Invalid phone number. Please check it'))
      } else {
        var numberStr = String(value)
        if (numberStr.length === 13) {
          callback()
        } else {
          callback(new Error('Phone number must be 13 characters. Example: 2348078675634'))
        }
      }
    }
    var validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('Please choose a password'))
      } else {
        if (value.length < 8) {
          callback(new Error('Password must be at least 8 characters'))
        }
        if (this.registration.confirmPassword !== '') {
          this.$refs.registrationForm.validateField('confirmPassword')
        }
        callback()
      }
    }
    var validateConfirmPass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('Please confirm your password'))
      } else if (value !== this.registration.password) {
        callback(new Error('Confirm password does not match with the password! Please re-enter it.'))
      } else {
        callback()
      }
    }
    return {
      regex: /^(([^<>()\]\\.,;:\s@"]+(\.[^<>()\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/,
      isLoading: false,
      success: false,
      title: '',
      type: '',
      message: 'Error message',
      registration: {
        firstName: '',
        lastName: '',
        emailAddress: '',
        phoneNumber: '',
        username: '',
        password: '',
        confirmPassword: ''
      },
      rules: {
        firstName: [
          { required: true, message: 'Provide your administrator\'s first name', trigger: 'blur' },
          { min: 3, max: 20, message: 'First name should be 3 to 20 characters.', trigger: 'blur' }
        ],
        lastName: [
          { required: true, message: 'Provide your administrator\'s last name', trigger: 'blur' },
          { min: 3, max: 20, message: 'Last name should be 3 to 20 characters.', trigger: 'blur' }
        ],
        emailAddress: [
          { validator: validateEmailAddress, trigger: 'blur' }
        ],
        phoneNumber: [
          { validator: validatePhoneNumber, trigger: 'blur' }
        ],
        username: [
          { required: true, message: 'Choose a username', trigger: 'blur' },
          { min: 7, max: 50, message: 'Username should be 7 to 50 characters.', trigger: 'blur' }
        ],
        password: [
          { validator: validatePass, trigger: 'blur' }
        ],
        confirmPassword: [
          { validator: validateConfirmPass, trigger: 'blur' }
        ]
      }
    }
  },
  components: {
    Loading
  },
  mounted () {
  },
  methods: {
    validateRegistration: function (registrationForm) {
      this.$refs[registrationForm].validate((valid) => {
        if (valid) {
          this.registerUser()
        } else {
          return false
        }
      })
    },
    resetRegistrationForm: function (registrationForm) {
      this.$refs[registrationForm].resetFields()
    },
    loginPage: function () {
      this.$router.push('/app/login')
    },
    registerUser: function () {
      this.isLoading = true
      console.log('Saving user...')
      this.$http.userapi.post('/users', {
        'firstName': this.registration.firstName,
        'lastName': this.registration.lastName,
        'emailAddress': this.registration.emailAddress,
        'phoneNumber': this.registration.phoneNumber,
        'username': this.registration.username,
        'password': this.registration.password,
        'appCode': this.$store.state.appCode
      }, {
        headers: {
          'action': 'registerUser'
        }
      }).then(response => {
        this.isLoading = false
        if (response.data.code === 400) {
          this.success = true
          this.type = 'warning'
          this.title = 'Registration cannot be completed'
          this.message = response.data.message
        } else {
          if (response.data.code === 200) {
            this.success = true
            this.type = 'success'
            this.title = 'Registration completed successfully'
            this.message = response.data.message
            this.resetRegistrationForm('registrationForm')
          }
        }
      }).catch(error => {
        this.isLoading = false
        console.log(error)
      })
    }
  }
}
</script>

<style>
.left-margin {
  margin-right: 4em;
}
</style>
