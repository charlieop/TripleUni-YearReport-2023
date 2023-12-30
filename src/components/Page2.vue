<template>
  <div class="pages-container">
    <div class="page">
      <div class="text-area" v-if="collage == 0">
        <p>2023年</p>
        <p>
          你在{{ collageInfo.nickname[collage] }}共度过了
          <span class="b">{{ userInfo.days_with_triple_uni }}</span> 天
        </p>
        <p>
          共计 <span class="b">{{ userInfo.usage_minutes }}</span> 分钟
        </p>
        <p>
          相当于平均每天在{{ collageInfo.nickname[collage] }}流连
          <span class="b">{{ Math.ceil(userInfo.usage_minutes / 365) }}</span>
          分钟
        </p>
      </div>

      <div class="text-area" v-else>
        <p>2023年</p>
        <p>
          你有
          <span class="b">{{ userInfo.usage_percentage }}%</span> 的时间流连于{{
            collageInfo.nickname[collage]
          }}
        </p>
        <p>每次打开{{ collageInfo.nickname[collage] }}</p>
        <p style="color: red">请策划编一些文案</p>
      </div>

      <div class="text-area">
        <p>
          在 <span class="b">xx</span> 名{{
            collageInfo.nickname[collage]
          }}用户中排名第 <span class="b">xx</span>
        </p>
        <p>
          相当于修了
          <span class="b">{{
            Math.ceil(userInfo.usage_minutes / 21.6) / 100
          }}</span>
          门{{ equivalentCourse.credit[collage] }}学分的
        </p>
        <p>
          <span class="b">{{ equivalentCourse.code[collage] }}</span>
        </p>
      </div>

      <div class="special">
        <div class="b special-text" v-html="shownText.text[rank].value"></div>
        <div class="keyword" v-for="word in shownText.keyword[rank]">
          {{ word }}
        </div>
      </div>
      <img class="book" src="@/assets/imgs/enchanted-book.svg" alt="">
    </div>
    <div class="page">b</div>
    <div class="page">c</div>
    <div class="page"><div class="next" @click="next">点我继续</div></div>
  </div>
</template>

<script setup>
import { toRef, computed } from "vue";

const props = defineProps({
  collageInfo: Object,
  collage: Number,
  userInfo: Object,
});

const collageInfoRef = toRef(props, "collageInfo");
const collageRef = toRef(props, "collage");
const userInfoRef = toRef(props, "userInfo");
const nickname = computed(
  () => collageInfoRef.value.nickname[collageRef.value]
);
const rank = computed(() => {
  const percentage = userInfoRef.value.usage_percentage;
  if (percentage >= 95) {
    return 0;
  } else if (percentage >= 80) {
    return 1;
  } else if (percentage >= 50) {
    return 2;
  } else {
    return 0;
  }
});

const shownText = {
  text: [
    computed(
      () =>
        "「好幸福！」<br>与你一块儿经历了这么多喜怒哀乐<br>今后，" +
        nickname.value +
        "一定继续用快乐报答你的陪伴！"
    ),
    computed(
      () =>
        "不知道<br>" +
        nickname.value +
        "成为你心灵的归宿了吗？<br>希望可以永远给你提供温暖与欢欣！"
    ),
    computed(
      () =>
        nickname.value + "说:<br>希望能永远成为你内心的港湾喔。<br>一起加油吧!"
    ),
    computed(
      () =>
        nickname.value + "对你说:<br>明年再多逛逛吧，别漏了那些有趣的角落哦!"
    ),
  ],
  keyword: [
    ["好幸福", "好幸福哦", "好幸福!!", "好幸福~"],
    ["是归宿", "是归宿哦", "要温暖!!", "要欢欣~"],
    ["是港湾", "是港湾吧", "一定是!!", "是港湾~"],
    ["多逛逛", "多逛逛哦", "多逛逛!!", "多逛逛~"],
  ],
};

const equivalentCourse = {
  credit: [6, 3, 3],
  code: ["PUPU 2001", "CUBB 1001", "DUST 1001"],
};

function next() {
  const container = document.querySelector(".content-wrapper");
  container.scrollBy({
    top: container.clientHeight,
    behavior: "smooth",
  });
}
</script>

<style scoped>
.next {
  width: 100%;
  text-align: center;
  padding: 20px 0;
  background: #fff;
  color: #000;
  font-size: 20px;
  font-weight: bold;
  cursor: pointer;
  border-radius: 10px;
}

.special {
  position: relative;
  font-size: var(--fs-500);
  text-align: center;
  margin-top: 10svh;
}

.special-text {
  text-decoration: underline;
  text-decoration-color: rgb(60, 136, 223);
}

.book {
    position: absolute;
    width: 35%;
    bottom: 10%;
    right: 10%;
    pointer-events: none;
    animation: book 2s ease-in-out alternate infinite;
}

@keyframes book {
    from {
        transform: translateY(0.5rem);
    }
    to {
        transform: translateY(-0.5rem);
    }
    
}

.keyword {
  --_clr: #000;
  position: absolute;
  z-index: 1;
  text-align: right;
  left: -100%;
  font-family: var(--ff-accent);
  color: white;
  text-shadow: 3px 0 var(--_clr), -3px 0 var(--_clr), 0 3px var(--_clr),
    0 -3px var(--_clr), 1.7px 1.7px var(--_clr), -1.7px -1.7px var(--_clr),
    1.7px -1.7px var(--_clr), -1.7px 1.7px var(--_clr);
  animation: move 6s linear infinite;
}

.keyword:nth-child(1 of .keyword) {
  --_clr: #d900ff;
  top: -3.5rem;
  font-size: var(--fs-600);
  animation-delay: 0s;
}
.keyword:nth-child(2 of .keyword) {
  --_clr: #000dff;
  top: -2rem;
  font-size: 1.2em;
  animation-delay: 2.5s;
}
.keyword:nth-child(3 of .keyword) {
  --_clr: #ff3600;
  bottom: -4.5rem;
  font-size: var(--fs-800);
  animation-delay: 1.5s;
}
.keyword:nth-child(4 of .keyword) {
  --_clr: #00ff00;
  bottom: -1.5rem;
  font-size: 1.4em;
  animation-delay: 3.5s;
}

@keyframes move {
  0% {
    right: -50%;
  }
  100% {
    right: 140%;
  }
}

.pages-container {
  background: linear-gradient(180deg, #90c3d0 15.82%, #a48ed7 96.54%);
}
</style>
