<template>
  <div class="pages-container page3">
    <div class="page page3-2">
      <div class="hide text-area">
        <p>你浏览{{ collageInfo.nickname[collage] }}时遇到最多的词语分别是:</p>
      </div>
      <div class="sticker-area">
        <div
          class="sticker hide"
          :class="'sticker' + i"
          v-for="i in Array(
            Math.min(userInfo.user_most_view_word_list.length, 5)
          ).keys()"
        >
          <img :src="getStickerPath(i)" alt="" class="sticker-image" />
          <p class="sticker-text">{{ userInfo.user_most_view_word_list[i] }}</p>
        </div>
      </div>
      <div class="hide text-area">
        <p>在所有的话题里</p>
        <p>
          你最感兴趣的是
          <span class="b">{{ userInfo.user_most_view_topic }}</span>
          话题
        </p>
      </div>
      <div class="hide text-area">
        <p>{{ topicText }}</p>
      </div>
      <img class="icon hide transition" :src="topicImg" alt="" />
    </div>
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
            人围观, 你是第
            <span class="b">{{ userInfo.max_follow_post.rank }}</span> 个
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
          <p>这就是眼光毒辣的收藏家吗?!</p>
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
      <div class="next hide" @click="next">继续查看</div>
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

var topicText = computed(() => {
  switch (userInfoRef.value.user_most_view_topic) {
    case "分享":
      return "大家的经验有帮到你走向更广阔的世界吗？";
    case "情感":
      return "这个话题是否给你带来许多起伏的情绪呢？";
    case "随写":
      return "这些文字会带给你怎样的回忆和情绪呢？";
    case "学业":
      return "努力学习之余也不要忘记常休息喔～";
    case "求职":
      return "大家都有光明的未来٩( ö̆ ) و";
    case "美食":
      return "孤独的美食家终于不再孤单啦！";
    case "跳蚤":
      return "心仪的宝贝最终被带回家了吗？";
    case "SingCon":
      return "喜欢音乐的你找到那个同频的ta了吗？";
    case "UNI":
      return "三校的树洞都逃不过你的眼睛";
    case "投票":
      return "你真的很爱投票耶~";
    case "毕业":
      return "终于要离开校园了吗?";
    default:
      return "树洞永远欢迎你的光顾~~~";
  }
});

let rootPath = new URL(window.location).pathname;
var topicImg = computed(() => {
  switch (userInfoRef.value.user_most_view_topic) {
    case "分享":
    case "学业":
    case "求职":
    case "UNI":
      return rootPath + "imgs/enchanted-book.svg";
    case "情感":
    case "随写":
    case "树洞":
      return rootPath + "imgs/heart.svg";
    case "SingCon":
    case "毕业":
    case "投票":
      return rootPath + "imgs/notes.svg";
    case "跳蚤":
    case "美食":
    default:
      return rootPath + "imgs/chicken.svg";
  }
});

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

function getStickerPath(i) {
  return rootPath + "imgs/sticker" + i + ".png";
}

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

.sticker-area {
  margin-block: 1rem 6rem;
  position: relative;
  height: 20rem;
}

.sticker {
  position: absolute;
  width: 8.5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  transition: all 1s ease-in-out;
}
.sticker0 {
  top: 30%;
  left: 35%;
  transform: scale(1.3);
}
.sticker1 {
  top: 1rem;
  left: 1rem;
  transform: scale(1.15) rotate(-15deg);
}
.sticker2 {
  right: 2rem;
  bottom: -1rem;
  transform: scale(1.15) rotate(-7deg);
}
.sticker3 {
  top: 7%;
  right: 3%;
}
.sticker3 .sticker-image {
  transform: rotateY(180deg);
}
.sticker4 {
  bottom: 10%;
  left: 10%;
  transform: scale(0.95) rotate(10deg);
}

.sticker-image {
  object-fit: cover;
  object-position: center;
  width: 100%;
}

.sticker-text {
  position: absolute;
  top: 39%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: var(--fs-500);
  color: white;
  text-shadow: 0px 0px 5px rgba(0, 0, 0, 0.5);
}

.icon {
  position: absolute;
  width: 30%;
  bottom: 25%;
  right: 10%;
  pointer-events: none;
  animation: icon 2s ease-in-out alternate infinite;
}

@keyframes icon {
  from {
    transform: translateY(0.5rem);
  }
  to {
    transform: translateY(-0.5rem);
  }
}
</style>
