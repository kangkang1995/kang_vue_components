<template>
  <div id="app">
    <p @click="_changeColor">我是父组件</p>
    <app-son :dataList="appData" ref="appSonRef" @notice="_receiveSon"></app-son>
  </div>
</template>

<script>

import appSon from "./components/appSon/index"; //引入子组件appSon
import eventBus from "./eventBus";
export default {
  name: 'app',
  provide() {
    return { provideData: this };
  },
  components: {
    appSon
  },
  data(){
    return {
      appData:{
        title:"我是父组件的dataList数据"
      }
    }
  },
  mounted(){
    this._getEnentBus();
  },
  methods:{
    // 监听 eventBus 的$emit时间 
    _getEnentBus(){
      // 这里记得 that = this;因为eventBus.$on里的this是eventBus的实例，不是父组件的this
      const that = this;
      eventBus.$on("_noticeFather", function(val) {
        // 做些 事情
        console.log(val)
      });
      // 下面可以继续监听其它的方法
      // eventBus.$on("_noticeFather1111", function(val) {
      //   // 做些 事情
      //   console.log(data)
      // });
    },
    _changeColor(){
      this.$refs.appSonRef._changeSonColor("改变颜色")
    },
    // 接收 子组件
    _receiveSon(data){
      console.log(data)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
