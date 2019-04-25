<template>
  <div class="navbar">
    <img src="../assets/opti-24-logo.svg" class="opti24_logo">
    <img src="../assets/gpn-logo.svg" class="gpn_logo">
  </div>
</template>

<script>

export default {
  name: 'navBar',
}
</script>

<style lang="scss" scoped>

.navbar {
    width: 100%;
    height: 60px;
    background-color: #404040;
    padding: 0 95px 0 93px;
    box-sizing: border-box;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
    .opti24_logo {
        width: 93px;
        height: 39px;
        object-fit: contain;
    }
    .gpn_logo {
        width: 79px;
        height: 39px;
        object-fit: contain;
        padding-bottom: 9px;
    }
}

</style>