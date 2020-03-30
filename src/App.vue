<template>
  <div id="app">
    <div class="content">
      <div v-for="(item, index) in progress" :key="index" class="circle">
        <div>当前进度：{{item}}({{item * 100}}%)</div>
        <div class="item">
          <circle-progress :rate="item"></circle-progress>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CircleProgress from './components/CircleProgress.vue'

export default {
  name: 'App',
  components: {
    CircleProgress
  },
  data() {
    return {
      progress: [1, 0.8, 0.7, 0.5, 0.3, 0.25, 0.125, 0]
    }
  }
}
</script>

<style lang="scss" scoped>
#app {
  margin: 0;
  padding: 0;
  text-align: center;
  .content {
    margin: 0 auto;
    display: block;
    width: 100px;
    .circle {
      margin: 20px 0;
      width: 100px;
      .item {
      display: block;
      width: 100px;
      height: 100px;
    }
    }
  }
}
</style>
