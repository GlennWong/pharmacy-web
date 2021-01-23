<template>
  <div class="baidu-map">
    <h2>{{ msg }}</h2>
    <baidu-map class="baidu-map" ak="Lz5CRM5Ba94DM7NbZGtPcmx8eXFAoNPq" :center="nearby.center" :zoom="13" :max-zoom="13" :min-zoom="13">
      <label>Search and Select Location</label>
      <bm-view class="map"></bm-view>

      <!-- Search box -->
      <bm-control>
        <bm-auto-complete v-model="keyword" @confirm="goTodestination">
          <input class="search-box" placeholder="请输入地名关键字"></input>
        </bm-auto-complete>
      </bm-control>

      <!-- Relocate -->
      <bm-local-search class="search-location" :keyword="keyword" :auto-viewport="true" :panel="false" :selectFirstResult="true" :pageCapacity="1" @infohtmlset="getCurrentLocation" v-if="isAfterSearched"></bm-local-search>
      <bm-marker :position="nearby.center" :dragging="false" animation="BMAP_ANIMATION_BOUNCE"></bm-marker>

      <!-- Pharmacies result layer -->
      <bm-circle :center="nearby.center" :radius="nearby.radius"></bm-circle>
      <bm-local-search class="search-pharmacies" :keyword="searchFor" :auto-viewport="true" :nearby="nearby" :panel="true" :selectFirstResult="false" :pageCapacity="5" @markersset="getPharmacyList" ></bm-local-search>

    </baidu-map>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import {
  BaiduMap,
  BmView,
  BmControl,
  BmAutoComplete,
  BmMarker,
  BmInfoWindow,
  BmLocalSearch,
  BmCircle,
} from 'vue-baidu-map/components/';

export default Vue.extend({
  name: 'baidu-map2',
  data() {
    return {
      keyword: '',
      searchFor: '药店',
      isAfterSearched: true,
      nearby: {
        center: {
          lng: 116.404,
          lat: 39.915,
        },
        radius: 5000,
      },
    };
  },
  methods: {
    goTodestination() {
      this.isAfterSearched = true;
    },
    getCurrentLocation(e: { point: { lng: any; lat: any; }; }) {
      // console.log(e);
      const {lng, lat} = e.point;
      this.nearby.center.lng = lng;
      this.nearby.center.lat = lat;
      this.isAfterSearched = false;
    },
    getPharmacyList(e: any) {
      // console.log(e);
      // TODO push pharmacy list
    },
  },
  props: {
    msg: String,
  },
  components: {
    BaiduMap,
    BmView,
    BmControl,
    BmAutoComplete,
    BmMarker,
    BmInfoWindow,
    BmLocalSearch,
    BmCircle,
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.map {
  width: 79%;
  float: left;
  height: 500px;
}
.search-box {
  position: absolute;
  top: 30px;
  left: 30px;
  width: 200px;
  height: 30px;
}
.search-location {
  display: none;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
