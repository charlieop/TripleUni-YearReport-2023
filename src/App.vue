<template>
  <main scroll="no">
    <div class="content-wrapper">
      <IndexOverlay :loaded="loaded" />
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
      <Page4
        v-if="userInfo.data"
        :collageInfo="collageInfo"
        :collage="collage"
        :userInfo="userInfo.data"
      />
      <Page5_1
        v-if="userInfo.data"
        :collageInfo="collageInfo"
        :collage="collage"
        :userInfo="userInfo.data"
      />
      <Page5_2
        v-if="userInfo.data"
        :collageInfo="collageInfo"
        :collage="collage"
        :userInfo="userInfo.data"
      />
      <Page5_3
        v-if="userInfo.data"
        :collageInfo="collageInfo"
        :collage="collage"
        :userInfo="userInfo.data"
      />
      <Page6
        v-if="userInfo.data"
        :collageInfo="collageInfo"
        :collage="collage"
        :userInfo="userInfo.data"
      />
      <!-- <Test /> -->
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
import Page4 from "@/components/Page4.vue";
import Page5_1 from "@/components/Page5_1.vue";
import Page5_2 from "@/components/Page5_2.vue";
import Page5_3 from "@/components/Page5_3.vue";
import Page6 from "@/components/Page6.vue";

import { onMounted, ref, computed } from "vue";

const collageInfo = {
  name: ["HKU噗噗", "马料水哔哔机", "科大空间站"],
  nickname: ["噗噗", "哔哔机", "星尘"],
  creationTime: ["2020年 10月 31日", "2020年 8月 24日", "2020年 5月 2日"],
};

let collage = ref(0);
let userInfo = ref({ report_data: {} });

let loaded = computed(() => {
  return userInfo.value.data != undefined;
});

const testToken1 =
  "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJIS1XlmZflmZciLCJhdWQiOiJIS1VlciIsImlhdCI6MTcwNjE1MjA0Ni42NDU1NzUsImV4cCI6MTcwODc0NDA0Ni42NDU1NzUsInVzZXJfaXRzYyI6ImxzdHN0YXIiLCJ1c2VyX3NjaG9vbF9sYWJlbCI6IkhLVSIsInBsYXRmb3JtIjoid2ViIn0.h4_LNXcqIiGht_Aq0x8FnrdlemW2eAgGW_BcSSb7HHI";
const testToken2 =
  "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJIS1XlmZflmZciLCJhdWQiOiJIS1VlciIsImlhdCI6MTcwNzI2NjU2Ny45MDgxMjEsImV4cCI6MTcwOTg1ODU2Ny45MDgxMjEsInVzZXJfaXRzYyI6Im1vbmljYTAwIiwidXNlcl9zY2hvb2xfbGFiZWwiOiJIS1UiLCJwbGF0Zm9ybSI6IndlYiJ9.nZqgUVUnDezP84ThKqnQZRGc-LcYNo5mf7XW8IhpWJc";
const testToken3 =
  "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJTdGFyZHVzdOenkeWkp-epuumXtOermSIsImF1ZCI6IlVTVGVyIiwiaWF0IjoxNzA3MjY2NTM4LjMzOTg1MiwiZXhwIjoxNzA5ODU4NTM4LjMzOTg1MiwidXNlcl9pdHNjIjoic3BjaGFuYWUiLCJ1c2VyX3NjaG9vbF9sYWJlbCI6IlVTVCIsInBsYXRmb3JtIjoid2ViIn0.tjajHIHeBoVtGGrh1V1hAS0pvRi6Qe_MAA54LH3ndU4";
const falseToken = "haha";

// const userToken = new URL(location.href).searchParams.get('token')

// For Testing only !!!!
const userToken = new URL(location.href).searchParams.get('token')? new URL(location.href).searchParams.get('token') : testToken3;

onMounted(async () => {
  userInfo.value = await getUserInfo(userToken);

  if (userInfo.value.code >= 800) {
    console.log("Error with code", userInfo.value.code);
    return;
  } else if (userInfo.value.code == 400) {
    console.log("No User Report Found", userInfo.value.code);
    return;
  }

  console.log(userInfo.value.data);
  if (!userInfo.value.data) {
    console.log("Error: No user data");
    return;
  }
  switch (userInfo.value.data.user_school_label) {
    case "HKU":
      collage.value = 0;
      break;
    case "CUHK":
      collage.value = 1;
      break1;
    case "UST":
      collage.value = 2;
      break;
  }
});

async function getUserInfo(token) {
  const URL = "https://api.tripleuni.com/v4/report2023/get.php";
  const data = new URLSearchParams();
  data.append("token", token);
  const options = {
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded",
    },
    body: data.toString(),
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
  position: fixed;
  bottom: 2%;
  left: calc(var(--page-width) * 0.03 + 50vw - var(--page-width) / 2);
  width: fit-content;
  font-size: var(--fs-300);
  font-weight: 900;
  font-style: italic;
  mix-blend-mode: soft-light;
  color: white;
  z-index: 10;
}
</style>
