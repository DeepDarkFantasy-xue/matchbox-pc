<template>
  <div id="app">
    <div class="titleBar" >
      <div class="title">
        <div class="logo">
          <img src="@/assets/logo.png" />
        </div>
        <div class="txt">窗口标题</div>
      </div>
      <div class="windowTool">
        <div @click="minisize">
          <i class="iconfont iconminisize">&#xe7fd;</i>
        </div>
        <div v-if="isMaxSize" @click="restore">
          <i class="iconfont iconrestore">&#xe914;</i>
        </div>
        <div v-else @click="maxsize">
          <i class="iconfont iconmaxsize">&#xe915;</i>
        </div>
        <div @click="close" class="close">
          <i class="iconfont iconclose">&#xe7fc;</i>
        </div>
      </div>
    </div>
    <div class="content">
      <router-view />
    </div>
  </div>
</template>
<script>
let { remote } = require('electron');
export default {
  data() {
    return {
      isMaxSize: false
    }
  },
  mounted () {
    let win = remote.getCurrentWindow();
    win.on('maximize',() => {
      this.isMaxSize = true;
      this.setState();
    })
    win.on('unmaximize', () => {
      this.isMaxSize = false;
      this.setState();
    })
    win.on('move', this.throttle(() => {
      console.log(1);
    }))
    win.on('resize', this.throttle(() => {
      console.log(2);
    }))
  },
  methods: {
    close() {
      remote.getCurrentWindow().close();
    },
    minisize() {
      console.log(remote);
      remote.getCurrentWindow().minimize();
    },
    restore() {
      remote.getCurrentWindow().restore();
    },
    maxsize() {
      remote.getCurrentWindow().maximize();
    },
    throttle(fn) {
      let timeout = null;
      return function() {
        if(timeout) return;
        timeout = setTimeout(() => {
          fn.apply(this, arguments);
          timeout = null;
        },2000)
      }
    }
  },
};
</script>
<style lang="scss">
@font-face {
  font-family: 'iconfont'; /* project id 2390052 */
  src: url('//at.alicdn.com/t/font_2390052_5r70h1tq736.eot');
  src: url('//at.alicdn.com/t/font_2390052_5r70h1tq736.eot?#iefix') format('embedded-opentype'),
    url('//at.alicdn.com/t/font_2390052_5r70h1tq736.woff2') format('woff2'),
    url('//at.alicdn.com/t/font_2390052_5r70h1tq736.woff') format('woff'),
    url('//at.alicdn.com/t/font_2390052_5r70h1tq736.ttf') format('truetype'),
    url('//at.alicdn.com/t/font_2390052_5r70h1tq736.svg#iconfont') format('svg');
}
.iconfont {
  font-family: 'iconfont' !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
body,
html {
  margin: 0px;
  padding: 0px;
  overflow: hidden;
  height: 100%;
}
#app {
  text-align: center;
  margin: 0px;
  padding: 0px;
  height: 100%;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}
.titleBar {
  height: 38px;
  line-height: 36px;
  background: #ffffff;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.title {
  flex: 1;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  -webkit-app-region: drag;
  .logo {
    padding-left: 8px;
    padding-right: 6px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    img {
      width: 20px;
      height: 20px;
      margin-top: 7px;
    }
  }
}
.windowTool {
  div {
    color: #888;
    height: 100%;
    width: 38px;
    display: inline-block;
    cursor: pointer;
    i {
      font-size: 12px;
    }
    &:hover {
      background: hsl(0, 0%, 28%);
    }
  }
  .close:hover {
    color:  hsl(0, 0%, 28%);
  }
}
.content {
  flex: 1;
  overflow-y: auto;
  overflow-x: auto;
}
</style>
