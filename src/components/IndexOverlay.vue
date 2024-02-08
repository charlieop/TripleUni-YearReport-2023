<template>
  <div class="wrapper" v-if="!hidePage">
    <div class="container">
      <div class="img-container">
        <img src="https://i.boatonland.com/report2023/title.svg" alt="" />
        <img src="https://i.boatonland.com/report2023/pointer.svg" class="pointer" alt="" />
      </div>
      <div class="img-container">
        <img :src="subtitles[subtitleIndex % 3]" alt="" />
        <img src="https://i.boatonland.com/report2023/arrow.svg" class="arrow" alt="" />
        <img src="https://i.boatonland.com/report2023/right.svg" class="right" alt="" />
      </div>
      <div class="btn-container">
        <div class="button" @click="start">查收我的报告</div>
        <div class="button button-1"></div>
        <div class="button button-2"></div>
      </div>
      <div class="user_agreement">
        点击即代表您同意<a href="https://terms.tripleuni.com/uni/privacy">《隐私政策》</a>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, defineProps } from "vue";

const props = defineProps({
  loaded: Boolean,
});

const hidePage = ref(false);
const subtitles = ["https://i.boatonland.com/report2023/subtitle_0.svg", "https://i.boatonland.com/report2023/subtitle_1.svg", "https://i.boatonland.com/report2023/subtitle_2.svg"];
const colors = ["#d85050", "#7b4bed", "#80c5d2"];
let subtitleIndex = ref(0);
onMounted(() => {
  const button = document.querySelector(".btn-container");
  setInterval(() => {
    subtitleIndex.value++;
    button.style.setProperty("--_clr-0", colors[subtitleIndex.value % 3]);
    button.style.setProperty("--_clr-1", colors[(subtitleIndex.value + 1) % 3]);
    button.style.setProperty("--_clr-2", colors[(subtitleIndex.value + 2) % 3]);
  }, 2000);
});

function animate() {
  const areas = document.querySelectorAll(".page1-1 .hide");
  let timer = 1000;
  areas.forEach((area) => {
    setTimeout(() => {
      area.classList.remove("hide");
    }, timer);
    timer += 1000;
  });
}

function start() {
  if (!props.loaded){
    console.log("not loaded");
    return;
  };
  const wrapper = document.querySelector(".wrapper");
  wrapper.style.transform = "translateY(-100vh)";
  //   wrapper.style.opacity = "0";
  animate();

  setTimeout(() => {
    hidePage.value = true;
  }, 1000);
}
</script>

<style scoped>
.img-container {
  width: 100%;
  height: 100%;
  position: relative;
}

.img-container > img,
.img-container > svg {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cotain;
  object-position: center;
}

.img-container > .pointer {
  transform: rotate(30deg);
  width: 35%;
  inset: unset;
  top: -30%;
  right: -5%;
  animation: 1s move ease-in-out infinite alternate both running;
}

.img-container > .arrow {
  width: 35%;
  inset: unset;
  top: 25%;
  right: 0;
  animation: 3s circle ease-in-out infinite running;
}

.img-container > .right {
  width: 28%;
  inset: unset;
  top: 38%;
  left: 6%;
  animation: 5s swing ease-in-out infinite running;
}

@keyframes move {
  0% {
    transform: translateX(-8%);
  }
  100% {
    transform: translateX(8%);
  }
}

@keyframes circle {
  0% {
    transform: translateX(0) translateY(-10px);
  }
  25% {
    transform: translateX(10px) translateY(0);
  }
  50% {
    transform: translateX(0) translateY(10px);
  }
  75% {
    transform: translateX(-10px) translateY(0);
  }
  100% {
    transform: translateX(0) translateY(-10px);
  }
}
@keyframes swing {
  0%,
  100% {
    transform: scale(1) translateX(-5px);
  }
  50% {
    transform: scale(1.1) rotate(-10deg);
  }
}

.user_agreement {
  text-align: center;
  color: var(--clr-text-muted);
  font-size: var(--fs-100);
  padding-top: 0.5rem;
}

.user_agreement a {
  text-decoration: underline;
}

.wrapper {
  --_top-padding: 5svh;
  position: fixed;
  top: calc((100svh - var(--page-height)) / 2 - var(--_top-padding));
  left: calc((100vw - var(--page-width)) / 2);
  height: calc(var(--page-height) + var(--_top-padding));
  width: var(--page-width);
  padding-top: var(--_top-padding);
  z-index: 1000;
  background: var(--clr-background);
  transition: transform 1s cubic-bezier(1, -0.24, 0.92, 0.7);
}

.container {
  height: 100%;
  width: 100%;
  padding: var(--page-padding);
  display: grid;
  grid-template-rows: 3fr 5fr 1.5fr 0.5fr;
  grid-template-columns: 1fr;
  gap: 2svh;
  justify-content: center;
}

.btn-container {
  --_clr-0: #d85050;
  --_clr-1: #80c5d2;
  --_clr-2: #7b4bed;
  padding: 3% 7%;
  position: relative;
}

.button {
  border-radius: 100rem;
  position: absolute;
  inset: 12% 10%;
  border: 2px black solid;
  display: grid;
  place-items: center;
  font-size: var(--fs-600);
  font-weight: 900;
  background-color: var(--_clr-0);
  transition: all 0.2s ease-in-out;
}

.button-1 {
  background-color: var(--_clr-1);
  transform: translate(-16px, 10px);
  z-index: -2;
}
.button-2 {
  background-color: var(--_clr-2);
  transform: translate(-8px, 5px);
  z-index: -1;
}

.btn-container:has(.button:hover) .button {
  filter: brightness(1.1);
  transform: scale(1.05);
  background: gray;
  cursor: pointer;
}
</style>
