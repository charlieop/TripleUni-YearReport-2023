<template>
  <main scroll="no">
    <div class="content-wrapper">
      <IndexOverlay />
      <Page1
        v-if="userInfo.data"
        :collageInfo="collageInfo"
        :collage="collage"
        :userInfo="userInfo.data"
      />
      <Page2
        v-if="userInfo.data"
        :collageInfo="collageInfo"
        :collage="collage"
        :userInfo="userInfo.data"
      />
      <Page3
        v-if="userInfo.data"
        :collageInfo="collageInfo"
        :collage="collage"
        :userInfo="userInfo.data"
      />
      <Test />
      <div class="copyright-label">©TripleUni 2023</div>
    </div>
  </main>
</template>

<script setup>
import Test from "@/components/Test.vue";

import IndexOverlay from "@/components/IndexOverlay.vue";
import Page1 from "@/components/Page1.vue";
import Page2 from "@/components/Page2.vue";
import Page3 from "@/components/Page3.vue";
import { onMounted, ref } from "vue";

const collageInfo = {
  name: ["HKU噗噗", "马料水哔哔机", "科大空间站"],
  nickname: ["噗噗", "哔哔机", "星尘"],
  creationTime: ["2020年 10月 31日", "2020年 8月 24日", "2020年 5月 2日"],
};

let collage = ref(0);
let userInfo = ref({ report_data: {} });

onMounted(async () => {
  userInfo.value = await getUserInfo("temp");
  console.log(userInfo.value.data);
  switch (userInfo.value.data.user_school_label) {
    case "HKU":
      collage.value = 1;
      break;
    case "CUHK":
      collage.value = 1;
      break1;
    case "HKUST":
      collage.value = 2;
      break;
  }
});

async function getUserInfo(token) {
  const URL = "https://api.uni.hkupootal.com/v4/report2023_new/get.php";
  const data = {
    token: token,
  };
  const options = {
    method: "POST",
    body: JSON.stringify(data),
  };
  try {
    const response = await fetch(URL, options);
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }
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
  bottom: 2%;
  left: 4%;
  width: fit-content;
  font-size: var(--fs-300);
  font-weight: 900;
  font-style: italic;
  mix-blend-mode: soft-light;
  color: white;
  z-index: 10;
}
</style>
