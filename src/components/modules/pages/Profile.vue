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
    name: 'Profile',
    computed: {
      ...mapGetters(['getUser', 'getApplication', 'getOrganisation'])
    }
  }
</script>