<template>
    <div>
      <div class="row">
        <div class="col-md-8">
          <el-input
            placeholder="Type user's email address or username"
            v-model="searchEmail">
          </el-input>
        </div>
        <div class="col-md-4">
          <el-button type="default" icon="el-icon-search" @click="searchUser(searchEmail, callback)">Look Up</el-button>
        </div>
      </div>
      <hr v-if="hasDetails" />
      <div class="row" v-if="hasDetails">
        <div class="col-md-6">
          <p>Name: {{ user.first_name }} {{ user.last_name }} {{ user.other_name }} </p>
          <p>Email address: {{ user.email_address }}</p>
        </div>
        <div class="col-md-6">
          <p>Select a Role: </p>
          <el-select v-model="roleName" placeholder="Select">
            <el-option
              v-for="role in roles"
              :key="role.role_name"
              :label="role.description"
              :value="role.role_name">
              <span style="float: left">{{ role.role_name }}</span>
              <span style="float: right; color: #8492a6; font-size: 13px">{{ role.description }}</span>
            </el-option>
          </el-select>
          <el-button type="success" icon="el-icon-s-promotion" @click="sendInvite(user, roleName)">Send Invite</el-button>
        </div>
      </div> 
      
    </div>
    
</template>

<script>
export default {
  props: ['user', 'roles', 'searchEmail', 'hasDetails', 'callback', 'roleName'],
  name: 'InviteUser',
  data: function () {
    return {
      hasDetails: false
    }
  },
  methods: {
    sendInvite: function (user, roleName) {
      this.$emit('send-invite', user, roleName)
    },
    searchUser: function (param, callback) {
      if (param) {
        this.$emit('search-user', param, callback)
      }
    }
  } 
}
</script>
