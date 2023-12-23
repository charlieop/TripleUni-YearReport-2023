<template>
  <main scroll="no">
    <div class="content-wrapper">
      <IndexOverlay />
      <div class="test">
        1
        <p>Wellcome!</p>
      </div>
      <div class="test r">2</div>
      <div class="test long">
        <div class="sticky">
          3
          <p>Long</p>
        </div>
        <div class="blank"></div>
        sticky title ⬆
        <div class="blank"></div>
        can also disappear
        <div class="blank"></div>
        if needed
      </div>
      <div class="test r">4</div>
      <div class="test inner">
        <div class="innerFrame">
          5.1
          <p>Scroll Right →</p>
        </div>
        <div class="innerFrame">5.2</div>
        <div class="innerFrame">
          5.3
          <p>Keep scrolling ↓</p>
        </div>
      </div>
      <div class="test r">6</div>
      <div class="test long">
        7
        <p>Pan effect</p>
        <div class="blank"></div>
        <div class="pan">hey!</div>
        <div class="blank"></div>
        <div class="pan">Isn't this cool</div>
      </div>
      <div class="test r">8</div>
      <div class="test">End!</div>
    </div>
  </main>
</template>

<script setup>
import IndexOverlay from "@/components/IndexOverlay.vue";
import { onMounted } from "vue";

onMounted(() => {
  const pan = document.querySelectorAll(".pan");
  const wrapper = document.querySelector(".content-wrapper");
  // pan.forEach(element => {
  //   const observer = new IntersectionObserver((event)=>{
  //     console.log(event);
  //     addRemoveListner(event);
  //   },
  //   {
  //     root: document.querySelector(".content-wrapper"),
  //     rootMargin: "0px 100% 0px 100%",
  //     threshold: 1
  //   });
  //   observer.observe(element);
  // });
  wrapper.addEventListener("scroll", (event) => {
    // console.log(event);
    const pan = document.querySelectorAll(".pan");
    pan.forEach((element) => {
      const scrollPos = wrapper.scrollTop;
      const panPos = element.offsetTop;
      const progress =
        (scrollPos - panPos + wrapper.offsetHeight) / wrapper.offsetHeight;

      if (progress > 0 && progress < 1) {
        element.style.transform = "translateX(" + (50 - 100 * progress) + "%)";
        console.log(progress / wrapper.offsetHeight);
      }
    });
  });
});
</script>

<style scoped>
main {
  display: grid;
  place-items: center;
  height: 100svh;
  width: 100%;
  background-color: aliceblue;
}

.pan {
  transform: translateX(50%);
  transition: transform 0.1 cubic-bezier(0.31, 0.61, 0, 0.78);
}

.test {
  width: var(--page-width);
  height: var(--page-height);
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
  height: calc(var(--page-height) * 3);
}

.blank {
  height: 30svh;
}

.sticky {
  position: sticky;
  top: 0;
  padding: 10% 3rem;
  margin-bottom: 10%;
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
}

.inner {
  overflow-y: clip;
  overflow-x: scroll;

  scroll-snap-type: x mandatory;

  display: flex;
  flex-direction: row;

  padding-top: 0;
}

.innerFrame {
  flex-shrink: 0;
  padding-top: 15rem;
  scroll-snap-align: start;
  height: var(--page-height);
  width: var(--page-width);
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
