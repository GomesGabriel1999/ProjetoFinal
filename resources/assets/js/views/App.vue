<template>
    <div>
        <nav style="background-color:#15a729;" class="navbar navbar-expand-md navbar-light navbar-laravel">
            <div class="container">
                <router-link :to="{name: 'home'}" class="navbar-brand"><img style="width:85px;height:60px;" src="../../../../storage/img2.png"></router-link>
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
    
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <!-- Left Side Of Navbar -->
                    <ul class="navbar-nav mr-auto"></ul>
                    <!-- Right Side Of Navbar -->
                    <ul class="navbar-nav ml-auto" style="color:#eeeeee">
                        <router-link :to="{ name: 'login' }" class="nav-link" v-if="!isLoggedIn" style="color:#eeeeee" >Login</router-link>
                        <router-link :to="{ name: 'register' }" class="nav-link" v-if="!isLoggedIn" style="color:#eeeeee">Register</router-link>
                        <span v-if="isLoggedIn" style="color:#eeeeee">
                          <router-link :to="{ name: 'userboard' }" class="nav-link" v-if="user_type == 0"> Olá, {{name}}</router-link>
                          <router-link :to="{ name: 'admin' }" class="nav-link" v-if="user_type == 1"> Olá, {{name}}</router-link>
                        </span>
                        <li class="nav-link" v-if="isLoggedIn" @click="logout"> Logout</li>
                    </ul>
                </div>
            </div>
        </nav>
        <main class="py-4">
            <router-view @loggedIn="change"></router-view>
        </main>
    </div>
</template>
<script>
    export default {
      data(){
          return {
            name        : null,
            user_type   : 0,
            isLoggedIn  : localStorage.getItem('jwt') != null
          }
      },
      mounted() {
        this.setDefaults()
      },
      methods : {
        setDefaults(){
          if(this.isLoggedIn){
            let user        = JSON.parse(localStorage.getItem('user'))
            this.name       = user.name
            this.user_type  = user.is_admin
          }
        },
        change(){
            this.isLoggedIn = localStorage.getItem('jwt') != null
            this.setDefaults()

        },
        logout(){
            localStorage.removeItem('jwt')
            localStorage.removeItem('user')
            this.change()
            this.$router.push('/')
        }
      }
    }
</script>