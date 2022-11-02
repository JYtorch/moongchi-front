<template>
  <div id="app">
    <preloader v-if="isLoading" />
    <!-- <login-form></login-form>
    <signup-form></signup-form> -->


      
      <Header/>
      <router-view :key="$route.fullPath" :toggleLoading="toggleLoading" />
      <Footer/>


  </div>
</template>


<script>
// import LoginForm from './components/Account/LoginForm.vue'
// import SignupForm from './components/Account/SignupForm.vue'
// import LoginForm2 from './components/Account/LoginForm2.vue'
import Footer from './components/Footer.vue'
import Header from './components/Header.vue'
import Preloader from './views/Preloader.vue'

export default {
  components: { 
    // SignupForm, 
    // LoginForm, 
    Footer,
    Header,
    Preloader,

  },
  data () {
    return {
      clickLogin: true,
      isLoading: false,
    }
  },
  methods: {
    toggleLoading (bool) {      
      this.isLoading = bool      
    }
    // showModal () {
    //   this.$modal.show(
    //   LoginForm2,  
    //   { text: 'This text is passed as a property' },
    //   { draggable: true,
    //     adaptive: true,
    //     resizable: true,
    //     width: '400',
    //     height: '620',
    //     overlayTransition: 'opacity'
    //     },
    //   )
    // },
  }
} 


</script>

<style>



</style>
