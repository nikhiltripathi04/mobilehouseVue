<template>
  <footer class="mt-20 border-t pt-12 pb-8 bg-white">
    <div class="container mx-auto px-4">
      <div class="grid md:grid-cols-4 gap-10">
        <!-- LOGO + TAGLINE -->
        <div>
          <div class="flex items-center gap-2 mb-3">
            <img src="/images/mhlogo.jpg" class="h-8 w-auto" alt="Mobile House Logo" />
            <span class="text-2xl font-semibold text-black tracking-tight">Mobile House</span>
          </div>
          <p class="text-sm text-gray-600 leading-relaxed">
            Premium smartphones. Honest recommendations. Seamless upgrades.
          </p>
        </div>

        <!-- NAVIGATION -->
        <div>
          <h4 class="footer-heading text-logo-red">Explore</h4>
          <ul class="footer-list">
            <!-- Section links (Home scroll) -->
            <li>
              <a href="#about" class="footer-link" @click.prevent="goToSection('about')">About</a>
            </li>
            <!-- Routed pages -->
            <li>
              <RouterLink to="/mobile" class="footer-link">Mobile</RouterLink>
            </li>
            <li>
              <RouterLink to="/accessories" class="footer-link">Accessories</RouterLink>
            </li>
            <!-- Section links (Home scroll) -->
            <li>
              <a href="#news" class="footer-link" @click.prevent="goToSection('news')">News</a>
            </li>
            <li>
              <a href="#contact" class="footer-link" @click.prevent="goToSection('contact')">Contact</a>
            </li>
          </ul>
        </div>

        <!-- CONTACT -->
        <div>
          <h4 class="footer-heading text-logo-red">Contact</h4>
          <ul class="footer-list text-sm text-gray-700">
            <li>{{ addressLine1 }}</li>
            <li>{{ addressLine2 }}</li>
            <li class="mt-2">
              <a :href="`tel:${phone}`" class="footer-link underline">+91 9953335567</a>
            </li>
            <li>
              <a :href="`mailto:${email}`" class="footer-link underline">mobilehousemhnoida@gmail.com</a>
            </li>
          </ul>
        </div>

        <!-- SOCIAL -->
        <div>
          <h4 class="footer-heading text-logo-red">Follow us</h4>
          <div class="flex gap-4 text-xl mt-2">
            <a href="https://www.instagram.com/mobilehousenoida/?hl=en" target="_blank" class="social-icon"><i class="lab la-instagram"></i></a>
            <a href="https://www.facebook.com/mobilehousenoida18/" target="_blank" class="social-icon"><i class="lab la-facebook-f"></i></a>

          </div>
        </div>
      </div>

      <div class="text-center text-xs text-gray-600 mt-12">
        © {{ new Date().getFullYear() }} Mobile House · All rights reserved.
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
import { useRoute, useRouter } from 'vue-router'

const props = withDefaults(defineProps<{
  addressLine1?: string
  addressLine2?: string
  phone?: string
  email?: string
  instagram?: string
  facebook?: string
  twitter?: string
}>(), {
  addressLine1: "Mobile House, Hazratganj",
  addressLine2: "Lucknow, Uttar Pradesh",
  phone: "+91-98XXXXXXX",
  email: "hello@mobilehouse.in",
  instagram: "#",
  facebook: "#",
  twitter: "#"
})

const router = useRouter()
const route = useRoute()

function goToSection(id: string) {
  // If already on Home, smooth-scroll to the element
  if (route.path === '/' || route.name === 'home') {
    const el = document.getElementById(id)
    if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' })
    return
  }
  // Otherwise navigate Home with hash; your router.scrollBehavior will handle scrolling
  router.push({ path: '/', hash: `#${id}` })
}
</script>

<style scoped>
:root { --logo-red: #E20015; }
.text-logo-red { color: var(--logo-red); }

.footer-heading { @apply text-sm font-semibold tracking-wide mb-3; }
.footer-list { @apply text-sm space-y-2; }
.footer-link { @apply opacity-70 hover:opacity-100 transition; color: #000; }
.footer-link:hover { color: var(--logo-red); }

.social-icon { color: #000; transition: color .3s ease; }
.social-icon:hover { color: var(--logo-red); }
</style>
