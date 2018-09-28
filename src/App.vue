<template>
  <div id="app">
    <div class="sidebar" :style="{backgroundColor: bgColor}">
      <ul>
        <li id="js-item-1-3" :style="{color: textColor}" @click="switchDrawer1" @mouseover="hoverItem(1,3)" @mouseout="leaveItem(1,3)">
          <span :style="[{backgroundColor: textColor}, {opacity: '.4'}]"></span>点击打开 drawer
        </li>
        <li :id="'js-item-1-' + index" :style="{color: textColor}" :key="item.id" v-for="(item, index) in 3" @mouseover="hoverItem(1,index)" @mouseout="leaveItem(1,index)">
          <span :style="[{backgroundColor: textColor}, {opacity: '.4'}]"></span>示例选项
        </li>
      </ul>
      <ul>
        <li id="js-item-2-3" :style="{color: textColor}" @mouseover="openDrawer2();hoverItem(2,3)" @mouseout="timeoutCloseDrawer2();leaveItem(2,3)">
          <span :style="[{backgroundColor: textColor}, {opacity: '.4'}]"></span>hover 打开 drawer
        </li>
        <li :id="'js-item-2-' + index" :style="{color: textColor}" :key="item.id" v-for="(item, index) in 3" @mouseover="hoverItem(2,index)" @mouseout="leaveItem(2,index)">
          <span :style="[{backgroundColor: textColor}, {opacity: '.4'}]"></span>示例选项
        </li>
      </ul>
    </div>
    <transition name="fade">
      <div class="show-drawer" v-if="showDrawer1">
        <div class="mask" style="z-index:2" @click="closeDrawer1"></div>
        <div class="drawer" style="z-index:3">
          <div class="title">Drawer1<span @click="closeDrawer1">×</span></div>
          <div class="desc">点击打开，手动关闭</div>
        </div>
      </div>
    </transition>
    <transition name="fade">
      <div class="show-drawer" v-if="showDrawer2">
        <div class="mask" style="z-index:4"></div>
        <div class="drawer" style="z-index:5" @mouseover="hoverDrawer2" @mouseout="leaveDrawer2">
          <div class="title">Drawer2</div>
          <div class="desc">hover 打开，鼠标移动到 drawer 元素上面就保持打开，鼠标移出 drawer 元素就关闭</div>
        </div>
      </div>
    </transition>
    <div class="color-setting">
      <div>
        文字色
        <input type="range" v-model="textRgb" min="0" max="255">
      </div>
      <div>
        背景色
        <input type="range" v-model="bgRgb" min="0" max="255">
      </div>
    </div>
    <div class="content">
      <div class="item" :key="item.id" v-for="item in 19"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      bgRgb: 50,
      textRgb: 200,
      showDrawer1: false,
      showDrawer2: false,
      isHover: false,
    }
  },
  computed: {
    bgColor () {
      return 'rgba(' + this.bgRgb + ',' + this.bgRgb + ',' + this.bgRgb + ',1)'
    },
    textColor () {
      return 'rgba(' + this.textRgb + ',' + this.textRgb + ',' + this.textRgb + ',1)'
    },
  },
  methods: {
    hoverItem(e,index) {
      document.getElementById(`js-item-${e}-${index}`).style.backgroundColor = 'rgba(' + this.textRgb + ',' + this.textRgb + ',' + this.textRgb + ',.2)'
    },
    leaveItem(e,index) {
      document.getElementById(`js-item-${e}-${index}`).style.backgroundColor = 'transparent'
    },
    closeDrawer1() {
      this.showDrawer1 = false
    },
    switchDrawer1() {
      this.showDrawer1 = !this.showDrawer1
    },
    openDrawer2() {
      this.showDrawer2 = true
    },
    timeoutCloseDrawer2() {
      setTimeout(() => {
        if (this.isHover === false) {
          this.showDrawer2 = false
        }
      }, 200)
    },
    hoverDrawer2() {
      this.isHover = true
    },
    leaveDrawer2() {
      this.isHover = false
      setTimeout(() => {
        if (this.isHover === false) {
          this.showDrawer2 = false
        }
      }, 200)
    },
  }
}
</script>

<style lang="scss">
.fade-enter-active, .fade-leave-active {
  transition: .3s;

  .drawer {
    left: 180px;
    transition: .3s;
  }

  .mask {
    opacity: 1;
    transition: .3s;
  }
}

.fade-enter, .fade-leave-to {
  transition: .3s;

  .drawer {
    left: -140px !important;
    transition: .3s;
  }

  .mask {
    opacity: 0;
    transition: .3s;
  }
}

body {
  margin: 0;
}

#app {
  width: 100vw;
  height: 100vh;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  
  .sidebar {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    width: 180px;
    background-color: #000;
    z-index: 100;

    ul {
      margin-top: 60px;
      list-style: none;
      padding-inline-start: 0;

      li {
        margin: 0 8px;
        padding: 6px 10px;
        cursor: pointer;
        border-radius: 4px;
        font-size: 14px;

        span {
          display: inline-block;
          margin-right: 10px;
          width: 14px;
          height: 14px;
          border-radius: 50%;
        }
      }
    }
  }

  .color-setting {
    position: absolute;
    right: 20px;
    bottom: 20px;
    z-index: 100;

    span {
      color: #333;
    }
  }

  .mask {
    position: fixed;
    top: 0;
    left: 180px;
    bottom: 0;
    right: 0;
    background-color: rgba(0,0,0,0.4);
    cursor: pointer;
  }

  .drawer {
    position: absolute;
    top: 0;
    left: 180px;
    bottom: 0;
    width: 320px;
    background-color: white;
    box-shadow: 2px 0 10px rgba(0,0,0,0.1);

    .desc {
      padding: 60px 20px;
    }

    .title {
      position: fixed;
      top: 0;
      width: 280px;
      height: 30px;
      padding: 10px 20px;
      font-size: 18px;
      border-bottom: 1px solid #eee;
      background-color: white;
      cursor: pointer;

      span {
        float: right;
        font-size: 24px;
        color: #999;

        &:hover {
          color: #333;
        }
      }
    }
  }

  .content {
    position: fixed;
    top: 0;
    left: 180px;
    bottom: 0;
    right: 0;
    padding: 20px;
    background-color: #f8f8f8;
    z-index: 1;

    .item {
      display: inline-block;
      margin: 7px 10px;
      width: 320px;
      height: 60px;
      background-color: white;
      border-radius: 2px;
      box-shadow: 0 1px 2px rgba(0,0,0,0.1);

      &:hover {
        box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      }
    }
  }
}
</style>
