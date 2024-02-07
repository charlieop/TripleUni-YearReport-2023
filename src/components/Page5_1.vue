<template>
  <div class="page page5_1" v-if="userInfo.no_view_follow_post">
    <div class="text-area hide">
      <p>还记得这条树洞吗?</p>
      <p>
        那是在
        <span class="b"
          >{{ parseInt(userInfo.no_view_follow_post.date.split("-")[1]) }}月
        </span>
        <span class="b"
          >{{ parseInt(userInfo.no_view_follow_post.date.split("-")[2]) }}日
        </span>
        那一天
      </p>
    </div>
    <div class="text-area hide">
      <p>你默默围观了它</p>
      <p>可之后再未回头查看</p>
    </div>
    <div class="text-area hide">
      <PostComponnet :postInfo="userInfo.no_view_follow_post.data" />
    </div>
    <div class="text-area hide">
      <p>在忙碌的日子里</p>
      <p>也不要忘了来收藏夹看看吧!</p>
    </div>
    <div class="emoji hide transition">🥺</div>
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
  const pages = document.querySelectorAll(".page5_1");
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
.page5_1 {
  background: linear-gradient(180deg, #57904d 5%, #008bcd 40%, #00b9ff 95%);
  background: linear-gradient(180deg, #57904d 5%, #efa717 95%);
}

.emoji {
  position: absolute;
  bottom: 20%;
  right: 20%;
  font-size: 8rem;
}

.emoji::after,
.emoji::before {
  position: absolute;
  font-size: 3rem;
  bottom: 4%;
}
.emoji::before {
  content: "👉";
  left: 12%;
  animation: pointyFinger 1s infinite alternate ease-in-out;
}
.emoji::after {
  content: "👈";
  right: 12%;
  animation: pointyFinger 1s infinite alternate-reverse ease-in-out;
}
@keyframes pointyFinger {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(0.5rem);
  }
}
</style>
