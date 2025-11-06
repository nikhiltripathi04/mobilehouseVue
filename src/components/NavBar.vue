<template>
  <header class="fixed top-0 left-0 w-full z-[100] bg-white/80 backdrop-blur border-b">
    <nav class="container mx-auto px-4 flex justify-between mt-0 h-16 items-center">

      <!-- ✅ Clickable Logo -->
      <RouterLink to="/" class="logo flex items-center gap-2 hover:opacity-90 transition">
        <img src="/images/mhlogo.jpg" alt="Mobile House Logo" class="h-8 w-auto select-none" />
        <span class="text-2xl font-semibold tracking-tight">Mobile House</span>
      </RouterLink>

      <div class="menu-links text-xs lg:text-sm hidden lg:flex">
        <ul class="flex gap-4">
          <li v-for="(item, i) in items" :key="i">
            <RouterLink
              v-if="isRouted(item)"
              :to="routeFor(item)"
              class="transition-all"
              :class="[{ 'opacity-100': item === active, 'opacity-70 hover:opacity-100': item !== active }]"
            >
              {{ item }}
            </RouterLink>

            <button
              v-else
              class="transition-all bg-transparent"
              :class="[{ 'opacity-100': item === active, 'opacity-70 hover:opacity-100': item !== active }]"
              @click="handleClick(item)"
            >
              {{ item }}
            </button>
          </li>
        </ul>
      </div>

    </nav>
  </header>
</template>


<script setup lang="ts">
import { onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import { gsap } from 'gsap'

const props = withDefaults(defineProps<{
  items?: string[]
  active?: string
  showCart?: boolean
}>(), {
  items: ['about', 'mobile', 'accessories', 'news', 'contact'],
  active: '',
  showCart: true
})

const router = useRouter()
const route = useRoute()

// --- Routing / Scroll helpers ---
const routeMap: Record<string, string> = {
  mobile: '/mobile',
  accessories: '/accessories'
}
const sectionIdMap: Record<string, string> = {
  about: 'about',
  news: 'news',
  contact: 'contact'
}

// Define available routes
const availableRoutes = ['mobile', 'accessories']

function isRouted(item: string) {
	return Object.prototype.hasOwnProperty.call(routeMap, item.toLowerCase())
}

function routeFor(item: string) {
	return routeMap[item.toLowerCase()]
}

function handleClick(item: string) {
	const key = item.toLowerCase()
	if (availableRoutes.includes(key)) {
		router.push(routeMap[key])
		return
	}

	const id = sectionIdMap[key]
	if (!id) return

	// If already on home, smooth scroll
	if (route.path === '/' || route.name === 'home') {
		const el = document.getElementById(id)
		if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' })
		return
	}

	// If on another page, go home with hash; scrollBehavior will handle it
	router.push({ path: '/', hash: `#${id}` })
}

// --- Animations ---
onMounted(() => {
	gsap.from('.logo', { duration: 1, opacity: 0, x: -20, ease: 'Expo.easeInOut' })
	gsap.from('.menu-links ul li', {
		opacity: 0,
		x: -20,
		ease: 'Power3.easeInOut',
		stagger: 0.08,
		duration: 1
	})
	gsap.from('.cart', { delay: 0.7, duration: 1, opacity: 0, x: -20, ease: 'Expo.easeInOut' })
	gsap.from('.logo img', { duration: 1, opacity: 0, scale: 0.8, ease: 'Expo.easeOut' })

})
</script>

<style scoped>
.center { display: grid; place-items: center; }
.menu-links ul li { display: inline-block; padding-right: 20px; }
.logo img {
  transition: transform 0.3s ease;
}
.logo:hover img {
  transform: scale(1.05);
}

</style>
