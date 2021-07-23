<template>
  <div class="wrapper">
    <form action="#">
      <label for="search"></label>
      <input type="text" id="search" name="search">
      <input type="submit" value=">">
    </form>

    <ul class="info">
      <li class="info__item">
        <p>IP Address</p>
        <h2>192.212.174.101</h2>
      </li>
      <li class="info__item">
        <p>Location</p>
        <h2>Brooklyn, NY 10001</h2>
      </li>
      <li class="info__item">
        <p>Timezone</p>
        <h2>UTC -05:00</h2>
      </li>
      <li class="info__item">
        <p>ISP</p>
        <h2>SpaceX Starlink</h2>
      </li>
    </ul>
    <div class="map">
      <img :src="image" alt="">
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
    props:{
      image: Object,
  }
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import "../global.scss";

  .wrapper{
    display: flex;
    flex-flow: column nowrap;
    align-items: center;
    form{
      position: absolute;
      top: 10%;
      width: 100%;
      display: flex;
      justify-content: center;
      input{
        height: 2rem;
        margin: 0;
        padding: 0;
        border-width: 0;
      }
      input[type="text"]{
        width: 80%;
        border-radius: $border 0 0 $border;
      }
      input[type="submit"]{
        width: 10%;
        background-color: $btn-color;
        color: $primary-text-color;
        border-radius:  0 $border $border 0;
      }
    }

    .map{
      img{
      width: 100%;
      height: auto;
      }  
  }

}


</style>
