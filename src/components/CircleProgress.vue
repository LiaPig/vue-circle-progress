<template>
  <div class="circle-progress">
    <div class="progress">
      <div v-for="item in 4" :key="item" class="block" :class="`block${item}`">
        <div
          v-if="rate < 0.25 * item"
          class="modal"
          :class="`modal${item}`"
          :style="{transform: getProgress(item)}"
        ></div>
      </div>
    </div>
    <div class="content">
        <div class="content-bg1">
            <div class="content-bg2"></div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "circle-progress",
  props: {
    rate: {
      default: 0
    }
  },
  methods: {
    getProgress(index) {
      const { rate } = this;
      if (index === 1) {
        return `rotate(${(rate / 0.25) * 90}deg)`;
      }
      if (index === 2) {
        if (rate > 0.25 && rate < 0.5) {
          return `rotate(${((rate - 0.25) / 0.25) * 90}deg)`;
        }
      }
      if (index === 3) {
        if (rate > 0.5 && rate < 0.75) {
          return `rotate(${((rate - 0.5) / 0.25) * 90}deg)`;
        }
      }
      if (index === 4) {
        if (rate > 0.75) {
          return `rotate(${((rate - 0.75) / 0.25) * 90}deg)`;
        }
      }
    }
  }
};
</script>

<style scoped lang="scss">
.circle-progress {
  position: relative;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: #5140b4;
  border-radius: 100%;
  overflow: hidden;
  .progress,
  .content {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
  }
  .progress {
    .block {
      position: absolute;
      width: 50%;
      height: 50%;
      overflow: hidden;
      background-color: #64d3f6;
      &1 {
        left: 50%;
        top: 0;
      }
      &2 {
        left: 50%;
        top: 50%;
      }
      &3 {
        left: 0;
        top: 50%;
      }
      &4 {
        left: 0;
        top: 0;
      }
      .modal {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background-color: #5140b4;
        // transform: rotate(-90deg);
        &1 {
          transform-origin: 0 100%;
        }
        &2 {
          transform-origin: 0 0;
        }
        &3 {
          transform-origin: 100% 0;
        }
        &4 {
          transform-origin: 100% 100%;
        }
      }
    }
  }
  .content {
      .content-bg1, .content-bg2 {
          position: absolute;
          left: 5%;
          top: 5%;
          width: calc(100% - 10%);
          height: calc(100% - 10%);
          background-color: #4f40cd;
          border-radius: 100%;
      }
      .content-bg2 {
          background-color: #34277d;
      }
  }
}
</style>
