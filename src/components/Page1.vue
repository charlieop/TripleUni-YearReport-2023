<template>
  <div class="container" :style="disableScroll">
    <div class="page page1-1">
      <div class="text-area hide">
        <p>{{ collageInfo.creationTime[collage] }}</p>
        <p class="b">
          {{ collageInfo.name[collage] }}
        </p>
        <p>来到了这个世界</p>
      </div>
      <div class="text-area hide">
        <p>春疏 夏离</p>
        <p>秋遇 冬熟</p>
      </div>
      <div class="text-area hide">
        <p>
          不知不觉 <span class="b">{{ collageInfo.nickname[collage] }}</span>
        </p>
        <p>已经与你携手走至2023的尽头</p>
      </div>
      <div class="text-area hide">
        <p>现在翻开记忆</p>
        <p>
          看看这一年你在
          <span class="b">{{ collageInfo.nickname[collage] }}</span>
          留下的印记吧
        </p>
      </div>
      <div class="arrow animated text-area"></div>
    </div>
    <div class="page page1-2" v-if="collage == 0">
      <div class="text-area hide">
        <p>{{ userInfo.registration_date }}</p>
        <p>万有引力让我们在那天相遇</p>
        <p>直到今天,</p>
        <p>噗噗已经陪你度过了 {{ userInfo.days_with_triple_uni }} 个日升日落</p>
      </div>
      <div class="text-area hide">
        <p>在迄今为止已注册的xx名用户中</p>
        <p>你是噗噗遇到的第 {{ userInfo.registration_rank }} 个小伙伴</p>
      </div>
      <div class="text-area hide">
        <p>在初遇的这一天</p>
        <p>你点开的第一条噗噗是:</p>
      </div>
      <div class="text-area hide">
        <PostComponnet :postInfo="userInfo.first_meet_post" />
      </div>
    </div>
    <img
      class="earth"
      src="@/assets/imgs/earth.svg"
      alt=""
      @click="onClickAction"
    />
    <img class="bird" src="@/assets/imgs/bird.svg" alt="" />
    <img class="cloud group1" src="@/assets/imgs/cloud1.svg" alt="" />
    <img class="cloud group2" src="@/assets/imgs/cloud2.svg" alt="" />
    <div class="hint" @click="onClickAction">继续查看</div>
  </div>
</template>

<script setup>
import PostComponnet from "@/components/PostComponnet.vue";

import { computed, onMounted, toRef } from "vue";

const props = defineProps(["collage", "collageInfo", "userInfo"]);
const collageRef = toRef(props, "collage");

const disableScroll = computed(() => {
  return {
    "overflow-y": collageRef.value == 0 ? "scroll" : "clip",
  };
});

let onClickAction = undefined;

onMounted(() => {
  let page = document.querySelector(".page1-2");
  let observer = new IntersectionObserver(animate, {
    root: document.querySelector(".content-wrapper"),
    rootMargin: "0px 0px 0px 0px",
    threshold: 0.8,
  });
  observer.observe(page);
});

function animate(e) {
  if (!e[0].isIntersecting) return;
  const areas = document.querySelectorAll(".page1-2 .hide");
  setTimeout(stopAnimation, 500);
  let timer = 500;
  areas.forEach((area) => {
    setTimeout(() => {
      area.classList.remove("hide");
    }, timer);
    timer += 1000;
  });
}

function stopAnimation() {
  const cloud1 = document.querySelector(".group1");
  const cloud2 = document.querySelector(".group2");
  const earth = document.querySelector(".earth");
  const hint = document.querySelector(".hint");

  cloud1.style.animationPlayState = "paused";
  cloud2.style.animationPlayState = "paused";
  cloud1.style.opacity = "0";
  cloud2.style.opacity = "0";
  setTimeout(() => {
    cloud1.style.display = "none";
    cloud2.style.display = "none";
  }, 1000);
  hint.style.display = "block";
  //   earth.style.animationPlayState = "paused";
  //   earth.style.transform = "rotate(-90deg)";
  earth.addEventListener(
    "animationiteration",
    () => {
      earth.style.animation = "none";
      earth.classList.add("earth-reset");
      hint.style.opacity = "1";
      onClickAction = next;
    },
    { once: true }
  );
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
.arrow {
  display: block;
  margin: 0.2rem auto;
  height: 100px;
  width: 0;
}
.arrow:before,
.arrow:after {
  content: "";
  position: absolute;
  top: 0;
  right: -13.5px;
  width: 16px;
  height: 4px;
  border-radius: 4px;
  display: block;
  background: var(--clr-text-muted);
  transform: rotate(-30deg);
}
.arrow:after {
  right: inherit;
  left: -13.5px;
  transform: rotate(30deg);
}

.animated {
  animation: bounce 2s ease-in-out both infinite;
}
@keyframes bounce {
  0% {
    transform: translateY(0);
    opacity: 0;
  }
  10% {
    transform: translateY(0);
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  90% {
    transform: translateY(20px);
    opacity: 0;
  }
  100% {
    transform: translateY(20px);
    opacity: 0;
  }
}
.b {
  font-weight: 900;
}

.hide {
  opacity: 0;
  transform: translateY(50%);
}

.text-area {
  margin-top: 3svh;
  transition: all 1s ease-in-out;
}

.container {
  width: 100%;
  height: 100%;
  position: relative;
  background: linear-gradient(
    #ffffff 16%,
    #ebf4f6 33.58%,
    #badae2 69.1%,
    #90c3d0 96.5%
  );
  scroll-snap-type: y mandatory;
  overflow-y: scroll;
  overflow-x: clip;
}

.hint {
  position: sticky;
  z-index: 1;
  width: 100%;
  bottom: 17.5%;
  left: 0;
  color: white;
  text-align: center;
  font-size: var(--fs-600);
  opacity: 0;
  display: none;
  transition: all 0.5s ease-in-out;
}

.earth {
  position: sticky;
  transform: rotate(-90deg);
  width: 65%;
  bottom: 12%;
  left: 35%;
  animation: stupidRotate 7s linear infinite alternate;
  z-index: 1;
  transition: all 1s ease-in-out;
}

@keyframes stupidRotate {
  0% {
    transform: rotate(-90deg) rotate3d(1, 0, 0.2, 0deg);
  }
  100% {
    transform: rotate(270deg) rotate3d(1, 0, 0.2, 360deg);
  }
}

.earth-reset {
  animation: none;
  width: 70%;
  left: 15%;
  bottom: 2%;
  transition: all 0.5s ease-in-out;
  filter: blur(3px);
}
.earth-reset:hover,
.hint:hover ~ .earth-reset {
  transform: scale(1.1);
}

.cloud:hover,
.cloud:active {
  filter: blur(10px);
}

.bird {
  width: 40%;
  position: sticky;
  bottom: 25%;
  left: 0;
  animation: bird-move 15s linear infinite;
}

@keyframes bird-move {
  0% {
    transform: translateX(-100%);
  }
  12.5% {
    transform: translateX(-12.5%) scaleY(1.2) scaleX(0.9);
  }
  25% {
    transform: translateX(75%) scaleY(0.8) scaleX(1.1);
  }
  37.5% {
    transform: translateX(162.5%) scaleY(1.2) scaleX(0.9);
  }
  49.9% {
    transform: translateX(250%);
  }
  50% {
    transform: translateX(250%) rotate3D(0, 1, 0, 180deg);
  }
  62.5% {
    transform: translateX(162.5%) rotate3D(0, 1, 0, 180deg) rotate(90deg);
  }
  75% {
    transform: translateX(75%) rotate3D(0, 1, 0, 180deg) rotate(180deg);
  }
  87.5% {
    transform: translateX(-12.5%) rotate3D(0, 1, 0, 180deg) rotate(270deg);
  }
  100% {
    transform: translateX(-100%) rotate3D(0, 1, 0, 180deg) rotate(360deg);
  }
}

.cloud {
  position: sticky;
  width: 80%;
  transition: all 0.5s ease-in-out;
}

.group1 {
  width: 80%;
  bottom: 16%;
  left: -20%;
  z-index: 0;
  animation: cloud1 10s linear infinite both alternate;
}

@keyframes cloud1 {
  0% {
    transform: translateX(0%);
  }
  100% {
    transform: translateX(80%);
  }
}

.group2 {
  width: 80%;
  bottom: 9%;
  right: 10%;
  animation: cloud2 7s linear infinite both alternate;
  z-index: 2;
}

@keyframes cloud2 {
  0% {
    transform: translateX(-60%);
  }
  70% {
    transform: translateX(10%);
  }
  100% {
    transform: translateX(40%);
    filter: blur(2px);
  }
}

.page {
  width: var(--page-width);
  height: var(--page-height);
  padding: var(--page-padding);

  font-size: var(--fs-400);
  font-weight: 400;
  position: relative;

  z-index: 1;
  scroll-snap-align: start;
}
</style>
