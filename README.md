# circle-progress 

> vue圆环形进度条组件

##### 1.先来个圆形背景，为了后续方便定位，请给它安排上`position`属性。

**CircleProgress.vue**

```
<template>
  <div class="circle-progress"></div>
</template>

<script>
export default {
  name: "circle-progress"
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
  overflow: hidden;  // 这个属性十分关键，超过的都隐藏
}
</style>
```

![在调用的父组件里设置了长宽为100px](https://upload-images.jianshu.io/upload_images/7016617-69faa0d12b076cc2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 2.新增两个div，一个用来做进度条，另一个用来放内容。

```
<template>
  <div class="circle-progress">
      <div class="progress"></div>
      <div class="content"></div>
  </div>
</template>

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
  .progress, .content {
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
  }
}
</style>
```

##### 3.进度条部分

1. 将圆分割成四等分，每一份都占四分之一(0.25)，角度都为90度。顺时针写样式。

![block1](https://upload-images.jianshu.io/upload_images/7016617-234f808d424506e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![block2](https://upload-images.jianshu.io/upload_images/7016617-f53dd3144675c2e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![block3](https://upload-images.jianshu.io/upload_images/7016617-357396617d1b28a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![block4](https://upload-images.jianshu.io/upload_images/7016617-a30ae3580e8c6d41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.都给他们加上背景色，为进度条的颜色。

![#64d3f6](https://upload-images.jianshu.io/upload_images/7016617-e45c78ea994c4adc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.每一块里都加上遮挡层，遮挡层颜色与背景色相同，设定样式以背景圆的圆心为旋转点。

![第一块的旋转中心应该为左下角](https://upload-images.jianshu.io/upload_images/7016617-fd1735e9bd5b5ed2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.给每一块的modal都加上`v-if`属性，只有当比例小于这一块才显示modal层。

例如：进度(rate)为0.25，第一块modal不显示(不满足rate小于0.25)，第二块modal显示(rate < 0.5)，
第三块modal显示（rate < 0.75），第四块modal显示（rate < 1）；

5.加上动态样式

① 顺时针第一块，右上角那块(范围应该为0~0.25)：

+ 如果进度(rate)大于等于0.25，则不显示与背景色一样为紫色的modal层。
+ 如果进度(rate)在0~0.25之间，比如0.125，要显示与背景色一样为紫色的modal层，且modal层只显示一半，且为后半部分，即顺时针旋转(rate * 0.25 * 90deg)。

![modal1层显示了，且旋转了45°](https://upload-images.jianshu.io/upload_images/7016617-6ccdbb1edb0f5089.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

② 顺时针第二块，右下角那块(范围应该为0.25~0.5)：

+ 如果进度(rate)大于等于0.5，则不显示与背景色一样为紫色的modal层。
+ 如果进度(rate)小于0.25，要显示与背景色一样为紫色的modal层，且不用旋转任何角度。
+ 如果进度(rate)在0.25~0.5之间，比如0.3，要显示与背景色一样为紫色的modal层，且modal层顺时针旋转((rate - 0.25) * 0.25 * 90deg)。

![modal2显示了，且旋转了18°](https://upload-images.jianshu.io/upload_images/7016617-ebe0da32476d46fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


③ 顺时针第三块，左下角那块(范围应该为0.5~0.75)：

+ 如果进度(rate)大于等于0.75，则不显示与背景色一样为紫色的modal层。
+ 如果进度(rate)小于0.5，要显示与背景色一样为紫色的modal层，且不用旋转任何角度。
+ 如果进度(rate)在0.5~0.75之间，比如0.7，要显示与背景色一样为紫色的modal层，且modal层顺时针旋转((rate - 0.5) * 0.25 * 90deg)。

![modal3显示了，且旋转了72°](https://upload-images.jianshu.io/upload_images/7016617-7e36e24d07516c19.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

④ 顺时针第四块，左上角那块(范围应该为0.75~1)：

+ 如果进度(rate)大于等于1，则不显示与背景色一样为紫色的modal层。
+ 如果进度(rate)小于0.75，要显示与背景色一样为紫色的modal层，且不用旋转任何角度。
+ 如果进度(rate)在0.75~0.1之间，比如0.8，要显示与背景色一样为紫色的modal层，且modal层顺时针旋转((rate - 0.75) * 0.25 * 90deg)。

![modal4显示了，且旋转了18°](https://upload-images.jianshu.io/upload_images/7016617-5bc71015f4c0bfc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以可以将上述四个显示的modal层旋转的角度的计算方式写成一个函数，根据是第几块和当前的进度比例（this.rate）来得到角度：

```
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
    <div class="content"></div>
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
}
</style>
```

6.说好的圆环形进度条呢？怎么变成圆饼啦？在content里加上圆，挡住中心部分
```
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
```

像这样调用即可：

```
 <circle-progress :rate="0.8"></circle-progress>
```

![图1](https://upload-images.jianshu.io/upload_images/7016617-b071e612685596d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![图2](https://upload-images.jianshu.io/upload_images/7016617-8fc8647656bcc9da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
