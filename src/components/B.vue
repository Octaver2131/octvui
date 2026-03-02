<template>
  <section class="intro">
    <p>A collection of selected works.</p>
  </section>
  <section class="spotlight">
    <div class="project-index">
      <h1>01/10</h1>
    </div>
    <div class="project-images">
      <div class="project-image"><img src="/makima.png" alt=""></div>
      <div class="project-image"><img src="/makima.png" alt=""></div>
      <div class="project-image"><img src="/makima.png" alt=""></div>
      <div class="project-image"><img src="/makima.png" alt=""></div>
      <div class="project-image"><img src="/makima.png" alt=""></div>
      <div class="project-image"><img src="/makima.png" alt=""></div>
      <div class="project-image"><img src="/makima.png" alt=""></div>
      <div class="project-image"><img src="/makima.png" alt=""></div>
      <div class="project-image"><img src="/makima.png" alt=""></div>
      <div class="project-image"><img src="/makima.png" alt=""></div>
    </div>
    <div class="project-names">
      <p>Human Form Study</p>
      <p>Interior Light</p>
      <p>Project 21</p>
      <p>Shadow Portraits</p>
      <p>Everyday Objects</p>
      <p>Unit 07 Care</p>
      <p>Motion Practice</p>
      <p>Moonlight Series</p>
      <p>Material Stillness</p>
      <p>Quiet Walk</p>
    </div>
  </section>
  <section class="outro">
    <p>Scroll complete</p>
  </section>
</template>

<script setup lang="ts">
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import Lenis from "lenis";

gsap.registerPlugin(ScrollTrigger);

document.addEventListener("DOMContentLoaded", () => {
  const lenis = new Lenis();
  lenis.on("scroll", ScrollTrigger.update);
  gsap.ticker.add((time) => {
    lenis.raf(time * 1000);
  });
  gsap.ticker.lagSmoothing(0);

  const spotlightSection = document.querySelector(".spotlight");
  const projectIndex = document.querySelector(".project-index h1");
  const projectImages = document.querySelectorAll(".project-image");
  const projectImagesContainer = document.querySelector(".project-images");
  const projectNames = document.querySelectorAll(".project-names p");
  const projectNamesContainer = document.querySelector(".project-names");
  const totalProjectCount = projectImages.length;

  const spotlightSectionHeight = spotlightSection.offsetHeight;
  const spotlightSectionPadding = parseInt(
      getComputedStyle(spotlightSection).padding,
  );
  const projectIndexHeight = projectIndex.offsetHeight;
  const containerHeight = projectNamesContainer.offsetHeight;
  const imagesHeight = projectImagesContainer.offsetHeight;

  const moveDistanceIndex =
      spotlightSectionHeight - spotlightSectionPadding * 2 -
      projectIndexHeight;
  const moveDistanceNames =
      spotlightSectionHeight - spotlightSectionPadding * 2 -
      containerHeight;
  const moveDistanceImages =
      window.innerHeight - imagesHeight;

  const imageActivationThreshold = window.innerHeight / 2;

  ScrollTrigger.create({
    trigger: ".spotlight",
    start: "top top",
    end: `+=${window.innerHeight * 5}px`,
    pin: true,
    pinSpacing: true,
    scrub: 1,
    onUpdate: (self) => {
      const progress = self.progress;
      const currentIndex = Math.min(
          Math.floor(progress * totalProjectCount) + 1,
          totalProjectCount
      );

      projectIndex.textContent = `${String(currentIndex).padStart(
          2,
          "0"
      )}/${String(totalProjectCount).padStart(2, "0")}`;

      gsap.set(projectIndex, {
        y: progress * moveDistanceIndex,
      });

      gsap.set(projectImagesContainer, {
        y: progress * moveDistanceImages,
      });

      projectImages.forEach((image) => {
        const imageRect = image.getBoundingClientRect();
        const imageTop = imageRect.top;
        const imageBottom = imageRect.bottom;

        if (
            imageTop <= imageActivationThreshold &&
            imageBottom >= imageActivationThreshold
        ) {
          gsap.set(image, {
            opacity: 1,
          });
        } else {
          gsap.set(image, {
            opacity: 0.5,
          });
        }
      });

      projectNames.forEach((p, index) => {
        const startProgress = index / totalProjectCount;
        const endProgress = (index + 1) / totalProjectCount;
        const projectProgress = Math.max(
            0,
            Math.min(
                1,
                (progress - startProgress) / (endProgress - startProgress),
            ),
        );

        gsap.set(p, {
          y: -projectProgress * moveDistanceNames,
        });

        if (projectProgress > 0 && projectProgress < 1) {
          gsap.set(p, {
            color: "#fff",
          });
        } else {
          gsap.set(p, {
            color: "#4a4a4a",
          });
        }
      });
    },
  });
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,100..1000;1,9..40,100..1000&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "DM Sans", sans-serif;
  background-color: #000;
  color: #fff;
}

img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

h1 {
  text-transform: uppercase;
  font-size: clamp(3rem, 5vw, 7rem);
  font-weight: 400;
  line-height: 1;
}

p {
  font-size: 1.5rem;
  font-weight: 500;
  line-height: 1.25;
}

section {
  position: relative;
  width: 100%;
  height: 100svh;
  padding: 2rem;
  overflow: hidden;
}

.intro,
.outro {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.project-images {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 35%;
  padding: 50svh 0;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  z-index: -1;
}

.project-image {
  width: 100%;
  aspect-ratio: 16/9;
  opacity: 0.5;
  transition: all 0.3s;
  overflow: hidden;
}

.project-names {
  position: absolute;
  right: 2rem;
  bottom: 2rem;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
}

.project-names p {
  color: #4a4a4a;
  transition: color 0.3s ease;
}

.project-index h1,
.project-images,
.project-names p {
  will-change: transform;
}

@media (max-width: 1000px) {
  .project-images {
    width: calc(100% - 4rem);
    gap: 25svh;
  }

  .project-names p{
    color: #fff !important;
  }
}
</style>