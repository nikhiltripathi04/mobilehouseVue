<template>
	<NavBar />
	<br>
	<br>
	<br>
    <Home />
  <About id="about" /> 
  <News id="news" />
  <CTA id="contact"
    address-line1="Mobile House, G-29, Savitra Market"
    address-line2="Sector-18, Noida 201301"
    email="mobilehousemhnoida@gmail.com"
    phone="+91 9953335567"
    whatsapp="+91 9953335567"
    :lat="28.5700772" :lng="77.3250106"
    submit-endpoint="/api/contact"
  />
  <Footer />
</template>

<script setup lang="ts">
import Parallax from 'parallax-js'
import { onMounted, ref } from 'vue'
import { gsap } from 'gsap'
import NavBar from '@/components/NavBar.vue'
import Home from '@/pages/Home.vue'
import About from '@/pages/About.vue'
import Mobile from '@/pages/Mobile.vue'
import Accessories from '@/pages/Accessories.vue'
import News from '@/pages/News.vue'
import CTA from '@/pages/CTA.vue'
import Footer from '@/components/Footer.vue'
</script>

