<template>
  <div class="pages-container page2">
    <div class="page page2-1">
      <div class="text-area hide">
        <p>2023年里</p>
        <!-- <div v-if="collage == 0"> -->
        <div v-if="true">
          <p>
            你与{{ collageInfo.nickname[collage] }}共度过了
            <span class="b">{{ userInfo.user_date_count }}</span> 天
          </p>
          <p>
            共计 <span class="b">{{ userInfo.user_minute_count }}</span> 分钟
          </p>
          <p>
            相当于平均每天在{{ collageInfo.nickname[collage] }}中流连
            <span class="b">{{
              Math.ceil(userInfo.user_minute_count / 365)
            }}</span>
            分钟
          </p>
        </div>
        <div v-else>
          <p>
            你有
            <span class="b">{{ Math.ceil(userInfo.user_date_count / 3.65) }}%</span>
            的日子流连于{{ collageInfo.nickname[collage] }}
          </p>
          <p>每一次访问</p>
          <p>{{ collageInfo.nickname[collage] }}都很高兴与你相见</p>
        </div>
      </div>

      <div class="text-area hide">
        <p>你的使用时间</p>
        <div v-if="userInfo.user_minute_count_rank">
          <p>
            在 <span class="b">{{ userInfo.total_user_count }}</span> 名{{
              collageInfo.nickname[collage]
            }}用户中
          </p>
          <p>
            排名第 <span class="b">{{ userInfo.user_minute_count_rank }} </span>
          </p>
        </div>
        <p v-else>
          超过了<span class="b">{{ userInfo.user_minute_count_percentage }}% </span>的{{ collageInfo.nickname[collage] }}用户
        </p>
      </div>
      <div class="text-area hide">
        <p>
          相当于修了
          <span class="b">{{
            Math.ceil(userInfo.user_minute_count / 21.6) / 100
          }}</span>
          门{{ equivalentCourse.credit[collage] }}学分的
        </p>
        <p>
          <span class="b">{{ equivalentCourse.code[collage] }}</span>
        </p>
      </div>

      <div class="special hide transition">
        <div class="b special-text" v-html="shownText.text[rank].value"></div>
        <div class="keyword" v-for="word in shownText.keyword[rank]">
          {{ word }}
        </div>
      </div>
      <img class="book hide transition" src="@/assets/imgs/enchanted-book.svg" alt="" />
    </div>
    <div class="page page2-2">
      <div class="text-area hide">
        <p>一天的24小时里</p>
        <p>
          你最常在
          <span class="b">{{ userInfo.user_most_common_time_period }}</span>
          {{ visitText[Math.floor(Math.random() * visitText.length)]
          }}{{ collageInfo.nickname[collage] }}
        </p>
      </div>
      <div class="text-area hide">
        <p>在过去的365天里</p>
        <template v-if="userInfo.user_stayup_percentage >= 60">
          <p>
            你居然有
            <span class="b">{{ userInfo.user_stayup_percentage }}%</span>
            的日子在熬夜看{{ collageInfo.nickname[collage] }}!
          </p>
          <p>{{ collageInfo.nickname[collage] }}感觉自己深深被爱着❤️</p>
        </template>
        <template v-else-if="userInfo.user_stayup_percentage >= 20">
          <p>
            你在
            <span class="b">{{ userInfo.user_stayup_percentage }}%</span>
            的天数里熬夜刷着{{ collageInfo.nickname[collage] }}
          </p>
          <p>能被如此对待, {{ collageInfo.nickname[collage] }}感觉很荣幸</p>
        </template>
        <template v-else>
          <p>
            你只在
            <span class="b">{{ userInfo.user_stayup_percentage }}%</span>
            的深夜打开了{{ collageInfo.nickname[collage] }}
          </p>
          <p>是拥有着传说中的《健康作息》吗?!</p>
        </template>
      </div>
      <div class="text-area hide">
        <p><span :style="'color:' + my_distribution_color">你</span>和<span :style="'color:' + average_distribution_color">大家</span>的使用时间分布是:</p>
        <Line class="lineChart" :options="options" :data="data_average"/>
      </div>
      <!-- <div class="text-area hide">
        <p>你的使用时间分布是:</p>
        <Line class="lineChart" :options="options" :data="data_user"/>
      </div> -->
    </div>
    <div class="page page2-3">
      <div class="text-area" v-if="userInfo.earliest_post">
        <div class="text-area hide">
          <p class="b header">
            <span>
              {{ parseInt(userInfo.earliest_post.date.split("-")[1]) }}月{{
                parseInt(userInfo.earliest_post.date.split("-")[2])
              }}日
            </span>
            <span>{{ userInfo.earliest_post.time }}</span>
          </p>
          <p>你比{{ collageInfo.nickname[collage] }}起的还要早</p>
          <p>你点开的第一条树洞是:</p>
        </div>
        <div class="text-area hide">
          <PostComponnet :postInfo="userInfo.earliest_post.data" />
        </div>
      </div>
      <div v-else class="text-area hide special">
        <p>你似乎从来没有在早上打开过{{ collageInfo.nickname[collage] }}</p>
        <p>想必你的睡眠质量一定很好吧</p>
      </div>
      <div v-if="userInfo.latest_post">
        <div class="text-area hide">
          <p class="b header">
            <span>
              {{ parseInt(userInfo.latest_post.date.split("-")[1]) }}月{{
                parseInt(userInfo.latest_post.date.split("-")[2])
              }}日
            </span>
            <span>{{ userInfo.latest_post.time }}</span>
          </p>
          <p>还没睡的{{ collageInfo.nickname[collage] }}</p>
          <p>与你一同点开了</p>
        </div>
        <div class="text-area hide">
          <PostComponnet :postInfo="userInfo.latest_post.data" />
        </div>
        <div class="text-area special b hide">
          <p>一眨眼天快亮了&nbsp;&nbsp;&nbsp;</p>
          <p>早知道不眨眼了...</p>
        </div>
      </div>
      <div class="text-area special hide" v-else>
        <p>你从来没有在深夜打开过{{ collageInfo.nickname[collage] }}</p>
        <p>真的是一个大学牲吗?</p>
        <p>实在是太不可思议了</p>
      </div>
    </div>
    <!-- <div class="page page2-4">
      <div class="text-area hide">
        <p class="header b">
          <span>
            {{ parseInt(userInfo.user_view_longest_date.split("-")[1]) }}月{{
              parseInt(userInfo.user_view_longest_date.split("-")[2])
            }}日
          </span>
        </p>
        <p>
          那天居然有
          <span class="b">{{ userInfo.user_view_longest_minute_count }}</span>
          分钟在浏览{{ collageInfo.nickname[collage] }}
        </p>
      </div>
      <div class="text-area hide">
        <p>这一天是你这一年中</p>
        <p>
          陪伴{{ collageInfo.nickname[collage] }}时间
          <span class="b special">最长</span> 的一天
        </p>
      </div>
      <div class="text-area hide">
        <p>陪完了{{ collageInfo.nickname[collage] }}</p>
        <p>也不要忘了家人与朋友们哦</p>
      </div>
      <div class="emoji hide transition">🥹</div>
      <div class="emoji hide transition">🥹</div>
      <div class="emoji hide transition">🥹</div>

      <div class="next hide" @click="next">继续查看</div>
    </div> -->
  </div>
</template>

<script setup>
import PostComponnet from "@/components/PostComponnet.vue";

import { toRef, computed, onMounted } from "vue";
import { Line } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, LinearScale, LineElement, PointElement, CategoryScale } from 'chart.js'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
);

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
  const percentage = userInfoRef.value.user_minute_count_percentage;
  if (percentage >= 95) {
    return 0;
  } else if (percentage >= 80) {
    return 1;
  } else if (percentage >= 50) {
    return 2;
  } else {
    return 3;
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

const visitText = ["光顾", "访问", "浏览", "宠幸", "打开", "来到"];

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

const my_distribution_color = '#2f78ed'
const average_distribution_color = '#a473d1'

const options = {
    responsive: true,
    backgroundColor: '#D8D8D8',
    elements:{
      point:{
        radius: 0,
      }
    },
    legend: {
      display: false
    },
    plugins: {
      legend: {
        display: false
      },
      title: {
        display: false,
      },
      customCanvasBackgroundColor: {
        color: 'lightGreen',
      }
    },
    layout: {
      padding:{
        bottom: 7,
      }
    },
    scales: {
      y: {
        display: false
      }
    },
    aspectRatio: 1.2,
  };

const data_average = {
  labels: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23'],
  datasets: [
  {
      label: '你的时间分布',
      data: userInfoRef.value.user_hour_distribution,
      borderColor: my_distribution_color,
    },
    {
      label: '平均时间分布',
      data: userInfoRef.value.total_hour_distribution,
      borderColor: average_distribution_color,
    },
    
  ]
};

const data_user = {
  labels: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23'],
  datasets: [
    {
      label: '你的时间分布',
      data: userInfoRef.value.user_hour_distribution,
      borderColor: '#2f78ed',
      backgroundColor: 'rgba(153,153,153, 0.5)',
    },
  ]
};

onMounted(() => {
  const observer = new IntersectionObserver(animate, {
    root: document.querySelector(".content-wrapper"),
    rootMargin: "0px 0px 0px 0px",
    threshold: 0.8,
  });
  const pages = document.querySelectorAll(".page2 .page");
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
.emoji {
  position: absolute;
  font-size: 5em;
}

.emoji::after {
  content: "🫶";
  position: absolute;
  font-size: var(--fs-900);
}

.emoji:nth-child(1 of .emoji) {
  top: 40%;
  left: 15%;
}

.emoji:nth-child(1 of .emoji)::after {
  bottom: 20%;
  left: 50%;
  animation: hand1 1.5s linear infinite alternate-reverse;
}

.emoji:nth-child(2 of .emoji) {
  top: 40%;
  right: 15%;
}

.emoji:nth-child(2 of .emoji)::after {
  bottom: 20%;
  left: 50%;
  animation: hand1 1.5s linear infinite alternate;
}

.emoji:nth-child(3 of .emoji) {
  top: 57%;
  right: 50%;
  transform: translateX(50%);
}

.emoji:nth-child(3 of .emoji)::after {
  bottom: -3%;
  left: 50%;
  animation: hand2 1.5s linear infinite;
}

@keyframes hand1 {
  0% {
    transform: translateX(-50%) rotate(60deg) translate(0, 2.75rem) rotate(-60deg);
  }

  100% {
    transform: translateX(-50%) rotate(-60deg) translate(0, 2.75rem) rotate(60deg);
  }
}

@keyframes hand2 {
  0% {
    transform: rotate(0deg) translate(0, 0.5rem) rotate(-0deg);
  }

  100% {
    transform: rotate(360deg) translate(0, 0.5rem) rotate(-360deg);
  }
}

.header {
  font-size: var(--fs-500);
  margin-top: 0.5rem;
  display: flex;
  gap: 1rem;
}

.special {
  position: relative;
  font-size: var(--fs-500);
  text-align: center;
  margin-top: 2.5rem;
}

.special-text {
  text-decoration: underline;
  text-decoration-color: rgb(60, 136, 223);
}

.book {
  position: absolute;
  width: 35%;
  bottom: 5%;
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

.lineChart{
  margin-top: 20px;
}

.pages-container {
  background: linear-gradient(180deg, #90c3d0 15.82%, #a48ed7 96.54%);
}
</style>
