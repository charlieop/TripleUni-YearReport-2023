<template>
  <div class="pages-container page6">
    <div class="page page6-1" style="color: whitesmoke">
      <div class="image-outer">
          <img :src="imageUrl" v-if="imageUrl" class="rendered-image">
      </div>
      <div class="frame-outer" @click="getImage" ref="frameArea">

        <div class="frame-bg"></div>
        <div class="frame-inner">
          <div style="display: flex; justify-content: space-between;">
            <p>2023年</p>
            <div style="display: flex; margin-bottom: 1rem;">
              <img class="avatar"
                :src="'https://i.boatonland.com/avatar/' + userInfoRef.user_avatar + '.svg?v=' + timeStamp"
                crossOrigin="anonymous" />
              <!-- <img class="avatar" src="https://i.boatonland.com/avatar/party-12.svg"/> -->
              <p>{{ userInfoRef.user_serial }}</p>
            </div>
          </div>

          <div style="display: flex; flex-direction: column;">
            <p>{{ title1 }}</p>
            <p style="align-self: flex-end;">{{ title2 }}</p>
          </div>


        </div>
      </div>
    </div>
  </div>
</template>

<script setup>

import { toRef, computed, onMounted, ref } from "vue";
import instax_img from "@/assets/imgs/instax-frame.svg";
import { toPng } from 'html-to-image';

const props = defineProps({
  collageInfo: Object,
  collage: Number,
  userInfo: Object,
});

const imageUrl = ref('')
const timeStamp = ref(new Date().getTime())

const collageInfoRef = toRef(props, "collageInfo");
const collageRef = toRef(props, "collage");
const userInfoRef = toRef(props, "userInfo");

const frameArea = ref(null)

const collage_nickname = ref(collageInfoRef.value.nickname[collageRef.value]);
const title1 = ref(userInfoRef.value.title_1.replace('REPLACE', collage_nickname.value).replace(/「.*」/g, ''));
const title2 = ref(userInfoRef.value.title_2.replace('REPLACE', collage_nickname.value).replace(/「.*」/g, ''));

onMounted(() => {
  const observer = new IntersectionObserver(animate, {
    root: document.querySelector(".content-wrapper"),
    rootMargin: "0px 0px 0px 0px",
    threshold: 0.8,
  });
  const pages = document.querySelectorAll(".page6 .page");
  pages.forEach((page) => {
    observer.observe(page);
  });
  getImage();
  // draw(true);

});

function draw(withName) {
  let canvas = document.getElementById("myCanvas");
  let ctx = canvas.getContext("2d");
  let collage_nickname = collageInfoRef.value.nickname[collageRef.value];
  // Set height and width
  canvas.style.width = '100%';
  canvas.style.height = '100%';
  ctx.canvas.width = document.getElementById("frame-outer").offsetWidth;
  ctx.canvas.height = document.getElementById("frame-outer").offsetHeight;
  // Load assest
  const instax_frame = new Image();
  instax_frame.src = instax_img
  let frame_width = canvas.width
  let frame_height = canvas.width * 1.47531172
  let margin = canvas.width / 9
  instax_frame.onload = () => {
    // Draw instax frame
    ctx.fillStyle = "white";
    ctx.fillRect(20, 20, frame_width - 40, frame_height - 80);
    ctx.drawImage(instax_frame, 0, 0, frame_width, frame_height);

    // Draw year
    ctx.font = "22px AlibabaPuHuiTi";
    ctx.fillStyle = "black";
    ctx.fillText("2023", margin + 15, margin + 40);

    // Draw report title
    ctx.font = "22px AlibabaPuHuiTi";
    ctx.fillStyle = "black";
    // ctx.fillText(collageInfoRef.value.name[collageRef.value] + "年度报告", 50, 110);

    // Draw avatar name
    if (withName) {
      const name = userInfoRef.value.user_serial;
      const avatar = new Image();
      avatar.src = "https://i.boatonland.com/avatar/" + userInfoRef.value.user_avatar + ".svg";
      avatar.onload = () => { ctx.drawImage(avatar, frame_width - ctx.measureText(name).width - margin - 50, margin + 12, 30, 30) };

      // Draw name
      ctx.font = "20px AlibabaPuHuiTi";
      ctx.fillStyle = "black";
      ctx.fillText(name, frame_width - ctx.measureText(name).width - margin - 10, margin + 38);
    }

    // Draw 谁在干嘛
    ctx.font = "18px AlibabaPuHuiTi";
    ctx.fillStyle = "black";
    const title1 = userInfoRef.value.title_1.replace('REPLACE', collage_nickname).replace(/「.*」/g, '')
    ctx.fillText(title1, margin + 15, margin + 90);
    const title2 = userInfoRef.value.title_2.replace('REPLACE', collage_nickname).replace(/「.*」/g, '')
    ctx.fillText(title2, frame_width - ctx.measureText(title2).width - margin - 5, margin + 125);
  }
}

function getImage() {
  console.log(frameArea)
  toPng(frameArea.value, { useCorsEverywhereProxy: true })
    .then(function (dataUrl) {
      // console.log(dataUrl)
      imageUrl.value = dataUrl
    })
    .catch(function (error) {
      console.error('oops, something went wrong!', error);
    });
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
  background: linear-gradient(180deg, #00B9FF 10%, #D3F4FF 90%);
}

#canvas-container {
  position: relative;
  width: 100%;
  /* height: 80%; */
  overflow: hidden;
  z-index: 1;
}

.frame-outer {

  position: relative;
  width: 100%;
  aspect-ratio: 1 / 1.37;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;


}

.frame-inner {
  /* background-color: white; */
  width: 100%;
  aspect-ratio: 1 / 1.37;
  align-self: flex-start;
  z-index: 2;
  /* background-image: url("@/assets/imgs/instax-frame.svg"); */
  background-image: url("@/assets/imgs/instax-frame.svg");
  padding: 13% 15% 36% 15%;
  color: black;
  overflow: hidden;
  display: flex;
  flex-direction: column;

}

.frame-bg {
  position: absolute;
  width: 90%;
  height: 90%;
  z-index: 1;
  background-color: white;
}

.content {
  background-color: white;
  width: 80%;
  height: 80%;
  position: absolute;
}

.image-outer {
  /* margin-top: 2rem; */
  width: 100%;
  height: fit-content;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 5;
}

.rendered-image {
  width: 100%;
  z-index: 6;
}

.avatar {
  width: 1.5rem;
  margin-right: 0.3rem;
  aspect-ratio: 1;
}
</style>
