<template>
  <div>
    <p class="red">我是子组件</p>
    <p class="red">{{dataList.title}}</p>
    <button @click="_noticeFather">
      点我通知父组件
    </button>
    <br/>
    <br/>
    <button @click="_eventBusFather">
      点我通知父组件（eventBus）
    </button>
    <br/>
    <br/>
    <button @click="_provide">
      点我通知父组件（provide/inject）
    </button>
  </div>
</template>

<script>
import eventBus from "../../eventBus.js";

export default {
  name: 'appSon',
  inject: ["provideData"],
  props: {
    dataList: {
      type: [Object],
    },
    
  },
  mounted(){
    console.log(this.dataList.title)
  },
  methods:{
    _changeSonColor(data){
      alert(data)
    },
    // 通过$emit 通知父组件
    _noticeFather(){
      this.$emit("notice",{name:"通知父组件的参数1",data:"通知父组件的参数2"})
    },
    // 通过 eventbus 通信
    _eventBusFather(){
      eventBus.$emit("_noticeFather",{name:"kangkang",age:"18"})
    },
    // 通过 provide/inject
    _provide(){
      // 在这里，你可以看到惊喜，provide的父组件所有的参数，方法，你都可以拿到
      // 你可以在这个子组件 当做在父组件一样 操作父组件
      // 比如调用方法，直接修改父组件的data值
      console.log(this.provideData)  //可以打印一下，看看
      this.provideData.appData.title = "provide/inject"; //直接修改 父组件的data
      this.provideData.$refs.appSonRef._changeSonColor("改变颜色") // 直接调用 父组件的 方法；并且通知子组件
    }
  }
}
</script>

<style scope>
  .red{
    color: red; 
  }
</style>
