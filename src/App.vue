<template>
  <div id="app">
    <header>
      <h1 class="title">IP Address Tracker</h1>
    </header>
    
    <!-- <img class="background-img" :src="backgroundImg" alt=""> -->
    <HelloWorld/>
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
      // backgroundImg: require('/src/assets/pattern-bg.png'),
      // map: require('/src/assets/Maps.png')
    }
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300&display=swap');
@import "./global.scss";

html,
body{
  margin: 0;
  padding: 0;
}

h1{
  font-size: $h1-mobile-font-size;
}
h2{
  font-size: $h2-mobile-font-size;
}
p{
  font-size: $p-mobile-font-size;
}

#app {
  font-family: 'Roboto', sans-serif;
  font-size: 16px;
  width: 100%;
  height: 100vh;
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  header{
    width: 100%;
    height: 33vh;
    background-image: url("./assets/pattern-bg.png");
    background-repeat: no-repeat;
    background-size: cover;
    display: flex;
    flex-flow: column nowrap;
    align-items: center;
    .title{
      font-size: $h1-mobile-font-size;
      position: absolute;
      color: $primary-text-color;
      font-weight: lighter;
    }
  }
}
</style>
