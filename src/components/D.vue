<template>
  <section class="hero">
    <div class="hero-img"><img src="/makima-vertical.png" /></div>
  </section>

  <section class="about">
    <h1 class="animate-text">
      A space for work sharped with clarity and intention. Each project
      follows a simple path from thought to form, from form to function.
    </h1>
  </section>

  <section class="services">
    <div class="services-header"><img src="/whatido.svg" /></div>
    <div class="services-header"><img src="/whatido.svg" /></div>
    <div class="services-header"><img src="/whatido.svg" /></div>
  </section>

  <section class="services-copy">
    <h1 class="animate-text">
      I create website and digital experiences that value charity above
      excess. Through minimal form and precise detail, I aim to build work
      that lasts and offers a quiet sense of order.
    </h1>
  </section>

  <section class="outro">
    <div class="outro-img"><img src="/makima-vertical.png" /></div>
  </section>
</template>

<script setup lang="ts">
import gasp from "gsap";
import { ScrollTrigger} from "gsap/ScrollTrigger";
import Lenis from "lenis";

document.addEventListener("DOMContentLoaded", () => {
  gasp.registerPlugin(ScrollTrigger);

  const lenis = new Lenis();
  lenis.on("scroll", ScrollTrigger.update);
  gsap.ticker.add((time) => {
    lenis.raf(time * 1000);
  });
  gsap.ticker.lagSmoothing(0);

  document.querySelectorAll(".animate-text").forEach((textElement) => {
    textElement.setAttribute("data-text", textElement.textContent.trim());

    ScrollTrigger.create({
      trigger: textElement,
      start: "top 50%",
      end: "bottom 50%",
      scrub: 1,
      onUpdate: (self) => {
        const clipValue = Math.max(0, 100 - self.progress * 100);
        textElement.style.setProperty("--clip-value", `${clipValue}%`);
      },
    });
  });

  ScrollTrigger.create({
    trigger: ".services",
    start: "top bottom",
    end: "top top",
    scrub: 1,
    onUpdate: (self) => {
      const headers = document.querySelectorAll(".services-header");
      gsap.set(headers[0], { x: `${100 - self.progress * 100 }%`});
      gsap.set(headers[1], { x: `${-100 + self.progress * 100 }%`});
      gsap.set(headers[2], { x: `${100 - self.progress * 100 }%`});
    },
  });

  ScrollTrigger.create({
    trigger: ".services",
    start: "top top",
    end: `+=${window.innerHeight * 2}`,
    pin: true,
    scrub: 1,
    pinSpacing: false,
    onUpdate: (self) => {
      const headers = document.querySelectorAll(".services-header");

      if (self.progress <= 0.5) {
        const yProgress = self.progress / 0.5;
        gsap.set(headers[0], { y: `${yProgress * 100}%`});
        gsap.set(headers[2], { y: `${yProgress * -100}%`});
      } else {
        gsap.set(headers[0], { y: "100%" });
        gsap.set(headers[2], { y: "-100%" });

        const scaleProgress = (self.progress - 0.5) / 0.5;
        const minScale = window.innerHeight <= 1000 ? 0.3 : 0.1;
        const scale = 1 - scaleProgress * (1 - minScale);

        headers.forEach((header) =>  gsap.set(header, { scale }));
      }
    },
  });
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Manrope:wght@200..800&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Manrope", sans-serif;
  background-color: #000;
}

img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

h1 {
  font-size: 4rem;
  font-weight: 900;
  line-height: 1.125;
  text-align: center;
  max-width: 360px;
}

.hero,
.outro,
.about {
  position: relative;
  width: 100%;
  height: 100svh;
  padding: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

.hero-img,
.outro-img {
  width: 300px;
  aspect-ratio: 5/7;
  overflow: hidden;
}

.services {
  position: relative;
  width: 100%;
  height: 100svh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

.services-header {
  position: relative;
  width: 100%;
  padding: 0 2rem;
  background-color: #000;
  will-change: transform;
}

.services-header img {
  object-fit: contain;
}

.services-header:nth-child(1),
.services-header:nth-child(3) {
  transform: translatex(100%) translateY(0%);
}

.services-header:nth-child(2) {
  transform: translatex(-100%) translateY(0%);
  z-index: 2;
}

.services-copy {
  position: relative;
  width: 100%;
  height: 100%;
  margin-top: 155svh;
  padding: 2rem 2rem 25svh 2rem;
  text-align: center;
}

.animate-text {
  position: relative;
  width: 60%;
  margin: 0 auto;
  color: #4f4f4f;
  --clip-value: 100%;
}

.animate-text::before {
  content: attr(data-text);
  position: absolute;
  top: 0;
  left: 0;
  color: #fff;
  clip-path: inset(0 0 var(--clip-value) 0);
  will-change: clip-path;
}

@media (min-width: 1000px) {
  h1 {
    font-size: 2rem;
    letter-spacing: -0.05rem;
  }

  .animate-text {
    width: 100%;
  }
}
</style>