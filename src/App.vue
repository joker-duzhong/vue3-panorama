<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import SphericalPanorama from "./components/SphericalPanorama.vue";
import SquarePanorama from "./components/SquarePanorama.vue";

const spherical =
  "https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg.zcool.cn%2Fcommunity%2F018c5d554561c40000019ae931d796.jpg%402o.jpg&refer=http%3A%2F%2Fimg.zcool.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1664335285&t=0345e49a23e52ee188dba4f4a5d6a242";
const square =
  "https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fwww.hnr.cn%2Fnews%2Ftppd%2Ftpxw%2F201503%2FW020150326357142467617.jpg&refer=http%3A%2F%2Fwww.hnr.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1664333982&t=831a5623572d90e296424c0274c3ce03";
</script>

<template>
  <div class="lxyy">
    <SphericalPanorama :src="spherical" />
    <!-- <SquarePanorama
      :east="square"
      :west="square"
      :south="square"
      :north="square"
      :up="square"
      :down="square"
    /> -->
  </div>
</template>
