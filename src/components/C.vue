<template>
  <nav>
    <div class="menu-bar">
      <div class="menu-logo">
        <a href="#"><img src="/logo.svg" alt=""></a>
      </div>
      <div class="menu-toggle-btn">
        <div class="menu-toggle-label"><p>Menu</p></div>
        <div class="menu-hamburger-icon">
          <span></span>
          <span></span>
        </div>
      </div>
    </div>
    <div class="menu-overlay">
      <div class="menu-overlay-content">
        <div class="menu-media-wrapper">
          <img src="/makima.png" alt="" />
        </div>
        <div class="menu-content-wrapper">
          <div class="menu-content-main">
            <div class="menu-col">
              <div class="menu-link"><a href="#">Index</a></div>
              <div class="menu-link"><a href="#">Portfolio</a></div>
              <div class="menu-link"><a href="#">Studio</a></div>
              <div class="menu-link"><a href="#">Journal</a></div>
              <div class="menu-link"><a href="#">Connect</a></div>
            </div>
            <div class="menu-col">
              <div class="menu-tag"><a href="#">Web Animation</a></div>
              <div class="menu-tag"><a href="#">Interactive Media</a></div>
              <div class="menu-tag"><a href="#">Motion Craft</a></div>
            </div>
          </div>
          <div class="menu-footer">
            <div class="menu-col">
              <p>Octaver, Chinese</p>
            </div>
            <div class="menu-col">
              <p>+86 123456789</p>
              <p>Octaver2131@outlook.com</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
  <div class="container">
    <section class="hero">
      <h1>Modern design system made that looks timeless</h1>
    </section>
    <section class="banner">
      <img src="/makima.png" alt="" />
    </section>
    <section class="outro">
      <h1>Let's build something quietly iconic</h1>
    </section>
  </div>
</template>

<script setup lang="ts">
import gsap from "gsap";
import { CustomEase } from "gsap/CustomEase";
import { SplitText} from "gsap/SplitText";
import Lenis from "lenis";

document.addEventListener("DOMContentLoaded", () => {
  gsap.registerPlugin(CustomEase, SplitText);
  CustomEase.create("hop", ".87, 0, .13, 1");

  const lenis = new Lenis();
  function raf(time) {
    lenis.raf(time);
    requestAnimationFrame(raf);
  }
  requestAnimationFrame(raf);

  const textContainers = document.querySelectorAll(".menu-col");
  let splitTextByContainer = [];

  textContainers.forEach((container) => {
    const textElements = container.querySelectorAll("a, p");
    let containerSplits = [];

    textElements.forEach((element) => {
      const split = SplitText.create(element, {
        type: "lines",
        mask: "lines",
        linesClass: "line"
      });
      containerSplits.push(split);

      gsap.set(split.lines, { y: "-110%" });
    });

    splitTextByContainer.push(containerSplits);
  });

  const container = document.querySelector(".container");
  const menuToggleBtn = document.querySelector(".menu-toggle-btn");
  const menuOverlay = document.querySelector(".menu-overlay");
  const menuOverlayContainer = document.querySelector(".menu-overlay-content");
  const menuMediaWrapper = document.querySelector(".menu-media-wrapper");
  const copyContainers = document.querySelectorAll(".menu-col");
  const menuToggleLabel = document.querySelector(".menu-toggle-label p")
  const hamburgerIcon = document.querySelector(".menu-hamburger-icon");

  let isMenuOpen = false;
  let isAnimating = false;

  menuToggleBtn.addEventListener("click", () => {
    if (isAnimating) return;

    if (!isMenuOpen) {
      isAnimating = true;

      lenis.stop();

      const tl = gsap.timeline();

      tl.to(menuToggleLabel, {
        y: "-110%",
        duration: 1,
        ease: "hop",
      }).to(container, {
        y: "100svh",
        duration: 1,
        ease: "hop",
      },
      "<"
      ).to(menuOverlay, {
        clipPath: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
        duration: 1,
        ease: "hop",
      },
      "<"
      ).to(menuOverlayContainer, {
        yPercent: 0,
        duration: 1,
        ease: "hop",
      },
      "<"
      ).to(menuMediaWrapper, {
        opacity: 1,
        duration: 0.75,
        ease: "power2.out",
        delay: 0.5,
      },
      "<"
      );


      splitTextByContainer.forEach((containerSplits) => {
        const copyLines = containerSplits.flatMap((split) => split.lines);
        tl.to(copyLines, {
          y: "0%",
          duration: 2,
          ease: "hop",
          stagger: -0.075,
        },
        -0.15
        );
      });

      hamburgerIcon.classList.add("active");

      tl.call(() => {
        isAnimating = false;
      });

      isMenuOpen = true;
    } else {
      isAnimating = true;

      hamburgerIcon.classList.remove("active");
      const tl = gsap.timeline();

      tl.to(container, {
        y: "0svh",
        duration: 1,
        ease: "hop",
      }).to(menuOverlay, {
        clipPath: "polygon(0% 0%, 100% 0%, 100% 0%, 0% 0%)",
        duration: 1,
        ease: "hop",
      },
      "<"
      ).to(menuOverlayContainer, {
        yPercent: -50,
        duration: 1,
        ease: "hop",
      },
      "<"
      ).to(menuToggleLabel, {
        y: "0%",
        duration: 1,
        ease: "hop",
      },
      "<"
      ).to(copyContainers, {
        opacity: 0.25,
        duration: 1,
        ease: "hop",
      },
      "<"
      );

      tl.call(() => {
        splitTextByContainer.forEach((containerSplits) => {
          const copyLines = containerSplits.flatMap((split) => split.lines);
          gsap.set(copyLines, { y: "-110%" })
        });

        gsap.set(copyContainers, { opacity: 1});
        gsap.set(menuMediaWrapper, { opacity: 0});

        isAnimating = false;
        lenis.start();
      });
      isMenuOpen = false;
    }
  });
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,100..1000;1,9..40,100..1000&display=swap");

:root {
  --bg: #000;
  --fg: #fff;
  --menu-bg: #0f0f0f;
  --menu-fg-secondary: #5f5f5f;
  --hamburger-icon-border: rgba(255, 255, 255, 0.1);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "DM Sans", sans-serif;
  background-color: #000;
  color: #fff;
  width: 100%;
  height: 100%;
  overflow-x: hidden;
}

img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

h1 {
  font-size: 7.5rem;
  font-weight: 500;
  letter-spacing: -0.2rem;
  line-height: 1;
}

p {
  font-size: 0.95rem;
  font-weight: 500;
}

a {
  text-decoration: none;
  color: var(--fg);
  font-size: 1.5rem;
  font-weight: 500;
}

.container {
  position: relative;
  transform: translateY(0svh);
  background-color: var(--bg);
  color: var(--fg);
  width: 100%;
  min-height: 100vh;
}
section {
  position: relative;
  width: 100%;
  height: 100svh;
  padding: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  z-index: -1;
}

section h1 {
  width: 75%;
}

section img {
  opacity: 0.5;
}

nav {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100svh;
  pointer-events: none;
  overflow: hidden;
  z-index: 2;
}

.menu-bar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  padding: 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  pointer-events: all;
  color: var(--menu-fg-secondary);
  z-index: 2;
}

.menu-logo {
  width: 2rem;
  height: 2rem;
}

.menu-toggle-btn {
  display: flex;
  align-items: center;
  gap: 1rem;
  cursor: pointer;
}

.menu-toggle-label {
  overflow: hidden;
}

.menu-toggle-label p {
  position: relative;
  transform: translateY(0%);
  will-change: transform;
}

.menu-hamburger-icon {
  position: relative;
  width: 3rem;
  height: 3rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 1px solid var(--hamburger-icon-border);
  border-radius: 100%;
}

.menu-hamburger-icon span {
  position: absolute;
  width: 15px;
  height: 1.25px;
  background-color: var(--fg);
  transition: all 0.75s cubic-bezier(0.87, 0, 0.13, 1);
  transform-origin: center;
  will-change: transform;
}

.menu-hamburger-icon span:nth-child(1) {
  transform: translateY(-3px);
}

.menu-hamburger-icon span:nth-child(2) {
  transform: translateY(3px);
}

.menu-hamburger-icon.active span:nth-child(1) {
  transform: translateY(0) rotate(45deg) scaleX(1.05);
}

.menu-hamburger-icon.active span:nth-child(2) {
  transform: translateY(0) rotate(-45deg) scaleX(1.05);
}

.menu-overlay,
.menu-overlay-content {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100svh;
  color: var(--fg);
  overflow: hidden;
  z-index: 1;
}

.menu-overlay {
  background-color: var(--menu-bg);
  clip-path: polygon(0% 0%,100% 0%,100% 0%,0% 0%);
  will-change: clip-path;
}

.menu-overlay-content {
  display: flex;
  transform: translateY(-50%);
  will-change: transform;
  pointer-events: all;
}

.menu-media-wrapper {
  flex: 2;
  opacity: 0;
  will-change: opacity;
}

.menu-media-wrapper img {
  opacity: 0.25;
}

.menu-content-wrapper {
  flex: 3;
  position: relative;
  display: flex;
}

.menu-content-main {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.menu-footer {
  margin: 0 auto;
}

.menu-content-main,
.menu-footer {
  width: 75%;
  padding: 2rem;
  display: flex;
  align-items: flex-end;
  gap: 2rem;
}

.menu-col {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.menu-col:nth-child(1) {
  flex: 3;
}

.menu-col:nth-child(2) {
  flex: 2;
}

.menu-link a {
  font-size: 3.5rem;
  font-weight: 500;
  line-height: 1.2;
}

.menu-tag a,
.menu-footer p {
  color: var(--menu-fg-secondary);
}

.line {
  position: relative;
  will-change: transform;
}

@media (max-width: 1000px) {
  h1 {
    font-size: 3rem;
    letter-spacing: -0.05rem;
  }

  section h1 {
    width: 100%;
  }

  .menu-media-wrapper {
    display: none;
  }

  .menu-content-main,
  .menu-footer {
    width: 100%;
  }

  .menu-content-main {
    top: 50%;
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>