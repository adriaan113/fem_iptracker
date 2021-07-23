<template>
  <div id="app">
    <h1 class="title">IP Address Tracker</h1>
    <img class="background-img" :src="backgroundImg" alt="">
    <HelloWorld :image="map"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data: function(){
    return{
      backgroundImg: require('/src/assets/pattern-bg.png'),
      map: require('/src/assets/Maps.png')
    }
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap');
@import "./global.scss";

html,
body{
  margin: 0;
  padding: 0;

}
#app {
  font-family: 'Roboto', sans-serif;
  font-size: 16px;
  width: 100%;
  height: 100vh;
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  .title{
    margin: 0 auto;
    position: absolute;
    color: $primary-text-color;
  }
  .background-img{
    width: 100%;
    height: 33vh;
    //height: 100%;
  }
}
</style>
