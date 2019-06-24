<template>
  <div class="round-profile" :style="`background-image: url(${img}); width: ${width}px; height: ${height}px`"></div>
</template>

<script>
  export default {
    name: 'PhotoPerson',
    props: {
      width: {
        default: 150
      },
      height: {
        default: 150
      },
      img: {
        default: 'https://via.placeholder.com/150'
      }
    }
  }
</script>

<style lang="scss" scoped>
  .round-profile {
    background-position: center;
    background-size: cover;
    border-radius: 50%;
    background-color: #c3c3c3;
  }
</style>
