<template>
  <div class="pages-container page4-1">
    <div class="group">
      <p>2023年</p>
      <p>
        TripleUni 一共有
        <span class="b">{{ userInfo.total_post_count }}</span> 条树洞
      </p>
      <p>
        你光顾了其中的
        <span class="b">{{ userInfo.user_view_post_count }}</span> 条
      </p>
      <p v-if="userInfo.user_view_post_count_rank">
        是全{{ collageInfo.nickname[collage] }}的第
        <span class="b">{{ userInfo.user_view_post_count_rank }}</span> 名
      </p>
      <p v-else>
        击败了
        <span class="b">{{ userInfo.user_view_post_count_percentage }}%</span>
        的{{ collageInfo.nickname[collage] }}用户!
      </p>
      <p class="lyric">「我看着你，你看着我」</p>
    </div>
    <div class="group">
      <p>
        <span class="b"
          >{{ parseInt(userInfo.user_view_longest_date.split("-")[1]) }}月</span
        >
        <span class="b"
          >{{ parseInt(userInfo.user_view_longest_date.split("-")[2]) }}日</span
        >
      </p>
      <p>
        那天居然有<span class="b">{{
          userInfo.user_view_longest_minute_count
        }}</span
        >分钟在浏览{{ collageInfo.nickname[collage] }}
      </p>
      <p>这一天是你2023年陪伴{{ collageInfo.nickname[collage] }}最长的一天</p>
      <p class="lyric">「🫶🏻」</p>
    </div>
    <div class="group">
      <p>
        去年你一共发送了<span class="b">{{ userInfo.user_post_count }}</span
        >条树洞
      </p>
      <template v-if="userInfo.user_post_count_percentage > 40">
        <p>
          秒杀了<span class="b">{{ userInfo.user_post_count_percentage }}%</span
          >的用户
        </p>
        <p>今年也要再接再厉哦!</p>
      </template>
      <template v-else>
        <p>
          被<span class="b"
            >{{
              parseInt((100 - userInfo.user_post_count_percentage) * 100) / 100
            }}%</span
          >
          的用户超过了
        </p>
        <p>今年请多发发树洞吧!</p>
      </template>
      <p class="lyric">「你分享的点点滴滴，定能给别人送去暖意」</p>
    </div>
    <div class="group">
      <p class="lyric">轨迹改变</p>
      <p class="lyric">角落交错</p>
      <p>非同一般的你</p>
      <p>参与了{{ userInfo.user_comment_count }}次树洞讨论</p>
      <p>在{{ userInfo.user_vote_count }}个投票中发表了见解</p>
      <p>高调围观了{{ userInfo.user_follow_count }}个树洞</p>
      <p>还向其他用户发送了{{ userInfo.user_pm_count }}条私信</p>
      <p class="lyric">「十面埋伏, 愿你我不再错过」</p>
    </div>
  </div>
  <div class="pages-container page4-2 page4">
    <div
      class="specialPosts"
      v-if="
        userInfo.most_view_post ||
        userInfo.most_comment_post ||
        userInfo.most_follow_post
      "
    >
      <h2 class="hide transition">你发布的树洞中<br />那些「顶流」般的存在</h2>
      <div class="horizontal_scroller">
        <div class="card hide transition" v-if="userInfo.most_view_post">
          <p>
            <span class="b">阅读量</span> 最高达到了
            <span class="b">{{ userInfo.most_view_post.data.post_view }}</span>
            次
          </p>
          <PostComponnet
            :postInfo="userInfo.most_view_post.data"
          ></PostComponnet>
        </div>
        <div class="card hide transition" v-if="userInfo.most_comment_post">
          <p>
            <span class="b">评论数</span> 最高达到了
            <span class="b">{{
              userInfo.most_comment_post.data.post_comment_num
            }}</span>
            条
          </p>
          <PostComponnet
            :postInfo="userInfo.most_comment_post.data"
          ></PostComponnet>
        </div>
        <div class="card hide transition" v-if="userInfo.most_follow_post">
          <p>
            <span class="b">围观数</span> 最高达到了
            <span class="b">{{
              userInfo.most_follow_post.data.post_follower_num
            }}</span>
            个
          </p>
          <PostComponnet
            :postInfo="userInfo.most_follow_post.data"
          ></PostComponnet>
        </div>
      </div>
    </div>
    <div
      class="keyWords hide text-area"
      v-if="userInfo.user_most_post_word_list"
    >
      <p>你发布的帖子中</p>
      <p>频频出现:</p>
      <div>
        <span v-for="word in userInfo.user_most_post_word_list.slice(0, 3)">
          {{ word }}</span
        >
      </div>
      <p>它们是否是你许多纷飞思绪的源头呢？</p>
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

onMounted(() => {
  const observer = new IntersectionObserver(animate, {
    root: document.querySelector(".content-wrapper"),
    rootMargin: "0px 0px 0px 0px",
    threshold: 0.8,
  });
  const pages = document.querySelectorAll(".page4");
  pages.forEach((page) => {
    observer.observe(page);
  });

  const lines = document.querySelectorAll(".pages-container.page4-1 p");
  const activeObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("active");
        } else {
          entry.target.classList.remove("active");
        }
      });
    },
    {
      rootMargin: "-45% 0px -45% 0px",
      threshold: 0,
    }
  );
  const blurObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.remove("blur");
        } else {
          entry.target.classList.add("blur");
        }
      });
    },
    {
      rootMargin: "-30% 0px -30% 0px",
      threshold: 0,
    }
  );

  lines.forEach((line) => {
    activeObserver.observe(line);
    blurObserver.observe(line);
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

const collageInfoRef = toRef(props, "collageInfo");
const collageRef = toRef(props, "collage");
const userInfoRef = toRef(props, "userInfo");
</script>

<style scoped>
.pages-container {
  overflow: visible;
}

.page4-1 {
  background: linear-gradient(#e49292 15%, #f2b823 85%);
  padding-block: 70svh 80svh;
  text-align: center;
  padding-inline: var(--page-padding);
  color: #ffffff70;
  font-size: var(--fs-500);
  height: auto;
}
.group {
  margin-bottom: 3rem;
}

.page4-1 p.active {
  color: #fff;
  transform: scale(1.1);
  filter: unset;
  mix-blend-mode: unset;
}

.page4-1 p.blur {
  filter: blur(5px);
}

.page4-1 p {
  padding-block: 0.75rem;
  transition: all 0.3s ease-in-out;
  filter: blur(1.5px);
  mix-blend-mode: soft-light;
}
.lyric {
  font-size: var(--fs-600);
  font-weight: 900;
  padding-block: 0.5rem;
  /* -webkit-background-clip: text;
  background-clip: text; */
}

.page4-2 {
  background: linear-gradient(#f2b823 0%, #96b255 55%, #57904d 95%);
  padding-block: 3rem 0;
}

.page4-2 h2 {
  font-size: var(--fs-500);
  margin-bottom: 1.5rem;
  padding-inline: var(--page-padding);
}

.horizontal_scroller {
  padding-inline: var(--page-padding);
  display: flex;
  flex-direction: row;
  gap: 1rem;
  overflow-x: scroll;
  scroll-snap-type: x mandatory;
  padding-bottom: 0.5rem;
}
.horizontal_scroller::before,
.horizontal_scroller::after {
  content: "";
  display: block;
  padding: 0.01rem;
}

.card {
  scroll-snap-align: center;
  flex-shrink: 0;
  width: 95%;
  background-color: #ffffff39;
  border: 1px solid #ffffff;
  border-radius: 0.5rem;
  padding: 2.5rem 1rem;
  gap: 3rem;
  display: flex;
  flex-direction: column;
}

.card p {
  font-size: var(--fs-400);
  padding-left: 0.5ch;
}
.card p .b {
  font-size: var(--fs-600);
}

.keyWords {
  padding-inline: var(--page-padding);
  font-size: var(--fs-400);
  gap: 1rem;
}
.keyWords span {
  font-size: var(--fs-500);
  font-weight: 900;
  padding-inline: 0.5rem;
  gap: 1rem;
}
</style>
