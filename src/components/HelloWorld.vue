<template>
  <div class="wrapper">
    <h1>IP Address Tracker</h1>
    <form action="#">
      <label for="search"></label>
      <input type="text" id="search" name="search">
      <input type="submit" value="Submit">
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
<style scoped>

</style>
