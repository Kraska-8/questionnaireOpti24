<template>
    <transition name="fade">
        <div class="alert">
            <p>Мы хотим стать лучше! Пожалуйста, пройдите опрос и оцените качество сервиса.
                <span @click="openModal">Пройти опрос</span>
            </p>
            <img src="../assets/close.svg" class="close" @click="$emit('close-alert')">
        </div>
    </transition>
</template>

<script>

export default {
  name: 'navBar',
     methods: {
        // ...
        openModal: function () {
            this.$emit('openModal');

        }
    },
}

</script>


<style lang="scss" scoped>

.alert {
    width: 100%;
    height: 30px;
    background-color: #60985c;
    padding: 0 95px 0 93px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    box-sizing: border-box;
    span{
        text-decoration: underline;
        cursor: pointer;
    }
    .close {
        width: 12px;
        height: 13px;
        object-fit: contain;
    }
}

</style>