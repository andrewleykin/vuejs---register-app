<template>
  <div id="#app">
    <div class="container-fluid navbar-light bg-dark">
      <div class="row">
        <div class="col">
          <nav class="navbar">
            <form class="form-inline justify-content-end">
              <div v-if="!signComplete">
                <button @click="switchSign('sign-in')" class="btn btn-outline-success mr-3" type="button">Войти</button>
                <button @click="switchSign('sign-up')" class="btn btn-outline-success" type="button">Регистрация</button>
              </div>
              <span v-else>{{ email }}</span>
            </form>
          </nav>
        </div>
      </div>
    </div>
    <div class="container" v-if="!isMainPage">
      <div class="row">
        <div class="col">
          <sign-in @addUser ="addUser($event)" v-if="sign == 'sign-in'"></sign-in>
          <sign-up @reqSuccess="sign = $event" v-else></sign-up>
        </div>
      </div>
    </div>
    <div class="container" v-else>
      <div class="row">
        <div class="col">
          <main-page :uid="uid"></main-page>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SignIn from './components/SignIn.vue'
import SignUp from './components/SignUp.vue'
import MainPage from './components/MainPage.vue'

export default {
  name: 'app',
  data () {
    return {
      sign: 'sign-in',
      isMainPage: false,
      signComplete: false,
      email: '',
      uid: ''
    }
  },
  components: {
    SignIn,
    SignUp,
    MainPage
  },
  methods: {
    addUser(sett){
      this.isMainPage = sett.mainPage;
      this.signComplete = sett.complete;
      this.email = sett.email;
      this.uid = sett.uid;
    },
    switchSign(currentSign) {
      this.sign = currentSign;
    }
  }
}
</script>

<style scoped>
.navbar form {
  width: 100%;
}
.navbar form span {
  color: white;
}
</style>
