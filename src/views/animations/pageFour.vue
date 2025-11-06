<template>
  <div class="w-screen overflow-x-hidden">
    <div class="container mx-auto px-4">
      <nav class="flex justify-between mt-4 container mx-auto items-center">
        <div class="logo text-2xl">Mobile House</div>

        <div class="menu-links text-xs lg:text-sm hidden lg:flex">
          <ul>
            <li>about</li>
            <li>mobile</li>
            <li>accessories</li>
            <li>news</li>
            <li>contact</li>
            <li>own a phone</li>
          </ul>
        </div>

        <div class="cart bg-black p-2 text-white h-8 w-8 center rounded-full">
          <i class="i las la-shopping-cart"></i>
        </div>
      </nav>

      <div class="relative h-screen">
        <!-- PAGE 1 -->
        <div :class="['page', { 'is-active': currentPage === 1 }]" class="absolute w-full" data-page="1">
          <main class="flex flex-wrap items-center mt-12 justify-center">
            <div class="tagline lg:text-3xl text-lg">Apple</div>
            <div class="pages"><span>1</span>/4</div>
            <div class="title text-center">Iphone 17</div>
            <div class="more hidden md:block"><a href="#">specifications</a></div>
            <div class="desc hidden md:block">
              <p>
                Sleek. <span>Smarter. </span> Simply futuristic. <br>
              </p>
            </div>
          </main>
          <div class="juice">
            <img src="/images/iphone17.png" alt="Iphone 17">
          </div>
        </div>

        <!-- PAGE 2 -->
        <div :class="['page', { 'is-active': currentPage === 2 }]" class="absolute w-full" data-page="2">
          <main class="flex flex-wrap items-center mt-12 justify-center">
            <div class="tagline lg:text-3xl text-lg">Samsung</div>
            <div class="pages"><span>2</span>/4</div>
            <div class="title text-center">Galaxy S25</div>
            <div class="more hidden md:block"><a href="#">specifications</a></div>
            <div class="desc hidden md:block">
              <p>
                Sleek. <span>Smarter. </span> Simply futuristic. <br>
              </p>
            </div>
          </main>
          <div class="juice">
            <img src="/images/s25.png" alt="Samsung S25">
          </div>
        </div>

        <!-- PAGE 3 -->
        <div :class="['page', { 'is-active': currentPage === 3 }]" class="absolute w-full" data-page="3">
          <main class="flex flex-wrap items-center mt-12 justify-center">
            <div class="tagline lg:text-3xl text-lg">Google</div>
            <div class="pages"><span>3</span>/4</div>
            <div class="title text-center">Pixel 10</div>
            <div class="more hidden md:block"><a href="#">specifications</a></div>
            <div class="desc hidden md:block">
              <p>
                Sleek. <span>Smarter. </span> Simply futuristic. <br>
              </p>
            </div>
          </main>
          <div class="juice">
            <img src="/images/pixel10.webp" alt="Pixel 10">
          </div>
        </div>

        <!-- PAGE 4 -->
        <div :class="['page', { 'is-active': currentPage === 4 }]" class="absolute w-full" data-page="4">
          <main class="flex flex-wrap items-center mt-12 justify-center">
            <div class="tagline lg:text-3xl text-lg">OnePlus</div>
            <div class="pages"><span>4</span>/4</div>
            <div class="title text-center">OnePlus 13</div>
            <div class="more hidden md:block"><a href="#">specifications</a></div>
            <div class="desc hidden md:block">
              <p>
                Sleek. <span>Smarter. </span> Simply futuristic. <br>
              </p>
            </div>
          </main>
          <div class="juice">
            <img src="/images/oneplus13.webp" alt="OnePlus 13">
          </div>
        </div>
      </div>

      <div class="leaves !z-50">
        <ul id="scene">
          <li class="layer" data-depth="-.1">
            <img src="@/assets/images/pageFour/leaf01.png" alt="">
          </li>
          <li class="layer" data-depth="-.3">
            <img src="@/assets/images/pageFour/leaf02.png" alt="">
          </li>
          <li class="layer" data-depth="-1.5">
            <img src="@/assets/images/pageFour/leaf03.png" alt="">
          </li>
          <li class="layer" data-depth=".1">
            <img src="@/assets/images/pageFour/leaf04.png" alt="">
          </li>
          <li class="layer" data-depth=".3">
            <img src="@/assets/images/pageFour/leaf05.png" alt="">
          </li>
        </ul>
      </div>

      <div class="arrows w-full lg:block">
        <button @click="changePage('prev')" class="prev md:left-24 left-4"><i class="las la-chevron-left"></i></button>
        <button @click="changePage('next')" class="next md:right-24 right-4"><i class="las la-chevron-right"></i></button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import Parallax from 'parallax-js'
import { onMounted, ref } from 'vue'
import { gsap } from 'gsap'
import { setTitle } from '@/composables/utils'

setTitle('Animation Four')

const currentPage = ref(1)

const changePage = (direction: 'next' | 'prev') => {
  const max = 4
  const min = 1
  if ((direction === 'next' && currentPage.value >= max) ||
      (direction === 'prev' && currentPage.value <= min)) return

  const current = currentPage.value
  const next = direction === 'next' ? current + 1 : current - 1

  const currentSel = `.page[data-page="${current}"]`
  const nextSel    = `.page[data-page="${next}"]`

  const xFrom = direction === 'next' ? 100 : -100
  const xTo   = direction === 'next' ? -100 : 100

  // Animate out the current page
  gsap.to(currentSel, {
    opacity: 0,
    x: xTo,
    duration: 0.5,
    ease: 'Power2.easeIn',
    onComplete: () => {
      // Swap active class after fade-out
      const curEl = document.querySelector(currentSel)
      const nextEl = document.querySelector(nextSel)
      curEl?.classList.remove('is-active')
      nextEl?.classList.add('is-active')

      // Prepare and animate the next page in
      gsap.fromTo(nextSel, { opacity: 0, x: xFrom }, {
        opacity: 1,
        x: 0,
        duration: 0.5,
        ease: 'Power2.easeOut'
      })

      currentPage.value = next
    }
  })
}

onMounted(() => {
  const scene = document.getElementById('scene')
  if (scene) new Parallax(scene)

  // Ensure the first page is visible for initial animations
  gsap.set('.page.is-active', { opacity: 1, x: 0 })

  gsap.from('.logo', { duration: 1, opacity: 0, x: -20, ease: 'Expo.easeInOut' })

  gsap.from('.menu-links ul li', {
    opacity: 0,
    x: -20,
    ease: 'Power3.easeInOut',
    stagger: 0.08,
    duration: 1
  })

  gsap.from('.cart', { delay: 0.7, duration: 1, opacity: 0, x: -20, ease: 'Expo.easeInOut' })

  gsap.from('.juice', { delay: 2, duration: 1, opacity: 0, y: -800, ease: 'Expo.easeInOut' })

  gsap.from('.leaves .layer:nth-child(1)', { delay: 2,   duration: 2, opacity: 0, y: -800, ease: 'Expo.easeInOut' })
  gsap.from('.leaves .layer:nth-child(2)', { delay: 2.1, duration: 2, opacity: 0, y: -800, ease: 'Expo.easeInOut' })
  gsap.from('.leaves .layer:nth-child(3)', { delay: 2.2, duration: 2, opacity: 0, y: -800, ease: 'Expo.easeInOut' })
  gsap.from('.leaves .layer:nth-child(4)', { delay: 2.3, duration: 2, opacity: 0, y: -800, ease: 'Expo.easeInOut' })
  gsap.from('.leaves .layer:nth-child(5)', { delay: 2.5, duration: 2, opacity: 0, y: -800, ease: 'Expo.easeInOut' })

  gsap.from('.title',  { delay: 1,   duration: 1, opacity: 0, y: 20, ease: 'Expo.easeInOut' })
  gsap.from('.tagline',{ delay: 1.3, duration: 1, opacity: 0, y: 20, ease: 'Expo.easeInOut' })
  gsap.from('.pages',  { delay: 1.3, duration: 1, opacity: 0, y: 20, ease: 'Expo.easeInOut' })
  gsap.from('.more',   { delay: 1.4, duration: 1, opacity: 0, y: 20, ease: 'Expo.easeInOut' })
  gsap.from('.desc',   { delay: 1.4, duration: 1, opacity: 0, y: 20, ease: 'Expo.easeInOut' })

  gsap.from('.arrows', { delay: 2,   duration: 1, opacity: 0, ease: 'Expo.easeInOut' })
})
</script>

<style scoped>
body {
  width: 100%;
  height: 100vh;
  font-family: 'Montserrat';
  font-size: 12px;
  overflow: hidden;
}

ul { list-style: none; }

.menu-links ul li { display: inline-block; padding-right: 20px; }

.title {
  flex: 0 0 100%;
  font-size: 15vw;
  text-transform: uppercase;
  font-weight: 700;
}

.tagline { flex: 1; color: #999; }

.pages { flex: 0; letter-spacing: 5px; color: #999; }

.pages span { font-size: 60px; color: #000; font-weight: 600; }

.more { flex: 1; }

.more a {
  text-decoration: none;
  font-size: 20px;
  color: #fff;
  background: #000;
  padding: 10px 30px;
  border-radius: 10px;
}

.desc { flex: 0 0 32%; }
.desc p:nth-child(1) { font-size: 30px; margin-bottom: 20px; }
.desc p:nth-child(2) { line-height: 2; }
.desc span { color: #AECD31; }

/* Updated page visibility logic: no v-show */
.page {
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
  /* absolutely positioned via template classes */
}
.page.is-active {
  opacity: 1;
  pointer-events: auto;
}

.juice {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1;
}
.juice img {
  animation: float 4s ease-in-out infinite;
  max-height: 500px;
}

@keyframes float {
  0%   { transform: translate(-50%, -46%); }
  50%  { transform: translate(-50%, -54%); }
  100% { transform: translate(-50%, -46%); }
}

.leaves {
  position: absolute;
  top: 50%;
  left: 50%;
}

.leaves img {
  max-width: 100%;
  max-height: 100%;
}

.leaves .layer:nth-child(1) { top: -100px !important; left: -480px !important; }
.leaves .layer:nth-child(2) { top: 10px !important;   left: 160px !important; }
.leaves .layer:nth-child(3) { top: -300px !important; left: 160px !important; }
.leaves .layer:nth-child(4) { top: -10px !important;  left: 320px !important; }
.leaves .layer:nth-child(5) { top: 200px !important;  left: -320px !important; }

.arrows .prev,
.arrows .next {
  position: absolute;
  top: 50%;
}

.arrows button {
  border: 1px solid #999;
  background: transparent;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  outline: none;
}
.arrows button:hover {
  color: #fff;
  background: #000;
}
</style>
