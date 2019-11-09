<template>
  <div class="button-container">
    <ArrowBack w="100px" h="100px" v-if="name === 'back'" />
    <ArrowForward w="100px" h="100px" v-if="name === 'forward'" />
    <Download w="100px" h="100px" v-if="name === 'download'" />
  </div>
</template>

<script>
import ArrowBack from 'vue-ionicons/dist/ios-arrow-back';
import ArrowForward from 'vue-ionicons/dist/ios-arrow-forward';
import Download from 'vue-ionicons/dist/ios-download';

export default {
  props: ['name'],
  components: {
    ArrowBack,
    ArrowForward,
    Download,
  },
}
</script>

<style lang="scss">
  .button-container {
    padding: 5px;
    margin: 10px;
    display: inline-block;
    transition: .3s;
    border-radius: 100%;
    cursor: pointer;
    
    &:hover {
      padding: 20px;
      margin: 10px;
      background: #f0f0f0;
    }

    &:active {
      background-color: rgba(0, 0, 0, 0.2);
    }

    .ion {
      display: block;
      .ion__svg {
        display: block;
      }
    }
  }
</style>