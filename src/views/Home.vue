<template>
  <div class="home">
    <BaiduMap msg="Input your location to find the pharmacies nearby."/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import BaiduMap from '@/components/BaiduMap.vue'; // @ is an alias to /src

export default Vue.extend({
  name: 'home',
  components: {
    BaiduMap,
  },
});
</script>
