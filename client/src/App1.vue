<template>
  <v-app id="app">
    <nav class="navbar navbar-expand-lg navbar-light py-1" style="background-color: #ffef90;" >
      <div class="container py-1" style=" margin-left: 0px; margin-right: 0px;" >
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <router-link class="navbar-brand mr-0" to="/">
          <img src="@/assets/logo.png" width="45" height="40">
        </router-link>
        <router-link to="/" class="d-none d-sm-none d-md-block d-lg-block mr-3" style="font-size: 23px; font-family :paybooc-Bold; font-weight : bold; color: black; text-decoration: none;">SSAFY TEMP</router-link>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent2" aria-controls="navbarSupportedContent2" aria-expanded="false" aria-label="Toggle navigation">
          <i class="fas fa-user-circle"></i>
        </button>

        <div class="collapse navbar-collapse justify-content-between" id="navbarSupportedContent">
          <ul class="navbar-nav pl-0">
            <li class="nav-item">
              <router-link class="nav-link" to="/measure">온도 측정</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/temperature">온도 확인</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/students">학생 관리</router-link>
            </li>
          </ul>
        </div>
      </div>
      
      <div class="collapse navbar-collapse" id="navbarSupportedContent2">
        <ul class="navbar-nav mx-auto d-block d-sm-block d-md-none d-lg-none pl-0">
          <div v-if="!isLoggedIn">
            <router-link to="/signin" class="dropdown-item text-center">관리자 로그인</router-link>
          </div>
          <div v-if="isLoggedIn">
            <button @click="signout" class="dropdown-item text-center">로그아웃</button>
          </div>
        </ul>
      </div>
      <div class="d-none d-sm-none d-md-block d-lg-block" style="width: 130px">
        <a href="http://edu.ssafy.com/" class="d-inline-block"><img src="./assets/edussafy.png" width="50" height="50" alt="edussafy"></a>  
        <a href="http://project.ssafy.com/" ><img src="./assets/projectssafy.png" width="50" height="50" alt="projectssafy" class="d-inline-block" style="cursor:pointer"></a>
      </div>
      <div class="dropdown d-none d-sm-none d-md-block d-lg-block">
        <button class="btn btn-light dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          <i class="fas fa-user-circle"></i>
        </button>
        <div class="dropdown-menu dropdown-menu-right" aria-labelledby="dropdownMenuButton">
          <div v-if="!isLoggedIn">
            <router-link to="/signin" class="dropdown-item text-center">관리자 로그인</router-link>
          </div>
          <div v-if="isLoggedIn">
            <button @click="signout" class="dropdown-item text-center">로그아웃</button>
          </div>      
        </div>
      </div>
    </nav>    
    <router-view
      class="mb-5"
      style="min-height: 700px"
    />
    <footer class="site-footer">
      <div class="container" style=" max-width: 1300px;">
        <div class="row">
          <div class="col-xs-6 col-sm-12 col-md-3">
            <img src="./assets/footerlogo.png" width="100" height="100" alt="footer" >
          </div>
          <div class="col-sm-12 col-md-6">
            <h6>About</h6>
            <p class="text-justify-center">사이트에 오신 여러분 환영합니다<i> Welcome!!</i> 
            </p>
          </div>

          <div class="col-xs-6 col-sm-12 col-md-3">
            <h6>Links</h6>
            <p class="my-0"><a href="http://edu.ssafy.com" target="_blank">SSAFY EDU</a></p>
            <p class="my-0"><a href="http://project.ssafy.com" target="_blank">SSAFY Project</a></p>
            <p class="my-0"><a href="http://lab.ssafy.com" target="_blank">SSAFY Git</a></p>
           
          </div>
        </div>
        <hr>
      </div>
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-md-12 col-sm-6 col-xs-12">
            <p class="copyright-text">Copyright &copy; {{year}} All Rights Reserved by 
             <a href="#">GOS</a>.
            </p>
          </div>
        </div>
      </div>
    </footer>
    

  </v-app>
</template>

<script>
import { mapState, mapActions} from 'vuex'
const sessionStore = 'sessionStore'

export default {
  name: 'App',
  methods: {
    ...mapActions(sessionStore, [
      'signin',
     
    ]),
    signout() {     
      sessionStorage.clear()    
      this.$router.push('/')
      this.$router.go(0)
    },
  },
  computed: {    
    ...mapState(sessionStore, [
      'isLoggedIn'
    ]),
    year: () => new Date().getFullYear()
    
  },
  data() {
    return {

    };
  }
}
</script>

<style>
#app {
  font-family: IBMPlexSansKR-Regular;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}


li.nav-item{
  width: 150px;
  font-size: 18px;
  font-family: paybooc-Bold;
}

.site-footer
{
  background-color:#26272b;
  padding:0px 0 20px;
  font-size:15px;
  line-height:24px;
  color:#737373;
}
.site-footer hr
{
  border-top-color:#bbb;
  opacity:0.5
}
.site-footer hr.small
{
  margin:20px 0
}
.site-footer h6
{
  color:#fff;
  font-size:16px;
  text-transform:uppercase;
  margin-top:5px;
  letter-spacing:2px
}
.site-footer a
{
  color:#737373;
}
.site-footer a:hover
{
  color:#3366cc;
  text-decoration:none;
}
.footer-links
{
  padding-left: 0;
  list-style:none
}
.footer-links li
{
  display:block
}
.footer-links a
{
  color:#737373
}
.footer-links a:active,.footer-links a:focus,.footer-links a:hover
{
  color:#3366cc;
  text-decoration:none;
}
.footer-links.inline li
{
  display:inline-block
}
.site-footer .social-icons
{
  text-align:right
}

.copyright-text
{
  margin:0
}

@media (max-width:991px)
{
  .site-footer [class^=col-]
  {
    margin-bottom:0px
  }
}
@media (max-width:767px)
{
  .site-footer
  {
    padding-bottom:0
  }
  .site-footer .copyright-text
  {
    text-align:center
  }
}



</style>
