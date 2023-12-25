<template>
  <div class="container">
    <div class="page">
      <div class="text-area">
        <p>{{ creationTime }}</p>
        <p class="b">
          {{ name }}
        </p>
        <p>来到了这个世界</p>
      </div>
      <div class="text-area">
        <p>春疏 夏离</p>
        <p>秋遇 冬熟</p>
      </div>
      <div class="text-area">
        <p>
          不知不觉 <span class="b">{{ nickname }}</span>
        </p>
        <p>已经与你携手走至2023的尽头</p>
      </div>
      <div class="text-area">
        <p>现在翻开记忆</p>
        <p>
          看看这一年你在 <span class="b">{{ nickname }}</span> 留下的印记吧
        </p>
      </div>
    </div>
    <div class="page" v-if="name == '噗噗'">
      <div class="text-area">
        <p>2023年 1月 1日</p>
        <p>万有引力让我们在那天相遇</p>
        <p>直到今天,</p>
        <p>噗噗已经陪你度过了xx个日升日落</p>
      </div>
      <div class="text-area">
        <p>在迄今为止已注册的xx名用户中</p>
        <p>你是噗噗遇到的第xx个小伙伴</p>
      </div>
      <div class="text-area">
        <p>在初遇的这一天</p>
        <p>你点开的第一条噗噗是:</p>
      </div>
    </div>
    <img class="earth" src="@/assets/imgs/earth.svg" alt="" @click="next"/>
    <img class="cloud group1" src="@/assets/imgs/cloud1.svg" alt="" />
    <img class="cloud group2" src="@/assets/imgs/cloud2.svg" alt="" />
  </div>
</template>

<script setup>
defineProps(["name", "nickname", "creationTime"]);

function next() {
  const container = document.querySelector(".content-wrapper");
  container.scrollBy({
    top: container.clientHeight,
    behavior: "smooth",
  });
}
</script>

<style scoped>
.b {
  font-weight: 900;
}

.text-area {
  margin-top: 3svh;
}
.container {
  width: var(--page-width);
  height: calc(var(--page-height));
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

.earth {
  position: sticky;
  transform: rotate(-90deg);
  width: 65%;
  bottom: 12%;
  left: 35%;
  animation: stupidRotate 15s linear infinite both alternate;
  z-index: 1;
  transition: all 0.5s ease-in-out;
}

.cloud:hover,
.cloud:active {
  filter: blur(10px);
}

@keyframes stupidRotate {
  0% {
    transform: rotate(-90deg) rotate3d(1, 0, 0.2, 0deg);
  }
  100% {
    transform: rotate(270deg) rotate3d(1, 0, 0.2, 360deg);
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
  100% {
    transform: translateX(40%);
  }
}

.page {
  width: var(--page-width);
  height: var(--page-height);
  padding: var(--page-padding);

  font-size: var(--fs-400);
  font-weight: 400;
  position: relative;

  outline: 1px solid black;
  z-index: 1;
  scroll-snap-align: start;
}
</style>
