<template>
  <main scroll="no">
    <div class="content-wrapper">
      <IndexOverlay />
      <Page1
        v-if="userInfo.report_data"
        :collageInfo="collageInfo"
        :collage="collage"
        :userInfo="userInfo.report_data"
      />
      <Test />
      <div class="copyright-label">©TripleUNI 2023</div>
    </div>
  </main>
</template>

<script setup>
import Test from "@/components/Test.vue";

import IndexOverlay from "@/components/IndexOverlay.vue";
import Page1 from "@/components/Page1.vue";
import { onMounted, ref } from "vue";

const collageInfo = {
  name: ["噗噗", "马料水BB机", "Stardust科大空间站"],
  nickname: ["噗噗", "BB机", "星尘"],
  creationTime: ["2020年 10月 31日", "2020年 8月 24日", "2020年 5月 2日"],
};

let collage = ref(0);
let userInfo = ref({ report_data: {} });

onMounted(async () => {
  userInfo.value = await getUserInfo("temp");
  console.log(userInfo.value.report_data);
  switch (userInfo.value.report_data.user_school_label) {
    case "HKU":
      collage.value = 0;
      break;
    case "CUHK":
      collage.value = 1;
      break;
    case "HKUST":
      collage.value = 2;
      break;
  }
});

async function getUserInfo(token) {
  const URL = "https://api.uni.hkupootal.com/v4/report2023/get.php";
  const data = {
    token: token,
  };
  const options = {
    method: "POST",
    body: JSON.stringify(data),
  };
  try {
    const response = await fetch(URL, options);
    const data = await response.json();
    return data;
  } catch (error) {
    console.log("Error:", error);
  }
}

document.addEventListener("touchstart", function () {}, true);
</script>

<style>
main {
  display: grid;
  place-items: center;
  height: 100svh;
  width: 100%;
  background-color: aliceblue;
}

.content-wrapper {
  width: var(--page-width);
  height: var(--page-height);

  position: relative;

  background-color: rgb(255, 4, 188);

  overflow-x: clip;
  overflow-y: scroll;

  scroll-snap-type: y mandatory;
}

.content-wrapper > * {
  scroll-snap-align: start;
}

.copyright-label {
  position: sticky;
  bottom: 4%;
  left: 8%;
  width: fit-content;
  font-size: var(--fs-300);
  font-weight: 900;
  font-style: italic;
  mix-blend-mode: soft-light;
  color: white;
  z-index: 10;
}
</style>
