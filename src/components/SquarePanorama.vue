<script setup lang="ts">
import * as THREE from "three";
import {
  WebGLRenderer, //渲染器
  Scene, //场景
  PerspectiveCamera, //相机
  SphereGeometry, //材质
  DoubleSide, //渲染两个面
  Mesh, //组合
  BoxGeometry, //球体 全景用
  MeshBasicMaterial, //球体 全景用
  TextureLoader, //加载器  内部使用ImageLoader来加载文件
} from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls"; //镜头控制
import { defineComponent, ref, onMounted, watch } from "vue";
import Stats from "three/examples/jsm/libs/stats.module.js"; //性能监控插件

const props = defineProps<{
  east: string;
  west: string;
  south: string;
  north: string;
  up: string;
  down: string;
}>();
const loading = ref(true);
//提前创建好调试插件
const box = ref<HTMLElement | null>(null); //ref获取dom
const scence: Scene = new Scene(); //场景
const camera = new PerspectiveCamera( //新建相机
  120, //摄像机视锥体垂直视野角度
  window.innerWidth / window.innerHeight, //摄像机视锥体长宽比
  0.1, //摄像机视锥体近端面
  10000 //摄像机视锥体远端面
);
const stats = Stats(); //性能监控
var orbitControls: OrbitControls; //摄像机控制
var rander: WebGLRenderer; //渲染器
var boxGeometry: Mesh<BoxGeometry>;
var img3D: MeshBasicMaterial[] = [];
// 渲染器方法
function randerFun() {
  rander = new WebGLRenderer({ antialias: true, alpha: true }); //新建渲染器 antialias 否执行抗锯齿
  rander.setClearColor(0xffffff, 1); //更改渲染器颜色为默认
}
randerFun();

//新建场景
function scenceFun() {
  scence.name = "场景"; //场景名字
  camera.name = "相机"; //相机名字
  camera.position.set(0, 0, 1); //相机位置
  orbitControls = new OrbitControls(camera, rander.domElement); //相机控制插件 实现拖拽渲染
  orbitControls.autoRotate = true;
}
scenceFun();

//正方体 全景开始
function boxGeometryFun() {
  img3D.push(
    new MeshBasicMaterial({
      map: new TextureLoader().load(props.east),
      side: DoubleSide,
    })
  );
  img3D.push(
    new MeshBasicMaterial({
      map: new TextureLoader().load(props.west),
      side: DoubleSide,
    })
  );
  img3D.push(
    new MeshBasicMaterial({
      map: new TextureLoader().load(props.south),
      side: DoubleSide,
    })
  );

  img3D.push(
    new MeshBasicMaterial({
      map: new TextureLoader().load(props.north),
      side: DoubleSide,
    })
  );

  img3D.push(
    new MeshBasicMaterial({
      map: new TextureLoader().load(props.up),
      side: DoubleSide,
    })
  );
  img3D.push(
    new MeshBasicMaterial({
      map: new TextureLoader().load(props.down),
      side: DoubleSide,
    })
  );
  boxGeometry = new Mesh(new BoxGeometry(1000, 1000, 1000), img3D);
  scence.add(boxGeometry);
}
boxGeometryFun();

//aim定时执行 动画
var aim = () => {
  stats.update(); //刷新性能监控
  orbitControls.update();
  rander.render(scence, camera); //更新试图
  requestAnimationFrame(aim); //定时器 到时间调用自己
};

watch(loading, (e) => {
  if (!e) {
    box.value?.append(rander.domElement);
    document.body.appendChild(stats.dom);
    aim();
  }
});
//生命周期 页面加载完
onMounted(() => {
  rander.setSize(window.innerWidth, window.innerHeight); //更改渲染大小
  loading.value = false;
});
//使画布动态大小
window.onresize = () => {
  camera.aspect = window.innerWidth / window.innerHeight; //更改比例
  camera.updateProjectionMatrix(); //更新摄像机投影矩阵
  rander.setSize(window.innerWidth, window.innerHeight); //更改场景大小
};
// const clickCanvas = () => {
//   stop = !stop;
//   if (!stop) {
//     aim();
//   }
// };
</script>

<template>
  <div class="main" ref="box" @click.stop="clickCanvas">
    <div class="loading" v-if="loading">
      <h1>loading...</h1>
      <svg
        viewBox="0 0 1024 1024"
        width="1em"
        height="1em"
        aria-hidden="true"
        focusable="false"
        class="anticon-loading"
      >
        <path
          d="M988 548c-19.9 0-36-16.1-36-36 0-59.4-11.6-117-34.6-171.3a440.45 440.45 0 0 0-94.3-139.9 437.71 437.71 0 0 0-139.9-94.3C629 83.6 571.4 72 512 72c-19.9 0-36-16.1-36-36s16.1-36 36-36c69.1 0 136.2 13.5 199.3 40.3C772.3 66 827 103 874 150c47 47 83.9 101.8 109.7 162.7 26.7 63.1 40.2 130.2 40.2 199.3.1 19.9-16 36-35.9 36z"
        ></path>
      </svg>
    </div>
  </div>
</template>

<style scoped>
.anticon-loading {
  display: inline-block;
  animation: loadingCircle 1s linear infinite;
}
@keyframes loadingCircle {
  100% {
    transform: rotate(1turn);
  }
}
* {
  padding: 0;
  margin: 0;
}
.main {
  height: 100%;
  width: 100%;
}
.loading {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}
</style>
