<template>
  <main scroll="no">
    <div class="content-wrapper">
      <IndexOverlay />
      <div class="test">1</div>
      <div class="test r">2</div>
      <div class="test">3</div>
      <div class="test r long">4. long</div>
      <div class="test">5</div>
      <div class="test r">6</div>
      <div class="test">7</div>
      <div class="test r">8</div>
      <div class="test">9</div>
    </div>
  </main>
</template>

<script setup>
import IndexOverlay from "@/components/IndexOverlay.vue";
</script>

<style scoped>
main {
  display: grid;
  place-items: center;
  height: 100svh;
  width: 100%;
  background-color: aliceblue;
}

.test {
  width: var(--page-width);
  height: 100svh;
  background: linear-gradient(
    to bottom,
    #051937,
    #004d7a,
    #008793,
    #00bf72,
    #a8eb12
  );

  color: white;
  font-size: var(--fs-900);
  font-weight: 700;
  text-align: center;
  padding-top: 20rem;
}

.r {
  background: linear-gradient(
    to bottom,
    #f90242,
    #ff5d1c,
    #f99200,
    #e6c000,
    #c7e928
  );
}

.long {
  height: 300svh;
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
</style>
