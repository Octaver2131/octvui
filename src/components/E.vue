<template>
  <div class="overlay">
    <div class="projects">
      <div class="projects-header">
        <p>Projects</p>
        <p>Directors</p>
      </div>
    </div>
    <div class="loader">
      <h1 class="logo-line-1">Nova</h1>
      <h1 class="logo-line-2">Vice</h1>
    </div>
    <div class="locations">
      <div class="locations-header">
        <p>Location</p>
      </div>
    </div>
  </div>
  <div class="image-grid">
    <div class="grid-row">
      <div class="img"><img src="/chainsawMan-1.jpeg" alt="" /></div>
      <div class="img"><img src="/chainsawMan-2.jpeg" alt="" /></div>
      <div class="img"><img src="/chainsawMan-3.jpeg" alt="" /></div>
    </div>
    <div class="grid-row">
      <div class="img"><img src="/chainsawMan-4.jpeg" alt="" /></div>
      <div class="img hero-img"><img src="/chainsawMan-5.jpeg" alt="" /></div>
      <div class="img"><img src="/chainsawMan-6.jpeg" alt="" /></div>
    </div>
    <div class="grid-row">
      <div class="img"><img src="/chainsawMan-7.jpeg" alt="" /></div>
      <div class="img"><img src="/chainsawMan-8.jpeg" alt="" /></div>
      <div class="img"><img src="/chainsawMan-9.jpeg" alt="" /></div>
    </div>
  </div>

  <nav>
    <div class="links">
      <a href="#">Index</a>
      <a href="#">Projects</a>
    </div>
    <div class="nav-logo">
      <a href="#">Nova<br />Vice</a>
    </div>
    <div class="links">
      <a href="#">About</a>
      <a href="#">Contact</a>
    </div>
  </nav>

  <div class="banner-img banner-img-1"><img src="/makima-vertical.png" alt="" /></div>
  <div class="banner-img banner-img-2"><img src="/makima-vertical.png" alt="" /></div>

  <div class="intro-copy">
    <h3>Creative Solutions</h3>
    <h3>Impactful Results</h3>
  </div>

  <div class="title">
    <h1>Crafting Bold Experience</h1>
  </div>
</template>

<script setup lang="ts">
import gsap from "gsap";
import { CustomEase } from "gsap/CustomEase";
import SplitType from "split-type";
import { projectsData } from "../projects.js";

gsap.registerPlugin(CustomEase);
CustomEase.create("hop", "0.9, 0, 0.1, 1");

document.addEventListener("DOMContentLoaded", () => {
  const projectsContainer = document.querySelector(".projects");
  const locationsContainer = document.querySelector(".locations");
  const gridImages = gsap.utils.toArray(".img");
  const heroImage = document.querySelector(".hero-img");

  const images = gridImages.filter((img) => img !== heroImage);

  const introCopy = new SplitType(".intro-copy h3", {
    type: "words",
    absolute: false,
  });

  const titleHeading = new SplitType(".title h1", {
    type: "words",
    absolute: false,
  });

  const allImagesSource = Array.from(
      { length: 9 },
      (_, i) => `/chainsawMan-${i + 1}.jpeg`
  );

  const getRandomImageSet = () => {
    const shuffled = [...allImagesSource].sort(() => 0.5 - Math.random());
    return shuffled.slice(0, 9);
  };

  function initializeDynamicContent() {
    projectsData.forEach((project) => {
      const projectItem = document.createElement("div");
      projectItem.className = "project-item";

      const projectName = document.createElement("p");
      projectName.textContent = project.name;

      const directorName = document.createElement("p");
      directorName.textContent = project.director;

      projectItem.appendChild(projectName);
      projectItem.appendChild(directorName);

      projectsContainer.appendChild(projectItem);
    });

    projectsData.forEach((project) => {
      const locationItem = document.createElement("div");
      locationItem.className = "location-item";

      const locationName = document.createElement("p");
      locationName.textContent = project.location;

      locationItem.appendChild(locationName);

      locationsContainer.appendChild(locationItem);
    });
  }

  function startImageRotation() {
    const totalCycles = 20;

    for (let cycle = 0; cycle < totalCycles; cycle++) {
      const randomImages = getRandomImageSet();

      gsap.to({}, {
        duration: 0,
        delay: cycle * 0.15,
        onComplete: () => {
          gridImages.forEach((img, index) => {
            const imgElement = img.querySelector("img");

            if (cycle === totalCycles - 1 && img === heroImage) {
              imgElement.src = "/chainsawMan-5.jpeg"
              gsap.set(".hero-img img", { scale: 2 });
            } else {
              imgElement.src = randomImages[index];
            }
          });
        }
      })
    }
  }

  function setupInitialStates() {
    gsap.set("nav", {
      y: "-125%",
    });

    gsap.set("introCopy.words", {
      y: "110%",
    });

    gsap.set("titleHeading.words", {
      y: "110%",
    });
  }
  function init() {
    initializeDynamicContent();
    setupInitialStates();
    createAnimationTimelines();
  }

  init();

  function createAnimationTimelines() {
    const overlayTimeline = gsap.timeline();
    const imagesTimeline = gsap.timeline();
    const textTimeline = gsap.timeline();

    overlayTimeline.to(".logo-line-1", {
      backgroundPosition: "0% 0%",
      color: "#fff",
      duration: 1,
      ease: "none",
      delay: 0.5,
      onComplete: () => {
        gsap.to(".logo-line-2", {
          backgroundPosition: "0% 0%",
          color: "#fff",
          duration: 1,
          ease: "none",
        });
      },
    });

    overlayTimeline.to([".projects-header", ".project-item"], {
      opacity: 1,
      duration: 0.15,
      stagger: 0.075,
      delay: 1,
    });

    overlayTimeline.to([".locations-header", ".location-item"], {
      opacity: 1,
      duration: 0.15,
      stagger: 0.075,
    },
        "<"
    );

    overlayTimeline.to(".project-item", {
      color: "#fff",
      duration: 0.15,
      stagger: 0.075,
    });

    overlayTimeline.to(".location-item", {
      color: "#fff",
      duration: 0.15,
      stagger: 0.075,
    },
        "<"
    );

    overlayTimeline.to([".projects-header", ".project-item"], {
      opacity: 0,
      duration: 0.15,
      stagger: 0.075,
    });

    overlayTimeline.to([".locations-header", ".location-item"], {
      opacity: 0,
      duration: 0.15,
      stagger: 0.075,
    },
    "<"
    );

    overlayTimeline.to(".overlay", {
      opacity: 0,
      duration: 0.5,
      delay: 1.5,
    });

    overlayTimeline.to(".img", {
      clipPath: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
      duration: 1,
      delay: 2.5,
      stagger: 0.05,
      ease: "hop",
      onStart: () => {
        setTimeout(() => {
          startImageRotation();
          gsap.to(".loader", { opacity: 0, duration: 0.3 });
        }, 1000);
      },
    });

    imagesTimeline.to(images, {
      clipPath: "polygon(0% 0%, 100% 0%, 100% 0%, 0% 0%)",
      duration: 1,
      delay: 2.5,
      stagger: 0.05,
      ease: "hop",
    });

    imagesTimeline.to(".hero-img", {
      y: -50,
      duration: 1,
      ease: "hop",
    });

    imagesTimeline.to(".hero-img", {
      scale: 4,
      clipPath: "polygon(20% 10%, 80% 10%, 80% 90%, 20% 90%)",
      duration: 1.5,
      ease: "hop",
      onStart: () => {
        gsap.to(".hero-img img", {
          scale: 1,
          duration: 1.5,
          ease: "hop",
        });

        gsap.to(".banner-img", { scale: 1, delay: 0.5, duration: 0.5 });
        gsap.to("nav", { y: "0%", duration: 1, ease: "hop", delay: 0.25 });
      },
    });

    imagesTimeline.to(".banner-img-1", {
      left: "40%",
      rotate: -20,
      duration: 1.5,
      delay: 0.5,
      ease: "hop",
    },
        "<"
    );

    imagesTimeline.to(".banner-img-2", {
          left: "40%",
          rotate: 20,
          duration: 1.5,
          delay: 0.5,
          ease: "hop",
        },
        "<"
    );

    textTimeline.to(titleHeading.words, {
      y: "0%",
      duration: 1,
      stagger: 0.1,
      delay: 9.5,
      ease: "power3 out",
    });

    textTimeline.to(introCopy.words, {
      y: "0%",
      duration: 1,
      stagger: 0.1,
      delay: 0.25,
      ease: "power3 out",
    },
    "<"
  );
  }
});

</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "PP Neue Montreal", sans-serif;
  background-color: #fff;
}

img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

p {
  text-transform: uppercase;
  font-family: "Akkurat Mono", monospace;
  font-size: 0.7rem;
}

a {
  text-decoration: none;
  text-transform: uppercase;
  font-family: "Akkurat Mono", monospace;
  font-size: 0.7rem;
  color: #000;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100svh;
  padding: 2em;
  background-color: #000;
  color: #fff;
  display: flex;
  gap: 2em;
  overflow: hidden;
}

.projects,
.loader,
.locations {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 2em;
}

.loader {
  align-items: center;
  gap: 0;
}

.loader h1 {
  text-align: center;
  text-transform: uppercase;
  font-family: "Druk", sans-serif;
  font-size: 2.5rem;
  font-style: italic;
  line-height: 0.9;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  background-image: linear-gradient(0deg, #3a3a3a, #3a3a3a 50%, #fff 0);
  background-size: 100% 200%;
  background-position: 0% 100%;
  color: #3a3a3a;
}

.projects-header,
.project-item,
.locations-header,
.location-item {
  display: flex;
  gap: 2em;
  opacity: 0;
}

.projects-header > *,
.locations-header > * {
  flex: 1;
}

.locations {
  align-items: center;
}

.locations-header,
.location-item {
  width: 50%;
}

.project-item,
.location-item {
  color: #4f4f4f;
}

.image-grid {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 30%;
  aspect-ratio: 1;
  display: flex;
  flex-direction: column;
  gap: 1em;
  z-index: 2;
}

.grid-row {
  width: 100%;
  display: flex;
  gap: 1em;
}

.img {
  position: relative;
  flex: 1;
  aspect-ratio: 1;
  clip-path: polygon(0% 0%, 100% 0%, 100% 0%, 0% 0%);
}

nav {
  position: fixed;
  width: 100vw;
  padding: 1em;
  display: flex;
  gap: 2em;
}

nav > * {
  flex: 1;
}

.links {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.nav-logo {
  text-align: center;
  display: flex;
  justify-content: center;
}

.nav-logo a {
  font-family: "Druk", sans-serif;
  font-size: 1.75rem;
  font-weight: bolder;
  font-style: italic;
  line-height: 0.9;
}

.banner-img {
  position: absolute;
  top: 45%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0);
  width: 20%;
  aspect-ratio: 4/5;
}

.intro-copy {
  position: absolute;
  top: 45%;
  transform: translateY(-50%);
  width: 100%;
  padding: 0 8em;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.title {
  position: absolute;
  bottom: 10%;
  left: 50%;
  transform: translateX(-50%);
}

.intro-copy h3,
.title h1 {
  position: relative;
  text-transform: uppercase;
  color: #000;
  font-family: "Druk", sans-serif;
  font-weight: 500;
  font-style: italic;
  line-height: 0.9;
}

.title h1 {
  font-size: 3.5rem;
}

.intro-copy h3 {
  font-size: 1.5rem;
}

.intro-copy h3,
.title h1 {
  clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%);
}

.intro-copy h3 .word,
.title h1 .word {
  display: inline-block;
  position: relative;
  will-change: transform;
  margin-right: 0.1rem;
}

@media(min-width: 1000px) {
  .loader {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .projects,
  .locations,
  .intro-copy,
  .banner-img {
    display: none;
  }

  title {
    width: 100%;
    bottom: 20%;
    display: flex;
    justify-content: center;
  }

  .title h1 {
    font-size: 2.5rem;
  }

  .image-grid {
    width: 75%;
    gap: 0.5em;
  }

  .grid-row {
    width: 95%;
    justify-content: space-around;
    gap: 0.5em;
  }
}
</style>