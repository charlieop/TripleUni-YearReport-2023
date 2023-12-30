<template>
  <div class="post" v-if="postInfo" @click="showDetail">
    <div class="post-header post-row">
      <div class="post-id">#{{ postInfo.post_id }}</div>
      <img
        class="post-avatar"
        :src="'https://i.boatonland.com/avatar/' + postInfo.user_avatar"
        alt=""
      />
      <div class="post-type">{{ postInfo.post_topic }}</div>
      <div class="post-data">
        <img
          class="post-icon"
          :src="
            postInfo.is_following
              ? rootPath + 'src/assets/imgs/icons/sf-star-fill.svg'
              : rootPath + '/src/assets/imgs/icons/sf-star.svg'
          "
          alt=""
        />
        <span>{{ postInfo.post_follower_num }}</span>
      </div>
      <div class="post-data">
        <img
          class="post-icon"
          src="@/assets/imgs/icons/sf-comment.svg"
          alt=""
        />
        <span>{{ postInfo.post_comment_num }}</span>
      </div>
    </div>
    <div class="post-content post-row">
      <div class="post-text">
        {{ postInfo.post_msg }}
      </div>
    </div>
  </div>
  <div class="dim transit" @click="hideDetail"></div>
  <div class="post-expand transit" v-if="postInfo">
    <div class="post-header post-row">
      <div class="post-id">#{{ postInfo.post_id }}</div>
      <img
        class="post-avatar"
        :src="'https://i.boatonland.com/avatar/' + postInfo.user_avatar"
        alt=""
      />
      <div class="post-type">{{ postInfo.post_topic }}</div>
      <div class="post-data">
        <img
          class="post-icon"
          :src="
            postInfo.is_following
              ? rootPath + 'src/assets/imgs/icons/sf-star-fill.svg'
              : rootPath + '/src/assets/imgs/icons/sf-star.svg'
          "
          alt=""
        />
        <span>{{ postInfo.post_follower_num }}</span>
      </div>
      <div class="post-data">
        <img
          class="post-icon"
          src="@/assets/imgs/icons/sf-comment.svg"
          alt=""
        />
        <span>{{ postInfo.post_comment_num }}</span>
      </div>
    </div>
    <div class="post-content post-row">
      <div class="post-text">
        {{ postInfo.post_msg }}
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(["postInfo"]);

let rootPath = window.location;

function showDetail() {
  const dim = document.querySelector(".dim");
  const postExpand = document.querySelector(".post-expand");
  dim.style.zIndex = "999";
  postExpand.style.zIndex = "999";
  dim.classList.add("active");
  postExpand.classList.add("active");
}

function hideDetail() {
  const dim = document.querySelector(".dim");
  const postExpand = document.querySelector(".post-expand");
  dim.classList.remove("active");
  postExpand.classList.remove("active");
  setTimeout(() => {
    dim.style.zIndex = "-1";
    postExpand.style.zIndex = "-1";
  }, 500);
}
</script>

<style scoped>
.wrapper {
  width: 100%;
  height: 100%;
  background-color: antiquewhite;
  display: flex;
  flex-direction: column;
  gap: 3rem;
  justify-content: center;
  align-items: center;
  padding-inline: 1rem;
}
.post {
  width: 100%;
  height: 7.5rem;
  background-color: white;
  box-shadow: 0 0 0.3rem rgba(0, 0, 0, 0.5);
  border-radius: 0.75rem;
  padding: 0.25rem 0.75rem;
  display: grid;
  grid-template-rows: 1fr 1fr;
  font-size: var(--fs-300);
  overflow: clip;
  z-index: 1;
}

.transit {
  transition: all 0.5s ease-in-out;
  z-index: -1;
}

.active {
  opacity: 1 !important;
}

.post-expand {
  position: fixed;
  transform: translateY(calc(-50% - 5svh));
  inset: 50% calc((100vw - var(--page-width)) / 2 + 0.5rem) auto
    calc((100vw - var(--page-width)) / 2 + 0.5rem);
  max-height: 60svh;
  background-color: white;
  border-radius: 0.75rem;
  padding: 0.25rem 0.75rem;
  display: flex;
  flex-direction: column;
  box-shadow: 0 0 0.3rem rgba(0, 0, 0, 0.5);
  overflow-y: scroll;
  opacity: 0;
  font-size: var(--fs-400);
}

.dim {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  opacity: 0;
  -webkit-backdrop-filter: blur(0.5rem);
  backdrop-filter: blur(0.5rem);
}

.post-expand .post-header {
  flex-shrink: 0;
  height: 3.5rem;
}

.post-expand .post-content {
  flex-shrink: 0;
  padding-block: 1rem;
}

.post-expand .post-text {
  display: block;
}

.post-row {
  position: relative;
  display: flex;
  flex-direction: row;
  gap: min(0.5rem, 3%);
  align-items: center;
  padding-inline: 0.75rem;
}

.post-row:first-child::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 1px;
  background-color: var(--clr-background-muted);
}

.post-id {
  font-weight: 900;
}

.post-avatar {
  object-fit: contain;
  object-position: center;
  width: 1.75rem;
  height: 1.75rem;
}

.post-type {
  background-color: var(--clr-background-muted);
  color: var(--clr-text-muted);
  padding: 0.25rem 0.75rem;
  border-radius: 100rem;
  margin-right: auto;
}

.post-data {
  display: flex;
  flex-direction: row;
  gap: 0.25rem;
  margin-left: 0.25rem;
  align-items: center;
  color: var(--clr-text-muted);
}

.post-icon {
  width: 1rem;
  height: 1rem;
}

.post-text {
  -webkit-line-clamp: 1;
  line-clamp: 1;
  text-overflow: ellipsis;
  display: -webkit-box;
  overflow: clip;
  -webkit-box-orient: vertical;
}
</style>
