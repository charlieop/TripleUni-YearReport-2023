<template>
  <main scroll="no">
    <div class="content-wrapper">
      <IndexOverlay />
      <Page1
        :creationTime="collageInfo.creationTime[collage]"
        :name="collageInfo.name[collage]"
        :nickname="collageInfo.nickname[collage]"
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

const collageInfo = {
  name: ["噗噗", "马料水BB机", "Stardust科大空间站"],
  nickname: ["噗噗", "BB机", "星尘"],
  creationTime: ["2020年 11月 31日", "2020年 11月 31日", "2020年 11月 31日"],
};

const collage = 0;

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
