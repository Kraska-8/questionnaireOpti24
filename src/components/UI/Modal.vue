<template>
    <transition name="fade">
        <div class="modal-mask" v-show="show">
            <div class="modal" >
                <div class="modalContent">
                    <Steps @closeModal="close"/>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>

import Steps from '../Steps';

export default {
    name: 'modal',
    props: ['show'],
    data(){
        return{
        }
    },
    methods: {
        close: function () {
            this.$emit('close');
        }
    },
    components: {
        Steps
    }
}
</script>


<style lang="scss">

.modal {
    z-index: 103;
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    overflow: auto;
}

.modalContent {
    position: absolute;
    top: 30%;
    left: 50%;
    background-color: #f5f5f5;
    padding: 44px 47px 40px 47px;
    transform: translateX(-50%);
    margin-bottom: 40px;
}

.modal-mask {
    width: 100%;
    height: 100%;
    position: fixed;
    z-index: 100;
    left: 0;
    top: 0;
    background-color: rgba(0, 0, 0, 0.5);
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>