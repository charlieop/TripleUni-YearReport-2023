<template>
  <div class="wrapper" v-if="!hidePage">
    <div class="container">
      <h1>
        <p>TripleUni 2023</p>
        <p>年度总结</p>
      </h1>
      <div class="button" @click="moveUp">查收我的报告</div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
const hidePage = ref(false);

function moveUp() {
  const wrapper = document.querySelector(".wrapper");
  wrapper.style.transform = "translateY(-100vh)";
  //   wrapper.style.opacity = "0";

  setTimeout(() => {
    hidePage.value = true;
  }, 1000);
}
</script>

<style scoped>
.wrapper {
  --_top-padding: 5svh;
  position: fixed;
  top: calc((100svh - var(--page-height)) / 2 - var(--_top-padding));
  left: calc((100vw - var(--page-width)) / 2);
  height: calc(var(--page-height) + var(--_top-padding));
  width: var(--page-width);
  padding-top: var(--_top-padding);
  z-index: 1000;
  background: var(--clr-background);
  transition: transform 1s cubic-bezier(1, -0.24, 0.92, 0.7);
}

.container {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 3rem;
  text-align: center;
  justify-content: space-around;
  align-items: center;
  padding: 5rem 1rem;
}

h1 {
  font-size: var(--fs-700);
}

.button {
  width: fit-content;
  padding: 1rem 2rem;
  background-color: var(--clr-accent);
  border-radius: 100rem;
  font-size: var(--fs-500);
  transition: all 0.2s ease-in-out;
}

.button:hover,
:focus {
  filter: brightness(0.9);
  transform: scale(1.05);
  cursor: pointer;
}
</style>
