<template>
  <div class="page page5_4">
    <div class="text-area hide">
      <p>根据你 2023年 的 Triple Uni 使用记录</p>
      <p>{{ collageInfo.nickname[collage] }}经过慎重考虑</p>
      <p>决定授予你荣誉称号:</p>
      <p class="b honor hide transition">{{ userInfo.title_1 }}</p>
    </div>
    <div class="text-area hide">
      <p>不仅如此</p>
      <p>你还达成了好多成就</p>
      <p>真是了不起啊!</p>
    </div>
    <div class="achievements hide transition">
      <div
        class="achievement ach-hide"
        :class="'tier-' + entry.importance"
        v-for="(entry, i) in userInfo.title_list"
      >
        <div class="trophy">
          <img
            :src="
              'https://i.boatonland.com/report2023/trophy-' +
              entry.importance +
              '.svg'
            "
            alt=""
          />
        </div>
        <div class="achievement-text">
          <p class="title b">{{ entry.title.replace(/(\r\n|\n|\r)/gm, "") }}</p>
          <p class="description">
            {{ entry.description.replace(/(\r\n|\n|\r)/gm, "") }}
          </p>
        </div>
      </div>
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
    threshold: 0,
  });
  const pages = document.querySelectorAll(".page5_4");
  pages.forEach((page) => {
    observer.observe(page);
  });
  const achiObserver = new IntersectionObserver(showAchievement, {
    rootMargin: "0px 0px -15% 0px",
    threshold: 0,
  });
  const achievements = document.querySelectorAll(".page5_4 .ach-hide");
  achievements.forEach((achi) => {
    achiObserver.observe(achi);
  });
});

function showAchievement(e) {
  e.forEach((entry) => {
    if (entry.isIntersecting) {
      setTimeout(() => {
        entry.target.classList.remove("ach-hide");
      }, 100);
    }
  });
}
function animate(e) {
  console.log(e);
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
.page5_4 {
  color: #054169;
  background: #add6f2;
  height: auto;
  padding-bottom: 50svh;
}

.honor {
  padding-block: 1.5rem;
  font-size: var(--fs-600);
  text-align: center;
  color: rgb(0, 0, 0);
  text-shadow: 0 0 1rem #ffe600;
}

.achievements {
  margin-top: 4rem;
  display: flex;
  flex-direction: column;
  gap: 2.5rem;
}
.achievement {
  --_color: #fcfcfc;
  padding: 1.5rem 1rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  transition: all 0.5s cubic-bezier(0.62, 1.32, 0.82, 1.39);
}

.tier-5 {
  --_color: #0792ee;
}
.tier-5 .trophy {
  box-shadow: 0 0 1.5rem var(--_color);
  border: 3px solid var(--_color);
}
.tier-4 {
  --_color: #fae420;
}
.tier-4 .trophy {
  box-shadow: 0 0 1rem var(--_color);
}
.tier-3 {
  --_color: #9327b8;
}
.tier-3 .trophy {
  box-shadow: 0 0 0.5rem var(--_color);
}
.tier-2 {
  --_color: #c57f06;
}
.tier-2 .trophy {
  border: 2px solid var(--_color);
}
.tier-1 {
  --_color: #8a8a8a;
}
.tier-1 .trophy {
  border: 1px solid var(--_color);
}

.ach-hide {
  opacity: 0;
  transform: translateX(100px);
  transition: all 0.5s;
}

.trophy {
  --_size: 5rem;
  width: var(--_size);
  height: var(--_size);
  margin: 0 20px 0 0;
  background-color: aliceblue;
  border-radius: 0.5rem;
  padding: 0.25rem;
}

.trophy img {
  width: 100%;
  height: 100%;
  object-fit: fill;
  object-position: center;
}

.achievement-text {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
.title {
  font-size: var(--fs-500);
  color: var(--_color);
}
</style>
