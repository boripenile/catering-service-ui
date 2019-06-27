<template>
    <transition name="el-zoom-in-top">
        <div class="content-wrapper">
            <!-- Content Header (Page header) -->
            <section class="content-header">
                <h1>Profile Page<small>It's all start from here <i class="ti-heart"></i><i class="ti-export"></i><i class="ti-printer"></i></small></h1>
                <ol class="breadcrumb">
                    <li>
                        <router-link to="/"> <i class="ti-home"></i></router-link>
                    </li>
                    <li><a href="#">{{ getApplication.app_name }}</a></li>
                    <li class="active">Profile</li>
                </ol>
            </section>
            <!-- Main content -->
            <section class="content">
                <div class="box">
                    <div class="box-body" style="min-height:400px;">
             
                        <div class="col-lg-3">
                                <a href="#"><img v-bind:src="getUser.image_url"
                                width="190" height="200"
                                alt="cover" /></a>
                            <h2>{{ getUser.first_name }} {{ getUser.last_name }}
                                        <span class="pull-right social-profile">
                                            <i class="entypo-facebook-circled"></i>  <i class="entypo-twitter-circled"></i>  <i class="entypo-linkedin-circled"></i>  <i class="entypo-github-circled"></i>  <i class="entypo-gplus-circled"></i>
                                        </span>
                                    </h2>
                        </div>
                        <!-- end movie-card -->
                        <div class="col-lg-9 profile-name">
                            <dl class="dl-horizontal-profile"> <dt>Name</dt>
                                    <dd>{{ getUser.first_name }} {{ getUser.last_name }}</dd> <dt>Email</dt>
                                    <dd>{{ getUser.email_address }}</dd> <dt>Phone</dt>
                                    <dd>{{ getUser.phone_number }}</dd> <dt>Active Periode</dt>
                                    <dd>02 Dec 2014</dd> <dt>Last Update</dt>
                                    <dd>02 Apr 2014</dd>
                                </dl>
                          <!-- <permission-detail :permissions="permissions" :show="show"></permission-detail> -->
                        <div>
                          <h4 class="header">Basic Permisisons</h4>
                          <el-tag
                            :key="permission"
                            v-for="permission in basicBermissions" :closable="false" :disable-transitions="false" @close="removePermission(permission)">
                            {{permission.description}}
                          </el-tag>
                        </div>
                        <div>
                          <h4 class="header">User Permisisons</h4>
                          <el-tag
                            :key="permission"
                            v-for="permission in selfPermissions" :closable="show" :disable-transitions="false" @close="removePermission(permission)">
                            {{permission.description}}
                          </el-tag>
                          <el-popover
                            placement="right"
                            width="400"
                            trigger="click">
                            <el-table ref="multipleRows" :data="basicBermissions" :row-key="name" stripe 
                              @selection-change="handleSelectionChange">
                              <el-table-column type="selection"></el-table-column>
                              <el-table-column prop="name" label="Name" 
                                :show-overflow-tooltip="true"></el-table-column>
                              <el-table-column prop="description" label="Description" 
                                :show-overflow-tooltip="true"></el-table-column>                           
                            </el-table>
                              <el-button slot="reference">Add permission</el-button>
                              <el-button @click="addPermissionsToUser">Set Permissions</el-button>
                          </el-popover>
                          
                        </div>
                        </div>
                    </div>
                </div>
            </section>
            <!-- /.content -->
        </div>
    </transition>
</template>
<script>
  import { mapGetters } from 'vuex'
  export default {
    data: function () {
      return {
        basicBermissions: [],
        selfPermissions: [],
        show: false,
        permissions: [],
        multipleSelection: [],
        userPermissions: [],
        selection: {}
      }
    },
    name: 'Profile',
    computed: {
      ...mapGetters(['getUser', 'getApplication', 'getOrganisation', 'getRoles', 'getPermissions', 'getToken'])
    },
    mounted () {
      this.checkRole(),
      this.basicBermissions = this.getPermissions.basic,
      this.selfSermissions = this.getPermissions.self
    },
    methods: {
      removePermission (permission) {
        return;
      },
      checkRole: function () {
        if (this.getRoles.find(x => (x.name === 'superadmin' || x.name === 'admin'))) {
          this.show = true
        } else {
          this.show = false
        }
      },
      loadPermissions: function () {
        this.$http.userapi.post('/roles', {

        }, {
          headers: {
            'token': this.getToken,
            'action': '' 
          }
        }).then(response => {

        }).catch(error => {

        })
      },
      tableRowClassName({row, rowIndex}) {
        if (rowIndex % 2 === 0) {
          return 'success-row';
        }
        return '';
      },
      handleSelectionChange: function (val) {
        this.multipleSelection = val
      },
      addPermissionsToUser: function () {
        this.userPermissions = []
        for (var i = 0; i < this.multipleSelection.length; i++ ) {
          console.log(this.multipleSelection[i].name)
          this.userPermissions.push(this.multipleSelection[i].name)
        }
        console.log('Total selection ' + this.userPermissions.length)
      }
    }
  }
</script>
<style>
  .el-tag + .el-tag {
    margin-left: 10px;
    margin-bottom: 10px;
  }
</style>
