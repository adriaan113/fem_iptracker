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
        <h2>{{ip}}</h2>
      </li>
      <li class="info__item">
        <p>Location</p>
        <h2>{{location}}</h2>
      </li>
      <li class="info__item">
        <p>Timezone</p>
        <h2>{{timezone}}</h2>
      </li>
      <li class="info__item">
        <p>ISP</p>
        <h2>{{isp}}</h2>
      </li>
    </ul>
    <div class="map">
      <!-- <img :src="image" alt=""> -->
    </div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    props:{
        // image: Object,
    },
    data: function(){
      return{
        // ip: "8.8.8.8", //heb ik eigenlijk niet nodig geloof ik

        apiKey: "at_spF8wiWyTRdL47h4nPRmiaWmx7wPR",
        ip: '',
        location: '',
        timezone: '',
        isp: '',
      }
    },
    computed:{
      url(){
        return `https://geo.ipify.org/api/v1?apiKey=${this.apiKey}`;
      }
    },
    methods:{
      async getInfo(){
        try{
          let response = await fetch(this.url);
          let result = await response.json();
          console.log(result);
          this.ip = result.ip;
          this.location = result.location.city;
          this.location += ', ' + result.location.country;
          this.timezone = result.location.timezone;
          this.isp = result.isp;

        }catch(error){
          console.log(`Dit gaat even niet goed want: ${error}`);
        }
      },
    }, 
    mounted:  function(){
      this.getInfo();
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
      max-width: 35rem;
      display: flex;
      justify-content: center;
      input{
        height: 2.5rem;
        margin: 0;
        padding: 0;
        border-width: 0;
      }
      input[type="text"]{
        width: 80vw;
        border-radius: $border 0 0 $border;
        padding-left: 1rem;
        color: $tertiary-text-color;
      }
      input[type="submit"]{
        width: 10%;
        max-width: 3rem;
        background-color: $btn-color;
        color: $primary-text-color;
        border-radius:  0 $border $border 0;
        cursor: pointer;
        &:hover{
          background-color: $btn-hover-color;
        }
      }
    }

    .info{
      background-color: $background-color;
      list-style: none;
      border-radius: $border;
      display: flex;
      flex-flow: column nowrap;
      align-items: center;
      padding: 1rem 0;
      margin:  -5rem 0 0;
      width: 90vw;
      max-width: 69rem;
      z-index: 2;
      @media  (min-width: $medium) {
        flex-flow: row nowrap;
        // padding: 1rem 0 2rem;
      }
      &__item{
        display: flex;
        flex-flow: column nowrap;
        align-items: center;
        width: 100%;
        @media  (min-width: $medium) {
          margin: .5rem 1rem;
          align-items: flex-start;
          border-left: 2px solid lighten($line-color, 40%);
          padding-left: 1.5rem;
          height: 4.5rem;
          &:nth-child(1){
            border-left: 0;
          }
        }
        p{
          color: $tertiary-text-color;
          @media  (min-width: $medium) {
            margin-top: 0;
          }
        }
        h2{
          margin: 0 0 .5rem;
          @media  (min-width: $medium) {
            max-width: 6rem;
            margin-bottom: 0;
          }
        }
        &:nth-child(1){
          h2{
            font-weight: lighter;
          }
        }
      }
    }

    .map{
        // background-image: url("../assets/Maps.png");
        // background-repeat: no-repeat;
        // background-size: cover;
        background-color: red;
        width: 100%;
        height: 67vh;
        position: absolute; 
  }

}


</style>
