<template>
  <div class="baidu-map">
    <label>距离当前定位最近的医院是： </label>
    <input v-model="pharmacy"></input>
    <baidu-map class="baidu-map" ak="Lz5CRM5Ba94DM7NbZGtPcmx8eXFAoNPq" :center="nearby.center" :zoom="13" :max-zoom="13" :min-zoom="13">
      <bm-view class="map"></bm-view>
      <!-- Search box -->
      <bm-control>
        <bm-auto-complete v-model="keyword" @confirm="goTodestination">
          <input class="search-box" placeholder="请输入道路交叉口或关键地标"></input>
        </bm-auto-complete>
      </bm-control>

      <!-- Relocate -->
      <bm-local-search :keyword="keyword" :auto-viewport="true" :panel="false" :selectFirstResult="true" :pageCapacity="1" @infohtmlset="getCurrentLocation" v-if="isAfterSearched"></bm-local-search>
      <bm-marker :position="nearby.center" :dragging="false" animation="BMAP_ANIMATION_BOUNCE"></bm-marker>

      <!-- Pharmacies result layer -->
      <bm-circle :center="nearby.center" :radius="nearby.radius"></bm-circle>
      <bm-local-search :keyword="searchFor" :auto-viewport="false" :nearby="nearby" :panel="false" :selectFirstResult="false" :pageCapacity="1" @markersset="getPharmacyList" ></bm-local-search>

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
      pharmacy: '',
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
      this.pharmacy = e[0].title;
      // console.log(e[0]);
      // console.log(e[0].uid);
      // console.log(e[0].address);
      // console.log(e[0].phoneNumber);
      // console.log(e[0].url);
      // TODO push pharmacy list
    // Vue.axios.get('http://localhost:8088/users/1').then((response) => {
    //   console.log(response.data)
    // });

    // Vue.axios.post('http://localhost:8088/users',{name: 'glen'}).then((response) => {
    //   console.log(response.data)
    // });

    Vue.axios.post('http://localhost:8088/pharmacies',{name: this.pharmacy}).then((response) => {
      console.log(response.data)
    });

    },
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
  width: 100%;
  height: 500px;
}
.search-box {
  position: absolute;
  top: 30px;
  left: 30px;
  width: 200px;
  height: 30px;
}
h3 {
  margin: 40px 0 0;
}
</style>
