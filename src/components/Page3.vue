<template>
  <div class="pages-container page3">
    <div class="page page3-1">3.1 -> 合并到4.1</div>
    <div class="page page3-2">3.2</div>
    <div class="page page3-3">
      <div class="text-area" v-if="userInfo.max_follow_post">
        <div class="text-area hide">
          <p>在这一年中</p>
          <p>
            <span class="b">#{{ userInfo.max_follow_post.data.post_id }}</span>
            这条树洞引起了大家的关注
          </p>
          <p>
            共有
            <span class="b">{{
              userInfo.max_follow_post.data.post_follower_num
            }}</span>
            人围观
          </p>
          <p>
            你是第 <span class="b">{{ userInfo.max_follow_post.rank }}</span> 个
          </p>
        </div>
        <div class="text-area hide">
          <PostComponnet :postInfo="userInfo.max_follow_post.data" />
        </div>
      </div>
      <div v-else class="text-area hide special">
        <p>你肯定是一个不喜欢喧嚣的人</p>
        <p>在这一年中</p>
        <p>
          没有围观过任何{{ collageInfo.nickname[collage] }}大家都在关注的树洞
        </p>
      </div>
      <div class="text-area" v-if="userInfo.min_follow_post">
        <div class="text-area hide">
          <p>
            <span v-if="!userInfo.max_follow_post">但</span>你<span
              v-if="userInfo.max_follow_post"
              >还</span
            >关注到了
          </p>
          <p>其他人或许不那么留意的树洞</p>
          <p>
            <span class="b">#{{ userInfo.min_follow_post.data.post_id }}</span>
            只有你一人围观
          </p>
        </div>
        <div class="text-area hide">
          <PostComponnet :postInfo="userInfo.min_follow_post.data" />
        </div>
        <div class="text-area b center hide">
          <p>这就是眼光毒辣的收藏夹吗?!</p>
        </div>
      </div>
      <div class="text-area special hide" v-else>
        <div v-if="!userInfo.max_follow_post">
          <p>你真是一个神秘的人</p>
          <p>也没有独自关注过任何树洞</p>
          <p>说真的, 你知道围观这个功能的</p>
          <p>对吧?</p>
        </div>
        <div v-else>
          <p>你没有独自关注过任何树洞</p>
          <p>真可惜没有小众的树洞进入你的法眼</p>
        </div>
      </div>
      <img class="chest" src="" alt="" />
      <div class="next" @click="next">继续查看</div>
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
  const pages = document.querySelectorAll(".page3 .page");
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
  background: linear-gradient(
    179.69deg,
    #a48ed7 15.82%,
    #ca90ae 48.61%,
    #e49292 96.54%
  );
}
</style>
