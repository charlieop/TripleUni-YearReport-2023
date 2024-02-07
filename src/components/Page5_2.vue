<template>
  <div class="page page5_2">
    <div class="header">
      <img src="https://i.boatonland.com/report2023/tech-pattern.svg" alt="" class="tech-pattern" />

      <div class="header-text">
        <p class="header-text-1">{{ collageInfo.nickname[collage] }}</p>
        <p class="header-text-2">热搜</p>
      </div>
    </div>
    <div class="nav">
      <ul>
        <li class="b">我的</li>
        <li>年度报告</li>
        <li>香港</li>
        <li>新年</li>
        <li>发财</li>
        <li>更多</li>
      </ul>
    </div>
    <div class="searchbar">
      <p>
        2023年你在{{ collageInfo.nickname[collage] }}一共进行了
        <span class="b">{{ userInfo.user_search_count }}</span> 次搜索!
      </p>
    </div>
    <div class="entries">
      <div class="entry">
        <span class="index" style="color: rgb(118, 118, 118)">-</span>
        <span class="keyword">你搜索最多的词条</span>
        <span class="total-search-count">总搜索数</span>
        <span class="label">&nbsp;公告&nbsp;</span>
      </div>
      <div
        class="entry"
        v-for="i in Array(userInfo.user_search_list.length).keys()"
      >
        <span class="index" :class="i <= 2 ? 'first3' : ''">{{ i + 1 }}</span>
        <span class="keyword">{{
          userInfo.user_search_list[i].keyword.slice(0, 15)
        }}</span>
        <span class="total-search-count">{{
          userInfo.user_search_list[i].other_user_count
        }}</span>
        <span
          class="label"
          :style="
            '--_color:#' +
            (userInfo.user_search_list[i].search_time_count >= 5
              ? 'f62944'
              : 'f6ae29')
          "
          >{{
            userInfo.user_search_list[i].search_time_count >= 5 ? "爆" : "热"
          }}</span
        >
      </div>
      <div class="entry">
        <span class="index" style="color: rgb(118, 118, 118)">{{
          userInfo.user_search_list.length + 1
        }}</span>
        <span class="keyword"
          >{{ collageInfo.nickname[collage] }}上线网页版啦!</span
        >
        <span class="total-search-count">8547</span>
        <span class="label">&nbsp;公告&nbsp;</span>
      </div>
      <div class="entry">
        <span class="index" style="color: rgb(118, 118, 118)">{{
          userInfo.user_search_list.length + 2
        }}</span>
        <span class="keyword">你体验过AI搜索了吗?</span>
        <span class="total-search-count">927</span>
        <span class="label">&nbsp;公告&nbsp;</span>
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
  const pages = document.querySelectorAll(".page5_2");
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
.page5_2 {
  background: white;
  padding: unset;
}

.header {
  position: relative;
  height: 25%;
  background: linear-gradient(180deg, #efa717 0%, #f8690a 100%);
  overflow: clip;
}

.tech-pattern {
  object-fit: cover;
  object-position: center;
  width: 100%;
  opacity: 0.15;
  filter: invert(85%);
  transform: translateY(-3rem) scale(1);
}

.header-text {
  position: absolute;
  bottom: 20%;
  left: 50%;
  transform: translate(-50%, 0%);
  color: white;
  text-align: center;
  font-size: 3.25rem;
  font-weight: 900;
  display: flex;
  gap: 0.25rem;
  font-style: italic;
  letter-spacing: 0.0125ch;
  text-shadow: 0 0 0.5rem rgb(170, 134, 17);
}

.header-text-1 {
  transform: translateY(-0.5rem);
}
.header-text-2 {
  transform: translateY(0.5rem);
}

.nav ul {
  display: flex;
  justify-content: space-around;
  padding-block: 0.75rem 0.5rem;
  padding-inline: 1rem 0.125rem;
  font-size: 1.25rem;
  color: #757575;
}
li.b {
  color: black;
  position: relative;
}
li.b::after {
  content: "";
  position: absolute;
  bottom: -5px;
  left: 10%;
  width: 80%;
  height: 0.25rem;
  border-radius: 100rem;
  background: #efa717;
}

.searchbar {
  margin-top: 0.25rem;
  padding: 0.35rem 1rem;
  font-size: 1.25rem;
  color: #606060;
  background-color: #ebebeb;
  font-size: var(--fs-200);
}

.entries {
  --_gap: 1.25px;
  display: grid;
  gap: var(--_gap);
  overflow-y: scroll;
}
.entry {
  position: relative;
  padding: 0.75rem 2rem;
  font-size: var(--fs-400);
}
.entry::after {
  content: "";
  position: absolute;
  bottom: calc(-1 * var(--_gap));
  left: 2%;
  width: 96%;
  height: var(--_gap);
  background-color: #e9e9e9;
}

.index {
  padding-inline: 0.75rem 1.25rem;
  color: #efa717;
}
.index.first3 {
  color: rgb(240, 56, 6);
  font-weight: 900;
  font-style: italic;
}

.keyword {
  padding-inline: 0.75rem 0.5rem;
}

.total-search-count {
  color: #8e8e8e;
  font-size: var(--fs-100);
}

.label {
  --_color: #29a7f6;
  --_size: 1.3rem;
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  top: 50%;
  right: 2rem;
  min-height: var(--_size);
  min-width: var(--_size);
  transform: translateY(-50%);
  border-radius: 5px;
  background-color: var(--_color);
  color: white;
  font-size: var(--fs-100);
  font-weight: 100;
}
</style>
