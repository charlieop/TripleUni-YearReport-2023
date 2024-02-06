<template>
  <div class="pages-container page6">
    <div class="page page6-1" style="color: whitesmoke">
      <p>这里是第六页</p>
      <p>请在此写canvas</p>
      <p>背景最终会是一个linear gradient, 但是还没定是什么颜色</p>
    </div>
  </div>
</template>

<script setup>
import PostComponnet from "@/components/PostComponnet.vue";

import { toRef, computed, onMounted } from "vue";

const props = defineProps({
  collageInfo: Object,
  collage: Number,
  userInfo: Object,
});

const collageInfoRef = toRef(props, "collageInfo");
const collageRef = toRef(props, "collage");
const userInfoRef = toRef(props, "userInfo");

onMounted(() => {
  const observer = new IntersectionObserver(animate, {
    root: document.querySelector(".content-wrapper"),
    rootMargin: "0px 0px 0px 0px",
    threshold: 0.8,
  });
  const pages = document.querySelectorAll(".page6 .page");
  pages.forEach((page) => {
    observer.observe(page);
  });
});

function animate(e) {
  if (!e[0].isIntersecting) return;
  const areas = e[0].target.querySelectorAll(".hide");
  let timer = 500;
  areas.forEach((area) => {
    setTimeout(() => {
      area.classList.remove("hide");
    }, timer);
    timer += 1000;
  });
}

function next() {
  const container = document.querySelector(".content-wrapper");
  container.scrollBy({
    top: container.clientHeight,
    behavior: "smooth",
  });
}
</script>

<style scoped>
.pages-container {
  background: linear-gradient(180deg, #00B9FF 10%, #D3F4FF 90%);
}
</style>
