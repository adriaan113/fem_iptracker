
<template>
  <div class="wrapper">
    <form action="#">
      <label for="search"></label>
      <input v-model="searchBar" type="text" id="search" name="search" :placeholder="placeHolderText" :class="{warning: warning}">
      <input @click="userSearch" type="submit" value=">">
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
        <div  class="map-container">
          <l-map
            :zoom="zoom"
            :center="center"
            @update:zoom="zoomUpdated"
            @update:center="centerUpdated"
            @update:bounds="boundsUpdated"
            id="l-map"
          >
            <l-tile-layer :url="koeka"></l-tile-layer>
            <l-marker :latLng="marker">
              <l-icon 
                :icon-size="dynamicSize"
                :icon-anchor="dynamicAnchor"
                :icon-url="iconUrl"
              />
            </l-marker>
          </l-map>
        </div>
    </div>
  </div>
</template>

<script>

import { latLng,Icon } from "leaflet";
import {LMap, LTileLayer,LMarker, LIcon} from 'vue2-leaflet';

//webpack solution for displaying the icon
delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
});

  export default {
    name: 'InnerApp',
    props:{
    },
    components: {
    LMap,
    LTileLayer,
    LMarker,
    LIcon
    },
    data: function(){
      return{
        apiKey: "at_spF8wiWyTRdL47h4nPRmiaWmx7wPR",
        ip: '',
        location: '',
        timezone: '',
        isp: '',
        searchBar: '',

        lat: '',
        lng: '',

        koeka: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        zoom: 15,
        center: latLng(52.3075,4.97222),//hier nog even naar kijken. moet geen hardcode zijn
        bounds: null,

        marker: latLng(52.3075,4.97222),//hier ook

        iconUrl: require("../assets/icon-location.svg"),
        iconSize: [32, 37],
        iconAnchor: [16, 37],
        
        placeHolderText: 'IP adress here',
        warningText: `You've entered a invalid IP adress`,
        warning: false,
      }
    },
    computed:{
      url(){
        if(this.ip === ''){
          return `https://geo.ipify.org/api/v1?apiKey=${this.apiKey}`;
        }else{
          return `https://geo.ipify.org/api/v1?apiKey=${this.apiKey}&ipAddress=${this.ip}`;
        }
      },
      dynamicSize() {
      return [this.iconSize, this.iconSize * 1.15];
      },
      dynamicAnchor() {
        return [this.iconSize / 2, this.iconSize * 1.15];
      }
    },
    methods:{
      async getInfo(){
        try{
          let response = await fetch(this.url);
          let result = await response.json();
          this.ip = result.ip;
          this.location = result.location.city;
          this.location += ', ' + result.location.country;
          this.timezone = result.location.timezone;
          this.isp = result.isp;

          this.lat = result.location.lat;
          this.lng = result.location.lng;
        
          this.center = latLng(this.lat, this.lng);
          this.marker = latLng(this.lat, this.lng);
        }catch(error){
          console.log(`Dit gaat even niet goed want: ${error}`);
        }
      },
      userSearch(e){
        e.preventDefault();
        this.warning = false;
        if(this.searchBar!= ''){
          if(/^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/.test(this.searchBar)){
            this.ip = this.searchBar;
            this.getInfo();
          }else{
            this.wrongIpAdress();
          }
            
        } 
      },
      zoomUpdated (zoom) {
        this.zoom = zoom;
      },
      centerUpdated (center) {
        this.center = center;
      },
      boundsUpdated (bounds) {
        this.bounds = bounds;
      },
      wrongIpAdress(){
        this.searchBar = '';
        this.warning = true;
        this.placeHolderText = this.warningText;
        this.resetForm();
      },
      resetForm(){
        setTimeout(()=>{
          this.placeHolderText = 'IP adress here';
          this.warning = false;
        },1500);
          
      },
    }, 
    created(){
      this.getInfo();
    },
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
        border: 2px solid transparent;
      }
      input[type="text"]{
        width: 80vw;
        border-radius: $border 0 0 $border;
        padding-left: 1rem;
        color: $tertiary-text-color;
        outline: none;
        :focus{
          outline: none;
        }
      }
      input[type="submit"]{
        width: 20%;
        max-width: 3rem;
        background-color: $btn-color;
        color: $primary-text-color;
        border-radius:  0 $border $border 0;
        cursor: pointer;
        height: calc(2.5rem + 5px);
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
      z-index: 999;
      @media  (min-width: $medium) {
        flex-flow: row nowrap;
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
      width: 100%;
      height: 67vh;
      position: absolute; 
      .map-container{
        height: 100%;
      }
      #l-map{
        height: 100%; 
        width: 100%;
      }
  }

  .warning{
    border: 2px solid $warning;
  }

}
</style>
