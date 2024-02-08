<template>
  <div class="pages-container page6">
    <div class="page page6-1" style="color: whitesmoke">
      <div class="image-outer">
        <img :src="imageUrl" v-if="imageUrl" class="rendered-image">
      </div>
      <div class="frame-outer" @click="getImage" ref="frameArea">
        <div class="frame-bg"></div>
        <div class="frame-inner">
          <div style="display: flex; justify-content: space-between; margin-bottom: 2.9rem;">
            
            <div style="display: flex; justify-content: center; align-items: center;  width: 100%;" v-if="showId">
              <img class="avatar"
                :src="'https://i.boatonland.com/avatar/' + userInfoRef.user_avatar + '.svg?v=' + timeStamp"
                crossOrigin="anonymous" />
              <!-- <img class="avatar" src="https://i.boatonland.com/avatar/party-12.svg"/> -->
              <p>{{ userInfoRef.user_serial }}</p>
            </div>
            <div style="height: 1.74rem" v-else></div>
          </div>

          <div style="display: flex; flex-direction: column;  padding: 2% 5% 0.7rem 15%;">
            <p class="accent-text">{{ title1 }}</p>

            <p style="display: inline;  white-space: pre; line-height: 1.2rem; font-weight: 600; font-size: 75%;">{{ title2 }}</p>
            
          </div>

          <div style="position: absolute; bottom: 16.3%; left:0; display: flex; flex-direction: column; align-items: center; width: 100%;">
          <p style="font-size:medium; text-align: center; margin: 0 0 1.4rem 0; font-weight:600">{{ collage_nickname + '恭喜你达成了以下成就' }}</p>

          <div style="display: flex; width: 70%; justify-content: space-between;">
            <div class="acheivements" v-for="item in displayed_acheivements" style="display: flex; flex-direction: column; justify-content: space-between;">
              <pre style="font-size: 0.9rem;  white-space: pre-wrap; margin-bottom: 0.4rem; font-weight: 3rem; line-height: 1rem;">{{ item.title }}</pre>
              <p style="font-size: 0.6rem; white-space: pre-wrap; line-height: 0.7rem;">{{ item.description }}</p>
            </div>
          </div>

          </div>
        </div>
      </div>

      <p style="font-size:var(--fs-300); text-align: center; margin-top: 3%; z-index: 5;"> &uarr; 长按保存分享</p>

      <div style="display: flex; justify-content: space-between; margin: 0 6%; ">
        <button @click="showId = !showId; getImage();" class="buttons" style="background-color: #F8EEAB;">{{ showId ? '隐藏ID' : '显示ID' }}</button>
        <button @click="changeAcheimentsIdx()" class="buttons" style="background-color: #D8C9F5;" :disabled="acheivements.length <= 3"> 切换成就 </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { toRef, computed, onMounted, ref } from "vue";
import { toPng } from 'html-to-image';

const props = defineProps({
  collageInfo: Object,
  collage: Number,
  userInfo: Object,
});

const imageUrl = ref("");
const timeStamp = ref(new Date().getTime());

const collageInfoRef = toRef(props, "collageInfo");
const collageRef = toRef(props, "collage");
const userInfoRef = toRef(props, "userInfo");

const frameArea = ref(null);
const showId = ref(true);

const collage_nickname = ref(collageInfoRef.value.nickname[collageRef.value]);
const title1 = ref(userInfoRef.value.title_1);
const title2 = ref(userInfoRef.value.title_2.replaceAll('！','!'));

const acheivements = ref(userInfoRef.value.title_list);
const displayed_acheivements = ref(acheivements.value.slice(0, 3));

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
});

function draw(withName) {
  let canvas = document.getElementById("myCanvas");
  let ctx = canvas.getContext("2d");
  let collage_nickname = collageInfoRef.value.nickname[collageRef.value];
  // Set height and width
  canvas.style.width = "100%";
  canvas.style.height = "100%";
  ctx.canvas.width = document.getElementById("frame-outer").offsetWidth;
  ctx.canvas.height = document.getElementById("frame-outer").offsetHeight;
  // Load assest
  const instax_frame = new Image();
  instax_frame.src = "https://i.boatonland.com/report2023/instax-frame.svg";
  let frame_width = canvas.width;
  let frame_height = canvas.width * 1.47531172;
  let margin = canvas.width / 9;
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
      avatar.src =
        "https://i.boatonland.com/avatar/" +
        userInfoRef.value.user_avatar +
        ".svg";
      avatar.onload = () => {
        ctx.drawImage(
          avatar,
          frame_width - ctx.measureText(name).width - margin - 50,
          margin + 12,
          30,
          30
        );
      };

      // Draw name
      ctx.font = "20px AlibabaPuHuiTi";
      ctx.fillStyle = "black";
      ctx.fillText(
        name,
        frame_width - ctx.measureText(name).width - margin - 10,
        margin + 38
      );
    }

    // Draw 谁在干嘛
    ctx.font = "18px AlibabaPuHuiTi";
    ctx.fillStyle = "black";
    const title1 = userInfoRef.value.title_1
      .replace("REPLACE", collage_nickname)
      .replace(/「.*」/g, "");
    ctx.fillText(title1, margin + 15, margin + 90);
    const title2 = userInfoRef.value.title_2
      .replace("REPLACE", collage_nickname)
      .replace(/「.*」/g, "");
    ctx.fillText(
      title2,
      frame_width - ctx.measureText(title2).width - margin - 5,
      margin + 125
    );
  };
}

function changeAcheimentsIdx() {
  displayed_acheivements.value = randomSelection(3, acheivements.value);
  getImage();
}

function randomSelection(n, originalArray) {
  let newArr = [];
  if (n >= originalArray.length) {
    return originalArray;
  }
  for (let i = 0; i < n; i++) {
    let newElem =
      originalArray[Math.floor(Math.random() * originalArray.length)];
    while (newArr.includes(newElem)) {
      newElem = originalArray[Math.floor(Math.random() * originalArray.length)];
    }
    newArr.push(newElem);
  }
  return newArr;
}

function getImage() {
  console.log(frameArea);
  toPng(frameArea.value, { useCorsEverywhereProxy: true })
    .then(function (dataUrl) {
      // console.log(dataUrl)
      imageUrl.value = dataUrl;
    })
    .catch(function (error) {
      console.error("oops, something went wrong!", error);
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
  background: #ADD6F2; 
  /* linear-gradient(180deg, #00B9FF 10%, #D3F4FF 90%); */
  /* padding-bottom: 7rem; */
}

#canvas-container {
  position: relative;
  width: 100%;
  /* height: 80%; */
  overflow: hidden;
  z-index: 1;
}

.frame-outer {
  margin-top: 1vw;
  position: relative;
  width: 100%;
  aspect-ratio: 1 / 1.8;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  z-index: 1;
}

.frame-inner {
  /* background-color: white; */
  width: 100%;
  aspect-ratio: 4 / 7;
  align-self: flex-start;
  z-index: 2;
  /* background-image: url("@/assets/imgs/instax-frame.svg"); */
  background-image: url("https://i.boatonland.com/report2023/canvas2.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  padding: 41% 15% 36% 15%;
  color: black;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  position: relative;
  border-radius: 20px;
  /* background-color: white; */
}

.frame-bg {
  position: absolute;
  margin-top: 0.6rem;
  width: 90%;
  height: 80%;
  z-index: 1;
  /* background-color: white; */
}

.content {
  background-color: white;
  width: 80%;
  height: 80%;
  position: absolute;
}

.image-outer {
  /* margin-top: 2rem; */
  width: 94%;
  margin-left: 3%;
  margin-top: 4% ;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  position: absolute;
  aspect-ratio: 1 / 1.8;
  overflow-y: hidden;
  top: 0;
  left: 0;
  z-index: 5;
}

.rendered-image {
  align-self: flex-start;
  width: 95%;
  z-index: 6;
}

.avatar {
  width: 1.3rem;
  margin-right: 0.3rem;
  aspect-ratio: 1;
}

.accent-text {
  color: #00749f;
  /* background: linear-gradient(
    90deg,
    #a48ed7 15.82%,
    #ca90ae 48.61%,
    #e49292 96.54%
  );; */
  /* -webkit-background-clip: text;
  -webkit-text-fill-color: transparent; */
  /* color: linear-gradient(180deg, #00B9FF 10%, #D3F4FF 90%); */
  font-weight: 900;
  /* text-decoration: underline; */
  font-size: 78%;
  margin: 0.2rem 0;
}

.acheivements {
  text-align: center;
  width: 30%;
  /* border: #ffb235 1px solid; */
  /* background-color: #f9f0d1; */
  border-radius: 10px;
  padding: 0.2rem 0.3rem;
  line-break: loose;
  white-space: nowrap;
  position: relative;
  height: 2.5rem;
}

.buttons {
  border: none;
  margin: 1rem 0;
  inset: auto 10% 10% 10%;
  text-align: center;
  padding: 0.6rem 0;
  background: #fff;
  color: #000;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  border-radius: 20px;
  box-shadow: 0 0 0.1rem rgba(0, 0, 0, 0.5);
  transition: all 0.3s ease-in-out;
  width: 45%;
}

.buttons:hover,
.buttons:focus {
  filter: brightness(0.8);
}

.buttons:active {
  filter: brightness(0.6);
  transform: scale(0.95);
}

.buttons:disabled {
  filter: brightness(0.6);
  transform: scale(1);
}
</style>
