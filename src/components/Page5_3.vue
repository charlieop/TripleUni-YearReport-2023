<template>
  <div class="page page5_3">
    <p class="title hide transition">喜报!</p>
    <p class="subtitle hide transition">
      2023年7月28日<br />{{ collageInfo.nickname[collage] }}网页版上线了!
    </p>
    <template
      v-if="
        userInfo.user_minute_count_web > 0 &&
        userInfo.user_minute_count_wechat > 0
      "
    >
      <div class="text-area hide">
        <p>
          你在
          <span class="b"
            >{{ parseInt(userInfo.user_first_web_date.split("-")[0]) }}年{{
              parseInt(userInfo.user_first_web_date.split("-")[1])
            }}月{{
              parseInt(userInfo.user_first_web_date.split("-")[2])
            }}日</span
          >
        </p>
        <p>第一次登陆网页端</p>
        <p>有了更新奇便利的体验</p>
      </div>
      <div class="text-area hide">
        <p>Web与小程序相比</p>
        <p>
          你似乎更喜欢使用<span class="b"
            >{{
              userInfo.user_minute_count_web >=
              userInfo.user_minute_count_wechat
                ? "网页"
                : "小程序"
            }}端</span
          >
        </p>
        <p>
          小程序使用了
          <span class="b">{{ userInfo.user_minute_count_wechat }}</span> 分钟
        </p>
        <p>
          而网页端则使用了
          <span class="b">{{ userInfo.user_minute_count_web }}</span> 分钟
        </p>
      </div>
      <div
        class="text-area hide"
        v-if="
          userInfo.user_minute_count_web < userInfo.user_minute_count_wechat
        "
      >
        <p>2024年多尝试尝试网页端吧!</p>
      </div>
    </template>
    <template v-else-if="userInfo.user_minute_count_web > 0">
      <div class="text-area hide">
        <p>
          你在
          <span class="b"
            >{{ parseInt(userInfo.user_first_web_date.split("-")[0]) }}年{{
              parseInt(userInfo.user_first_web_date.split("-")[1])
            }}月{{
              parseInt(userInfo.user_first_web_date.split("-")[2])
            }}日</span
          >
        </p>
        <p>第一次来到了{{ collageInfo.nickname[collage] }}</p>
      </div>
      <div class="text-area hide">
        <p>{{ collageInfo.name[collage] }}也向所有用户开放了小程序版本</p>
        <p>
          微信搜索
          <span class="b">Triple Uni</span>
          即可直达!
        </p>
      </div>
    </template>
    <template v-else>
      <div class="text-area hide">
        <p>今年你只用了小程序呢</p>
        <p>也去试试网页版吧!</p>
        <p>真的很方便的哦!</p>
      </div>
      <div class="text-area hide">
        <p>网址是:</p>
        <p>https://tripleuni.com/</p>
      </div>
    </template>
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

function next() {
  const container = document.querySelector(".content-wrapper");
  container.scrollBy({
    top: container.clientHeight,
    behavior: "smooth",
  });
}

onMounted(() => {
  const observer = new IntersectionObserver(animate, {
    root: document.querySelector(".content-wrapper"),
    rootMargin: "0px 0px 0px 0px",
    threshold: 0.8,
  });
  const pages = document.querySelectorAll(".page5_3");
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
</script>

<style scoped>
.page5_3 {
  background: linear-gradient(180deg, #ffffff 0%, #00b9ff 100%);
}

.title {
  background-image: linear-gradient(
    to left,
    red,
    rgb(235, 211, 0),
    orange,
    red
  );
  -webkit-background-clip: text;
  background-clip: text;
  background-size: 200% auto;
  color: transparent;
  animation: moveBackground 3s linear infinite;
  font-size: 5rem;
  font-weight: 900;
  text-align: center;
}
@keyframes moveBackground {
  0% {
    background-position: 0% 50%;
  }
  100% {
    background-position: 200% 50%;
  }
}

.subtitle {
  font-size: var(--fs-500);
  font-weight: 900;
  text-align: center;
  margin-bottom: 2rem;
}
</style>
