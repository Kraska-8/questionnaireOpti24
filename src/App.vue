<template>
  <div id="app">
     <Navigation/>
     <Alert v-if="seen" @close-alert="closeModal" @openModal="showModal = true"/>
     <Modal :show="showModal" @close="closeModal" />
  </div>
</template>

<script>

import Navigation from './components/Nav';
import Alert from './components/Alert';
import Modal from './components/UI/Modal';

export default {
  name: 'home',
  data () {
  return {
    seen: true,
    showModal: false,
  }
},
  methods: {
    closeModal(){
      if(this.showModal){
        this.showModal=false
      }
      this.seen=false;
      localStorage.setItem( 'ClosedAlert', true );
    }
  },

// Saving state of targeting alert on local storage 
  created(){
    let alerted = localStorage.getItem('ClosedAlert') ;
    if (alerted) {
      this.seen =  false
    }
  },

  components: {
    Navigation,
    Alert,
    Modal
  }
}
</script>

<style lang="scss">

body {
  margin: 0;
  padding: 0;
  font-family: 'PT Sans', sans-serif;
  font-size: 14px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: normal;
  color: #fff;
}


</style>
